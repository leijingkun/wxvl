#  n8n 满分漏洞 Ni8mare 可导致服务器遭劫持  
Bill Toulas  代码卫士   2026-01-08 10:03  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**n8n****中存在一个CVSS满分、被称为 “Ni8mare” 的严重漏洞CVE-2026-21858，可导致未认证攻击者控制本地部署的工作流自动化平台n8n 实例。**  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
网络安全公司 Cyera 表示，目前存在超过10万台易受攻击的 n8n 服务器。n8n 是一款开源的工作流自动化工具，可使用户将应用、API和服务通过一款可视化编辑器连接到复杂的工作流。该工具主要用于自动化任务，支持与AI和LLM 服务的集成。N8n 在 npm 上的周下载量超过5万次，在 Docker Hub 上的拉取超过1亿次。该工具在AI 领域非常流行，用于协同 LLM 调用、构建 AI 智能体和 RAG 管道，并自动化数据的采集和检索。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/oBANLWYScMSHZBiaVPjVHlxOEE8EGvBrPqzVFEeJ5VmHkJGh3ZcIjgvX78GVmOn4nN3jf7wZ4k8JlYo4ATPewkw/640?wx_fmt=gif&from=appmsg "")  
  
**漏洞详情**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/oBANLWYScMSHZBiaVPjVHlxOEE8EGvBrPqzVFEeJ5VmHkJGh3ZcIjgvX78GVmOn4nN3jf7wZ4k8JlYo4ATPewkw/640?wx_fmt=gif&from=appmsg "")  
  
  
  
该漏洞可使攻击者通过执行特定的表单驱动工作流，访问底层服务器上的文件。n8n开发团队表示：“存在漏洞的工作流可能允许未经身份验证的远程攻击者进行访问。根据部署配置和工作流使用情况，该漏洞可能导致系统中存储的敏感信息遭泄露，并可能引发进一步的危害。”  
  
 Cyera公司研究人员于2025年11月9日发现了该漏洞并向n8n报送。他们指出，该安全问题源于n8n解析数据时存在的内容类型混淆问题。n8n通过监听特定消息来触发工作流的Webhook组件，会依据其配置的“content-type”头部，使用两种不同函数处理传入数据。当Webhook请求标记为multipart/form-data 时，n8n会将其视为文件上传，并调用专用的上传解析器。该解析器会将文件保存到随机生成的临时位置中，这意味着用户无法控制文件最终存储路径，从而防止了路径遍历攻击。然而，对于所有其它内容类型（如application/json），n8n则会改用其标准解析器进行处理。  
  
研究人员发现，攻击者通过设置不同的内容类型（例如application/json），即可绕过上传解析器。在这种情况下，n8n仍会处理与文件相关的字段，但不会验证该请求是否实际包含有效的文件上传，从而导致攻击者能够完全控制文件元数据，包括文件路径，从而可能实现恶意文件写入或路径操纵。  
  
研究人员解释道：“由于调用此函数时未验证内容类型是否为multipart/form-data，我们可完全控制req.body.files对象。这意味着我们能操纵filepath参数——无需复制上传的文件，而是复制系统中的任意本地文件。”  
  
该漏洞可用于从n8n实例中读取任意文件，攻击者可通过将内部文件注入工作流知识库暴露敏感信息。研究人员指出，攻击者可借此窃取实例存储的密钥、向工作流注入敏感文件、伪造会话cookie绕过身份验证，甚至执行任意命令。   
  
研究人员强调称，n8n作为核心自动化枢纽，常存储API密钥、OAuth令牌、数据库凭据、云存储访问权限、CI/CD机密及商业数据等关键信息。n8n开发团队表示，目前不存在官方临时解决方案，但可通过限制或禁用公开访问的Webhook和表单端点作为缓解措施。强烈建议用户升级至n8n 1.121.0或更高版本，彻底修复该漏洞。  
  
  
  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[n8n严重漏洞可导致任意代码执行](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524734&idx=1&sn=7accfa41ad8e25a3c0a292eb451552af&scene=21#wechat_redirect)  
  
  
[欧洲空间局证实“外部服务器”遭攻陷](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524774&idx=1&sn=b7f1ee58a6471570b261a95d96541d42&scene=21#wechat_redirect)  
  
  
[SmarterMail 严重漏洞可导致服务器遭完全接管](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524768&idx=1&sn=9637d0eb4df20ac66e8c9fa5c1d84ddd&scene=21#wechat_redirect)  
  
  
[Red Hat服务器受陷，日产1.2万名客户数据遭泄露](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524719&idx=2&sn=f5d89f84af3cc83b347bda7d39e298df&scene=21#wechat_redirect)  
  
  
[CISA和NSA分享如何保护微软 Exchange 服务器](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524314&idx=1&sn=2ea2b06eca0631b008e170460b6c4c39&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://www.bleepingcomputer.com/news/security/max-severity-ni8mare-flaw-lets-hackers-hijack-n8n-servers/  
  
  
题图：Pixa  
bay Licens  
e  
  
  
**本文由奇安信编译，不代表奇安信观点。转载请注明“转自奇安信代码卫士 https://codesafe.qianxin.com”。**  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSf7nNLWrJL6dkJp7RB8Kl4zxU9ibnQjuvo4VoZ5ic9Q91K3WshWzqEybcroVEOQpgYfx1uYgwJhlFQ/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSN5sfviaCuvYQccJZlrr64sRlvcbdWjDic9mPQ8mBBFDCKP6VibiaNE1kDVuoIOiaIVRoTjSsSftGC8gw/640?wx_fmt=jpeg "")  
  
**奇安信代码卫士 (codesafe)**  
  
国内首个专注于软件开发安全的产品线。  
  
   ![](https://mmbiz.qpic.cn/mmbiz_gif/oBANLWYScMQ5iciaeKS21icDIWSVd0M9zEhicFK0rbCJOrgpc09iaH6nvqvsIdckDfxH2K4tu9CvPJgSf7XhGHJwVyQ/640?wx_fmt=gif "")  
  
   
觉得不错，就点个 “  
在看  
” 或 "  
赞  
” 吧~  
  
