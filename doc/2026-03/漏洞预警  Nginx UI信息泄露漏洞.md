#  漏洞预警 | Nginx UI信息泄露漏洞  
浅安
                    浅安  浅安安全   2026-03-11 00:01  
  
**0x00 漏洞编号**  
- # CVE-2026-27944  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
Nginx UI是一款基于Web的图形化管理工具，旨在简化Nginx服务器的配置、管理和监控。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/NQlfTO30MhyLrmHFy3ibsCh1LtXnXm7fkXiaudS1tCLxPpWdTP1maR82XXI5BU8zswdp0YPJznw0GFvLPDdVXXGqyXXa2JxEpD90NZ5ZZlmTM/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
CVE-2026-27944  
  
漏洞类型：  
信息泄露  
  
**影响：**  
获取敏感信息  
  
**简述：**  
Nginx UI存在信息泄露漏洞，由于其/api/backup端点无需身份验证即可访问，并在X-Backup-Security响应头中泄露了解密备份所需的加密密钥，攻击者通过该漏洞能够下载服务器敏感数据的完整系统备份并解密。  
  
**0x04 影响版本**  
- Nginx UI < 2.3.3  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://github.com/0xJacky/nginx-ui  
  
  
  
