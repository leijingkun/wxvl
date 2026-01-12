#  漏洞预警 | SmarterMail文件上传漏洞  
浅安  浅安安全   2026-01-12 00:01  
  
**0x00 漏洞编号**  
- # CVE-2025-52691  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
SmarterMail是SmarterTools公司推出的一款基于Windows平台的邮件服务器软件，支持SMTP、POP3、IMAP及WebMail等核心邮件功能，广泛应用于中小企业和自建邮件系统场景。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SWMH7URGvicoyl4Cvkc3uYgLb4moyaeBMugRStF77v5iclpOHl48y0DuxOeAKZ6FXyXm2PnNnlwGQBw/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-52691**  
  
**漏洞类型：**  
文件上传****  
  
**影响：**  
上传恶意文件  
  
  
****  
  
**简述：**  
SmarterMail存在文件上传漏洞，由于其对文件上传过程中的安全校验不足，未经身份认证的攻击者可通过该漏洞向邮件服务器任意路径上传恶意文件，进而执行任意代码，完全控制服务器。  
  
**0x04 影响版本**  
- SmarterMail <= 9406  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.smartertools.com/  
  
  
  
