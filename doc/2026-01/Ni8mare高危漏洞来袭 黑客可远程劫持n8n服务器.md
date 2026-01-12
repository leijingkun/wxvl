#  Ni8mare高危漏洞来袭 黑客可远程劫持n8n服务器  
胡金鱼  嘶吼专业版   2026-01-12 06:01  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
  
研究人员最新发现，一项被命名为Ni8mare的最高严重级漏洞，允许远程未授权攻击者完全接管本地部署的N8N工作流自动化平台。  
  
该安全漏洞编号为CVE-2026-21858，CVSS评分高达10分。据研究人员称，目前互联网上存在超过10万台易受攻击的N8N服务器。  
  
N8N是一款开源工作流自动化工具，用户可通过可视化编辑器将各类应用、API及服务连接成复杂的工作流。它主要用于任务自动化，并支持与人工智能及大语言模型服务的集成。  
  
该工具在npm上的周下载量超过5万次，在Docker Hub上的拉取量更是突破1亿次。作为AI领域的热门工具，它常被用于编排LLM调用、构建AI智能体（Agent）和检索增强生成（RAG）流水线，以及自动化数据摄入与检索。  
# Ni8mare漏洞技术细节分析  
  
Ni8mare漏洞允许攻击者通过执行特定的基于表单的工作流，访问底层服务器上的文件。   
  
N8N开发者表示：“存在漏洞的工作流可能会授予未授权远程攻击者访问权限。这可能导致存储在系统上的敏感信息泄露，并且根据部署配置和工作流的使用情况，可能会导致进一步的入侵。”  
  
研究人员于2025年11月发现了该漏洞并向N8N官方报告。他们指出，该漏洞源于N8N在解析数据时存在的 内容类型混淆问题。   
  
N8N使用两个不同的函数来处理传入数据，具体取决于Webhook中配置的content-type头部信息——Webhook是通过监听特定消息来触发工作流事件的组件。  
  
当Webhook请求被标记为multipart/form-data时，N8N会将其视为文件上传，并使用特殊的上传解析器，将文件保存在随机生成的临时位置。  
  
这意味着用户无法控制文件的最终存放路径，从而防范了路径遍历攻击。然而，对于所有其他内容类型，N8N则使用标准解析器。  
  
攻击者可以通过设置不同的内容类型（例如 application/json）来绕过上传解析器。在这种情况下，N8N仍会处理与文件相关的字段，但不会验证请求中是否真的包含有效的文件上传。这使得攻击者能够完全控制文件元数据，包括文件路径。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibHRmhnhsTVK1YGXhRGCkAUNWzic8Xuh9334bJnWFoJFrJSoCTictHibslicc6iaEsRUbYTLm58icn4pNxw/640?wx_fmt=png&from=appmsg "")  
  
存在问题的解析器逻辑  
  
研究员解释：由于调用该函数时未验证内容类型是否为 multipart/form-data，所以可以控制整个req.body.files对象。这意味着我们控制了 filepath`参数——因此，无需复制上传的文件，而是可以复制系统中的任何本地文件。  
  
这使得攻击者能够从N8N实例中读取任意文件，通过将内部文件添加到工作流的知识库中，从而泄露敏感信息。  
  
这一漏洞可被滥用于泄露存储在实例中的密钥、将敏感文件注入工作流、伪造会话Cookie以绕过身份验证，甚至执行任意命令。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibHRmhnhsTVK1YGXhRGCkAUreic0gqGflgUY7Jo3Ho1EuX8LLzRgfVdicVB4WWsnPlGWDiamtdv7h3KQ/640?wx_fmt=png&from=appmsg "")  
  
触发 Ni8mare访问数据库  
  
N8N通常存储着API密钥、OAuth令牌、数据库凭证、云存储访问权限、CI/CD密钥及业务数据，使其成为企业的核心自动化枢纽。  
  
N8N开发者表示，目前针对Ni8mare漏洞尚无官方临时缓解措施，但建议限制或禁用可公开访问的Webhook和表单端点。且强烈建议用户立即更新至N8N 1.121.0版本或更高版本。  
  
参考及来源：  
https://www.bleepingcomputer.com/news/security/max-severity-ni8mare-flaw-lets-hackers-hijack-n8n-servers/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibHRmhnhsTVK1YGXhRGCkAUFDVgicED9jSVQ2biaW4zqiavFPlrsBwvV8k3alU8ypvLl66bKiaMq3spzw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o2ibHRmhnhsTVK1YGXhRGCkAUeXGY30qvx1VdMwQYYicoOYuVa8lquUiciaYCv3icPIicwo4fc6swlGMVwgw/640?wx_fmt=png&from=appmsg "")  
  
  
