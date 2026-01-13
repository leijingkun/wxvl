#  SmarterMail邮件服务器任意文件上传漏洞  
安融技术  安融技术   2026-01-13 03:36  
  
SmarterMail  
是由  
‌  
SmarterTools Inc.  
‌  
开发的一款电子邮件服务器软件  
‌  
，首次发布于  
‌  
2003  
年  
‌  
，旨在为中小企业  
‌  
提供功能全面、成本效益高的电子邮件通信和协作解决方案  
‌  
。  
‌  
  
CVE-2025-52691  
是  
SmarterMail  
邮件服务器中存在的一个未授权任意文件上传漏洞，官方  
CVSS v3.1评分为10.0（严重等级）  
。该漏洞允许未经身份验证的攻击者将任意文件上传至邮件服务器的任意路径，进而实现远程代码执行（  
RCE  
），完全控制目标服务器。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB3aVJY0FUZITn891WBg50jfbNol8n6PCKuARfjcZK2To3N4NR44pqgJznolcqpqa8X55bJoa4Odg/640?wx_fmt=jpeg&from=appmsg "")  
  
  
漏洞成因  
  
根本原因：文件上传功能缺乏有效的安全校验机制，未对上传文件的类型、目标路径和内容进行验证。  
  
攻击向量：网络访问即可利用，无需身份认证或用户交互。  
  
利用方式：通过构造特殊的  
POST  
请求，利用路径遍历技术绕过限制，将恶意文件（如  
.aspx WebShell  
）上传至  
Web  
根目录。  
  
影响版本  
  
受影响版本：  
SmarterMail Build 9406  
及更早版本。  
  
安全版本：  
  
修复版本：  
Build 9413  
（  
2025  
年  
10  
月  
9  
日发布）。  
  
推荐版本：  
Build 9483  
（  
2025  
年  
12  
月  
18  
日发布）。  
  
主要受影响用户包括使用  
SmarterMail  
的虚拟主机服务商（如  
ASPnix  
、  
Hostek  
）及中小企业自建邮件系统。  
  
检测与验证  
  
版本检测方法  
  
向  
/interface/root#/login  
发送  
GET  
请求，检查响应中的  
 stProductVersion JavaScript  
变量：  
  
若内部版本号≤  
9406  
，则存在漏洞  
  
Censys  
探测规则  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCB3aVJY0FUZITn891WBg50jPcvurxCT2fibqpBnd8ZB6cLOKlhXPsJQYBCoD5J33ZOicW3LGb29gU6A/640?wx_fmt=jpeg&from=appmsg "")  
  
  
修复与缓解措施  
  
立即行动  
  
1.   
升级至安全版本：立即升级到  
 Build 9413   
或更高版本（推荐  
9483  
）。  
  
临时缓解（无法立即升级时）  
  
1.   
网络层防护：  
  
限制  
SmarterMail  
的互联网访问，仅允许可信  
IP  
连接。  
  
部署  
WAF  
拦截可疑文件上传请求。  
  
2.   
系统层加固：  
  
加强文件系统权限控制，限制邮件服务账户的写入权限。  
  
检查  
IIS  
日志中异常  
POST  
请求，排查已上传的后门文件。  
  
3.   
监控与响应：  
  
监控服务器上的可疑文件（特别是  
.aspx  
、  
.php  
、  
.dll  
文件）。  
  
检查邮件数据是否异常泄露。  
  
鉴于该漏洞无需认证、利用难度低、影响范围广，建议所有SmarterMail用户：  
  
1.   
立即自查：使用  
Censys  
规则或版本检测方法确认资产受影响情况。  
  
2.   
紧急升级：  
24  
小时内完成版本升级至  
Build 9413+  
。  
  
3.   
持续监控：升级前后检查服务器是否存在可疑文件和后门。  
  
4.   
供应商沟通：如通过服务商使用  
SmarterMail  
，立即联系确认补丁状态。  
  
