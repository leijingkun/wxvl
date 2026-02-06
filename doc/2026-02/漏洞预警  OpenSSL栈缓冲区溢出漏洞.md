#  漏洞预警 | OpenSSL栈缓冲区溢出漏洞  
浅安
                    浅安  浅安安全   2026-02-06 00:02  
  
**0x00 漏洞编号**  
- # CVE-2025-15467  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
OpenSSL是一个广泛使用的开源加密库，提供实现安全通信协议的工具和库，支持多种加密算法。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXgMRjiaD4aHaOzbCiaGypMy5IJPGbZ5sejgX9AEIcx3Ub0gChwzCL6EAAuBeFVYW7SLvUxberibQ32g/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-15467**  
  
**漏洞类型：**  
缓冲区溢出  
  
**影响：**  
程序崩溃  
  
****  
  
**简述：**  
OpenSSL存在栈缓冲区溢出漏洞，由于其在处理使用AEAD加密算法的消息时，未对ASN.1参数中的初始化向量长度进行校验，未经验证的攻击者可以构造特制的恶意CMS消息，利用超长IV值触发栈溢出，从而引发拒绝服务攻击，甚至在某些情况下，攻击者可通过栈溢出实现远程代码执行。  
  
**0x04 影响版本**  
- OpenSSL 3.6  
  
- OpenSSL 3.5  
  
- OpenSSL 3.4  
  
- OpenSSL 3.3  
  
- OpenSSL 3.0  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.openssl.org/  
  
  
  
