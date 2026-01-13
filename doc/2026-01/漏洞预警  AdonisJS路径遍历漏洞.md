#  漏洞预警 | AdonisJS路径遍历漏洞  
浅安  浅安安全   2026-01-13 00:02  
  
**0x00 漏洞编号**  
- CVE-  
2026-21440  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
AdonisJS是一个基于Node.js的全栈Web应用框架。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SUqHSL7bD1aLP0RwyFjWNtUjDo4EePnIFAWUOGa0rxNTZjJQOtrPicgE4jexURVHLbc2yoDjgAKt4A/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2026-21440**  
  
**漏洞类型：**  
**路径遍历**  
  
**影响：**  
**远程代码执行**  
  
**简述：AdonisJS的@adonisjs/bodyparser包存在路径遍历漏洞，攻击者可通过构造恶意文件名，利用MultipartFile.move的默认选项，将文件写入服务器任意位置，从而导致远程代码执行。**  
  
**0x04 影响版本**  
- @adonisjs/bodyparser <= 10.1.1  
  
- @adonisjs/bodyparser <= 11.0.0-next.5  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
******目前官方已发布漏洞修复版本，建议用户升级到安全版本****：******  
  
https://github.com/adonisjs/bodyparser/  
  
  
  
