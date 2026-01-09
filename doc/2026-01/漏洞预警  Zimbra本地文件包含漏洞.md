#  漏洞预警 | Zimbra本地文件包含漏洞  
浅安  浅安安全   2026-01-08 23:50  
  
**0x00 漏洞编号**  
- # CVE-2025-68645  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
Zimbra Collaboration Suite是一款功能强大的开源企业级电子邮件与协作平台。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SW6US95gq5OXwOKiaFNgP0Oh5xGuolBBw0rAqz1qia3XUUz5eCfyQa7Ldwgx1CjwqlPI9Bzl0fyRmkw/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-68645**  
  
**漏洞类型：**  
本地文件包含****  
  
**影响：执获取敏感文件******  
  
**简述：**  
Zimbra存在本地文件包含漏洞，由于其RestFilter servlet对请求参数过滤不严格，未经身份验证的远程攻击者可以构造恶意路径参数，获取服务器WebRoot目录下的敏感文件。  
  
**0x04 影响版本**  
- Zimbra Collaboration Suite 10.0 < 10.0.18  
  
- Zimbra Collaboration Suite 10.1 < 10.1.13  
  
**0x05****POC状态**  
- 未公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://wiki.zimbra.com/  
  
  
  
