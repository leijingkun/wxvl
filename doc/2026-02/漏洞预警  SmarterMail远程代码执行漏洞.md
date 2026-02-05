#  漏洞预警 | SmarterMail远程代码执行漏洞  
浅安
                    浅安  浅安安全   2026-02-05 00:01  
  
**0x00 漏洞编号**  
- # CVE-2026-24423  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
SmarterMail是SmarterTools公司推出的一款基于Windows平台的邮件服务器软件，支持SMTP、POP3、IMAP及WebMail等核心邮件功能，广泛应用于中小企业和自建邮件系统场景。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SWMH7URGvicoyl4Cvkc3uYgLb4moyaeBMugRStF77v5iclpOHl48y0DuxOeAKZ6FXyXm2PnNnlwGQBw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2026-24423**  
  
**漏洞类型：**  
远程代码执行****  
  
**影响：**  
执行任意命令  
  
  
****  
  
**简述：**  
SmarterTools SmarterMail存在远程代码执行漏洞，由于其ConnectToHub API接口未对访问者进行身份验证。攻击者可通过构造特定请求，诱导SmarterMail服务器指向恶意HTTP服务器，进而向SmarterMail返回恶意操作系统命令。攻击者可以远程执行任意命令，完全控制受影响的系统，导致数据泄露、服务中断等严重后果。  
  
**0x04 影响版本**  
- SmarterMail < Build 9511  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.smartertools.com/  
  
  
  
