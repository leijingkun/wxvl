#  n8n远程代码执行高危漏洞-最高严重性漏洞  
安融技术  安融技术   2026-01-10 02:16  
  
n8n  
是一款开源的低代码工作流自动化工具，允许用户通过可视化界面连接不同服务（如  
 Slack  
、  
Google Sheets  
、  
API   
等）构建自动化流程。其核心功能包括节点式工作流编排、表达式语言支持、自定义函数扩展等，广泛用于  
 DevOps  
、业务流程自动化和集成场景。  
n8n   
可部署于本地服务器、云环境或  
 Docker   
容器中，支持社区版与企业版。  
  
n8n远程代码执行漏洞：编号CVE-2026-21858。是2026年1月披露的一个最高严重性漏洞  
，代号为  
 Ni8mare  
，影响广受欢迎的工作流自动化平台  
n8n  
。该漏洞  
CVSS  
评分高达  
 10.0  
，允许未认证攻击者完全接管受影响实例。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfznF3Xz8kwl8Rv6V6juL1Q5RmzkibtkMTHOf9nibNGumMUbnwVkCrdFpL8Q/640?wx_fmt=jpeg&from=appmsg "")  
  
  
漏洞类型：  
内容类型混淆 → 任意文件读取 → 远程代码执行（  
RCE  
）  
  
披露时间：  
2026  
年  
1  
月  
7  
日  
  
影响范围：  
n8n  
自托管实例版本  
 < 1.121.0  
  
技术原理  
  
攻击路径：  
  
该漏洞是一个两阶段利用链，通过内容类型混淆实现任意文件读取，进而提升为完全  
RCE  
：  
  
第一阶段：任意文件读取  
  
根源：  
Form Webhook  
节点未验证请求的  
 Content-Type   
头是否确实为  
 multipart/form-data  
  
正常流程：  
  
multipart/form-data  
请求 →  
 parseFormData()   
→ 文件安全存储到临时目录 →  
req.body.files  
包含合法文件信息  
  
攻击流程：  
  
application/json  
请求 →  
 parseBody()   
→ 直接填充  
req.body   
→ 攻击者手动构造  
req.body.files  
字段 → 欺骗文件处理函数  
  
Payload  
示例：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfznHYRs9MEUTNsX1zJKloU5r6h7zaUR9Y0J960nia2geCbibqUKSn5UL1DQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
当  
Form Webhook  
节点处理此请求时，会将  
 /etc/passwd   
当作上传文件处理，并将其内容传递给下游节点。  
  
第二阶段：提升至RCE  
  
攻击者利用文件读取能力窃取关键数据：  
  
1.   
窃取  
SQLite  
数据库：  
 ~/.n8n/n8n.sqlite   
（包含用户记录，包括密码哈希）  
  
2.   
窃取配置文件：  
 ~/.n8n/config   
（包含会话签名密钥）  
  
会话  
Cookie  
伪造：  
 n8n  
使用  
 n8n-auth cookie  
进行认证，结构为：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfzn6RKWz5Tib4AIvbP3PchAvJRukicaB8pNWwwlP9GKFias6vsrLZTtKtiaew/640?wx_fmt=jpeg&from=appmsg "")  
  
  
并使用本地密钥签名。攻击者获得数据库和密钥后，可伪造管理员会话。  
  
3.  
   
执行命令：以管理员身份创建包含  
"Execute Command"  
节点的工作流，实现  
RCE  
。  
  
潜在危害  
  
完全系统接管：攻击者以  
n8n  
进程权限执行任意命令  
  
敏感数据泄露：  
API  
密钥、  
OAuth  
令牌、数据库凭证  
  
横向渗透：通过  
n8n  
中存储的凭证攻击其他系统  
  
供应链攻击：篡改工作流注入恶意代码  
  
影响范围巨大：  
n8n  
集中存储了所有集成凭证，一旦沦陷，等于  
"  
将攻击所有连接系统的钥匙交给了攻击者。  
  
修复方案  
  
1. 立即升级（唯一完整解决方案）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfznBibR64MIVDFwSVTmFnFSBI0xTsyKiaIkiaIWQYEW0yuLkwFOAfOickiavIg/640?wx_fmt=jpeg&from=appmsg "")  
  
2. 临时缓解措施（无法立即升级时）  
  
A.   
网络层防护  
  
WAF  
规则：阻止包含  
 files   
字段的非  
 multipart/form-data   
请求  
  
反向代理过滤：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfzny6jPGe0tExaPVibKNTJzWPsONY8icibFiaYQGGdyR0uANzMlEnfz2YGUUw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
B.   
访问控制  
  
禁用  
Form Webhook  
节点：如无必要，移除或禁用  
  
IP  
白名单：仅允许可信  
IP  
访问  
webhook  
端点  
  
认证前置：在反向代理层添加  
Basic Auth  
  
C.   
监控告警  
  
检测异常文件访问模式：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfznEZdUhXcyyb5Vo62zXvOiboxBy9QIsicENtnB7oUy1qkNBPaRQhKHukpw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
自查与检测  
  
检测是否受影响  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfznAV4WSxDJQ8APTeuvmVvziaO9pTicyhNLZuTtF1GY7iaW3PLhz4xWp0KkA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
入侵迹象（IoC）  
  
异常文件读取操作（  
/etc/passwd , /proc/self/environ  
）。  
  
n8n-auth  
   
cookie  
频繁更换。  
  
新增的  
"Execute Command"  
节点或修改现有工作流。  
  
异常出站连接（反向  
Shell  
）。  
  
数据库中新增管理员账户。  
  
漏洞利用特征  
  
Webhook  
请求：  
Content-Type  
非  
multipart  
但包含  
 files   
字段。  
  
请求  
IP  
：来自扫描器或  
Tor  
出口节点。  
  
响应内容：异常大的响应体（包含文件内容）。  
  
相关漏洞对比  
  
近期  
n8n  
连续披露多个严重漏洞，需注意区分：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB1g7UMxAZ1e3QJ08LrGfzncEbII1uUzH6nkicZWvHNLAQFRTpD9ThypU3XyzbNv0oM00icG1RcibevQ/640?wx_fmt=jpeg&from=appmsg "")  
  
