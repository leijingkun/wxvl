#  最高级别的 n8n 漏洞允许随机用户接管服务器  
会杀毒的单反狗  军哥网络安全读报   2026-01-09 01:03  
  
**导****读**  
  
  
  
流行的自动化平台 n8n 中存在一个最高级别的漏洞，导致估计有 10 万台服务器完全暴露在外，容易被完全控制。这个漏洞非常严重，甚至不需要登录。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaHPDzSialzBAnD554e0WZcem63dMgkpTfckS9XgEwD3SQhaRY9vibAlGe2uicrkCnDyhZJL1YhZP5L8A/640?wx_fmt=png&from=appmsg "")  
  
数据安全公司 Cyera 警告称，n8n 工作流自动化平台存在严重漏洞，攻击者可以利用该漏洞接管易受攻击的实例。  
  
  
n8n 是一款自托管的开源自动化工具，许多组织使用它来整合聊天应用、表单、云存储、数据库和第三方 API。据称，它已完成超过 1 亿次 Docker 镜像拉取，数百万用户和数千家公司使用它来自动化从内部工作流程到面向客户的流程等各种任务。  
  
  
该漏洞由安全公司 Cyera 的研究人员发现，CVSS 评分为 10.0，并被戏称为“ni8mare”（意为“噩梦”），名副其实。该漏洞编号为 CVE-2026-21858，允许未经身份验证的攻击者在易受攻击的系统上执行任意代码，从而完全控制受影响的环境。  
  
  
目前除了打补丁之外没有其他解决方法，强烈建议用户升级到 n8n 1.121.0 或更高版本。  
  
  
n8n 的安全公告指出： “n8n 的一个漏洞允许攻击者通过执行某些基于表单的工作流来访问底层服务器上的文件。存在漏洞的工作流可能会向未经身份验证的远程攻击者授予访问权限。”  
  
  
据 Cyera Research Labs 的研究员 Dor Attias 称，他发现了这个漏洞并将其命名为Ni8mare，该问题是内容类型混淆，导致当攻击者更改内容类型时，n8n 调用错误的解析器。  
  
  
由于将文件从临时文件复制到持久存储的函数在调用时没有验证内容类型，攻击者可以控制文件路径参数，复制任何本地文件而不是上传的文件。  
  
  
Attias解释说，这种安全缺陷可能允许攻击者提取敏感信息，并利用这些信息完全控制n8n实例。  
  
  
他首先拦截了使用表单节点上传文件时发送的 HTTP 请求，表单节点是允许用户与工作流交互的界面。  
  
  
接下来，Attias 更改了内容类型并编写了请求正文来控制文件路径，从而使他能够将内部“passwd”文件加载到组织知识库中。  
  
  
他指出：“要获取该内部文件的内容，我们只需要通过聊天界面询问即可。”  
  
Attias表示，该漏洞还可以被进一步利用来执行代码。  
  
  
攻击者可以触发该漏洞，加载 n8n 的整个数据库及其配置文件，从而获取敏感信息，进而伪造会话 cookie 并以管理员身份登录。之后，他们只需创建一个新的工作流即可执行命令。  
  
  
“一旦 n8n 系统遭到入侵，其影响范围将非常巨大。n8n 连接着无数系统，包括您组织的 Google 云端硬盘、OpenAI API 密钥、Salesforce 数据、IAM 系统、支付处理商、客户数据库、CI/CD 管道等等。”Attias 解释道。  
  
  
该漏洞已在 n8n 版本 1.121.0 中得到解决，该版本于 2025 年 11 月 18 日发布。  
  
  
所有面向互联网的 n8n 实例都有被完全接管的风险，应尽快进行修补，尤其是在 Cyera 发布了有关如何触发该漏洞的技术细节之后。  
  
  
n8n 指出：“目前没有官方的解决方法。作为临时缓解措施，用户可以在升级之前限制或禁用公开访问的 webhook 和表单端点。”  
  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaHPDzSialzBAnD554e0WZcemod7TfTnXOj8U3nibY2xuvk8QqNibBS2NOtmxHxQiaUQx0bsSYxFTjBVmQ/640?wx_fmt=png&from=appmsg "")  
  
  
漏洞详细分析：  
  
《Ni8mare - n8n 中的未经身份验证的远程代码执行漏洞 (CVE-2026-21858)》  
  
https://www.cyera.com/research-labs/ni8mare-unauthenticated-remote-code-execution-in-n8n-cve-2026-21858  
  
  
新闻链接：  
  
https://www.securityweek.com/critical-vulnerability-exposes-n8n-instances-to-takeover-attacks/  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/AnRWZJZfVaGC3gsJClsh4Fia0icylyBEnBywibdbkrLLzmpibfdnf5wNYzEUq2GpzfedMKUjlLJQ4uwxAFWLzHhPFQ/640?wx_fmt=jpeg "")  
  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
  
