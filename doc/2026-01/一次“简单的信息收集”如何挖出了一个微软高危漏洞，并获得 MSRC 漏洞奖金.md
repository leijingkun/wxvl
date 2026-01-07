#  一次“简单的信息收集”如何挖出了一个微软高危漏洞，并获得 MSRC 漏洞奖金  
haidragon  安全狗的自我修养   2026-01-07 04:17  
  
##   
# 官网：http://securitytech.cc/  
  
  
一次深夜的信息收集、一个随机的 .txt 文件，以及纯粹的好奇心，最终变成了一份高危安全报告，并获得了微软 MSRC 的公开致谢。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERGSzyW3b031xvXsmDRaRf6IrmqTaOoZoAOvNCtyvViaa2us19SqKlj3HD9ibES8icyx1onHeFAmRQTAg/640?wx_fmt=png&from=appmsg "")  
  
当世界上大多数人早已入睡、做梦或准备休息时，我真正的工作才刚刚开始。**午夜不仅仅是一个时间点，它是最完美的时刻**  
：城市安静下来，思维变得清晰，所有杂音都消失了。正是在这样的时刻，我一点一点地追逐着成为安全研究员的梦想。  
  
就在这样一个专注的夜晚，我打开终端，准备做一次**“简单的信息收集（simple recon）”**。一切看起来都和平常没什么不同，直到某个瞬间，我的注意力被**  
一个随机的 .txt  
 文件**吸引住了。这个文件很小、极容易被忽略，却正是那根线头，最终牵扯出了一个 **微软的高危漏洞**  
。  
  
这就是一个关于安静坚持、深夜直觉，以及如何从一个不起眼的发现，走到获得 MSRC 公开致谢的故事——也证明了：**真正的回报，往往来自最微小、最专注的努力。**  
## 🛠️ 环境与背景：简单的信息收集，而非技术性利用  
  
这并不是什么硬核漏洞利用开发的场景。  
  
没有堆喷（heap spray），没有竞争条件（race condition），也没有什么“法师级”的 RCE 黑魔法。  
  
我当时只是做了一次非常常规的信息收集，包括：  
- 对目标进行广泛的域名映射，找出所有可访问的子域名  
  
- 收集和整理历史 URL，寻找曾经暴露过的接口  
  
- 过滤 **纯文本文件（.txt 扩展名）**  
，因为这类文件**极容易被人无意中遗留敏感信息**  
  
如果你在想：**“为什么是 .txt 文件？”**  
  
答案很简单：因为**人们最容易把秘密留在文本文件里**  
。我曾经就因为一个普通的 .txt  
 文件拿到过一个 **CVE**  
。那次经历让我明白了一点：**永远不要低估“只是文本而已”**  
。  
  
  
说实话，在那之前，我预期看到的只是一些无聊的文档或无害的配置示例。但我点开了其中一个文件。**就这一个**  
，一切都变了。  
## 😲 那个“等等……什么情况？”的瞬间  
  
我打开了一个托管在 **微软管理的附件服务器**  
 下的 URL（出于负责任披露的要求，具体地址已脱敏）。  
  
浏览器自动下载了一个 .txt  
 文件。  
**不需要登录、不需要认证、不需要预签名 URL**  
。  
  
就是——“啪”一下，文件就到你手里了。  
  
  
文件内容是**内部诊断数据**  
，那种你绝对不希望被公开访问的数据。  
  
那一刻，我心里只有一个想法：**“这是真的问题。”**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/vBZcZNVQERGSzyW3b031xvXsmDRaRf6IhzHcibHkRUEX9eibQBW7wiaazthg9yVdYC68O83xViaiasxgF9eGhPYGoxQ/640?wx_fmt=gif&from=appmsg "")  
## 🕵️ 进一步挖掘（但始终保持克制）  
  
我没有去“利用”任何东西：  
- 没有测试任何凭据  
  
- 没有触碰生产系统  
  
- 没有进行任何破坏性操作  
  
我做的，仅仅是验证这个问题是否**具有系统性**  
：  
- 确认**多个文件**  
遵循相同的附件模式，且均可被公开访问  
  
- 通过针对性的搜索引擎查询，确认这并非偶发事件，并定位到更多同域文件  
  
- 检查这些文件所包含的数据类型，并**对任何敏感内容进行脱敏处理**  
  
- 详细记录所有发现，并以**负责任的方式**  
提交给微软 MSRC  
  
好奇心很重要。  
  
但**好奇心 + 道德底线**  
，才是安全研究真正的立身之本。🔐  
## 🚨 暴露了哪些类型的文件？（高危）  
  
我不会公开任何真实文件名或内部标识，这里只给出**安全、概括性的说明**  
。  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERGSzyW3b031xvXsmDRaRf6IXVUD7ZQop4bIzMficz3HrASy2IVdvhf9pPucwzkFx40nMtxgK0XCd4Q/640?wx_fmt=png&from=appmsg "")  
  
**严重性：高（High）**  
。  
  
这并不仅仅是“日志泄露”这么简单。**凭据、身份基础设施信息、内部云架构**  
全部处于可公开访问状态，这是一次典型的**运营安全（OpSec）失败**  
。  
## 🗓️ 时间线（协调披露）  
> **2025 年 11 月 2 日**  
  
在一次常规信息收集中，发现微软管理的在线服务中存在可公开访问的文本附件。  
> **2025 年 11 月 2 日**  
  
向 Microsoft Security Response Center（MSRC）提交了详细、已脱敏的漏洞报告。（案件编号 [#103203]()  
）  
> **2025 年 11 月 7 日**  
  
MSRC 确认问题存在，并启动内部调查。  
> **2025 年 11 月中旬**  
  
微软限制了相关资源的公开访问，作为修复措施的一部分。  
> **2025 年 11 月 11 日**  
  
MSRC 确认该问题有效，并符合公开致谢条件，但在当时的政策下不在奖金范围内；由于不需要客户采取任何行动，未分配 CVE。  
> **2025 年 12 月 11 日**  
  
微软宣布更新 MSRC 漏洞奖金计划，扩大适用范围，并引入 90 天的追溯奖励窗口。  
> **2025 年 12 月 12 日**  
  
案件 [#103203]()  
 根据更新后的标准奖励政策重新评估，被定级为 **重要级（Important）信息泄露漏洞**  
。  
> **2025 年 12 月 12 日**  
  
MSRC 确认该漏洞符合追溯奖金发放条件。  
> **2025 年 12 月**  
  
研究者姓名被加入微软官方 **MSRC 致谢名单（在线服务）**  
 页面。  
  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERGSzyW3b031xvXsmDRaRf6I50zytxPDja7BZ8uHIdXOrsAVLCDp5U2icye0BoLwTZFHFuibH9j54ibDA/640?wx_fmt=png&from=appmsg "")  
  
在协调披露和修复完成后，我的名字 **（Sreejith A R）**  
 被正式加入微软官方 **MSRC Acknowledgments – Onli溯性漏洞奖金确认**  
  
在最初提交时，该问题根据当时的微软政策并不符合奖金条件。  
  
但在 **2025 年 12 月**  
 微软更新 MSRC 漏洞奖金计划、扩大范围并允许 **90 天追溯审查**  
 后，该案例被重新评估。  
## 📢 MSRC 漏洞奖金计划更新通知  
  
（使追溯奖励成为可能的政策更新）  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERGSzyW3b031xvXsmDRaRf6I9pdhWcdzPLnw1HneDDVThf1sjXbjYGadqak1PIPXGup6cAvplfpM5w/640?wx_fmt=png&from=appmsg "")  
  
随后，MSRC 确认该漏洞符合 **标准奖励政策（Standard Award Policy）**  
，影响的是微软第一方在线服务，安全影响为 **重要级别（Important）信息泄露**  
。  
## 🏆 追溯奖金确认  
  
（基于新政策重新评估的结果）  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERGSzyW3b031xvXsmDRaRf6Ih1hEQDzsNTxEaqMoedXUAjVKbmrQ6R2CzjQjJ3epTk5fEpHEhNx6MQ/640?wx_fmt=png&from=appmsg "")  
  
从个人角度来说，这份认可意义非凡——这是我的**第一笔漏洞奖金**  
，而且还是来自微软，这让它变得更加难忘。🎉  
  
我非常感谢 MSRC 团队在整个过程中展现出的透明度、响应速度，以及对负责任安全研究的认可与支持。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/vBZcZNVQERGSzyW3b031xvXsmDRaRf6I2iaRIibuoL8U5UF2zFOf4RC9zn83iaQ2ViccQ9XkudyWZELF9xfYBgicw2g/640?wx_fmt=gif&from=appmsg "")  
## 🔒没有做的事情（很重要）  
  
严格遵守负责任披露原则，我明确做到：  
- **没有**  
尝试使用或验证任何凭据  
  
- **没有**  
在任何系统上执行代码或修改内容  
  
- **没有**  
访问任何非公开的客户数据  
  
所有操作都仅限于对**公开可访问文件的被动发现**  
，并在上报前进行了脱敏处理。  
## ✨ 我的收获与思考  
1. **永远不要低估“简单的信息收集”**  
  
不需要复杂的漏洞链，真正重要的是耐心、好奇心和持续投入。  
  
1. **纯文本文件同样危险**  
  
日志、转储、脚本、导出文件中，往往藏着最敏感的秘密。  
  
1. **负责任披露比炫耀更重要**  
  
目标不是利用，而是修复。清晰、尊重、协同，才是正确路径。  
  
1. **关注模式，而不仅是单个页面**  
  
一个暴露的文件，往往意味着系统性访问控制失败。  
  
- 公众号:安全狗的自我修养  
  
- vx:2207344074  
  
- http://  
gitee.com/haidragon  
  
- http://  
github.com/haidragon  
  
- bilibili:haidragonx  
  
##   
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/vBZcZNVQEREmdic8DA3zxUsm3LkA13z2BKfZkMdibDHpwWyhGlUibEB4icbzwgBh6VkXArx1VZQUkpoph7BlJIRiaEQ/640?wxfrom=5&wx_fmt=jpg&watermark=1&wx_lazy=1&tp=webp#imgIndex=34 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERHYgfyicoHWcBVxH85UOBNaPZeRlpCaIfwnM0IM4vnVugkAyDFJlhe1Rkalbz0a282U9iaVU12iaEiahw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&randomid=z84f6pb5&tp=webp#imgIndex=5 "")  
  
****- ![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERHYgfyicoHWcBVxH85UOBNaPMJPjIWnCTP3EjrhOXhJsryIkR34mCwqetPF7aRmbhnxBbiaicS0rwu6w/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&randomid=omk5zkfc&tp=webp#imgIndex=5 "")  
  
