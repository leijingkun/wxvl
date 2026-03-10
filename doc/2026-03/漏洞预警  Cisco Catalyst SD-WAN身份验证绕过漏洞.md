#  漏洞预警 | Cisco Catalyst SD-WAN身份验证绕过漏洞  
浅安
                    浅安  浅安安全   2026-03-09 23:51  
  
**0x00 漏洞编号**  
- # CVE-2026-20127  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
Cisco Catalyst SD-WAN是思科推出的企业级软件定义广域网解决方案，旨在通过集中式管理和自动化功能简化分支机构网络部署。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SU7yiaA9KHbxNq1iaCtatkAMRdMxh9GQW5qsybAMVCfgV1icAvzV2Y7tKZDV9WTF3l0dMcaKaNR0jllQ/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=0 "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2026-20127**  
  
**漏洞类型：**  
身份验证绕过  
  
**影响：**  
越权访问  
  
**简述：**  
Cisco Catalyst SD-WAN存在身份验证绕过漏洞，由于其对等身份验证逻辑的实现错误，未经身份验证的攻击者可利用该漏洞向受影响系统发送精心构造的请求，绕过身份验证机制获得高权限的非root访问权限，进而访问NETCONF接口，对SD-WAN网络架构的配置进行任意篡改，严重破坏网络的完整性和可用性。  
  
**0x04 影响版本**  
- Catalyst SD-WAN < 20.9  
  
- Catalyst SD-WAN 20.9 < 20.9.8.2  
  
- Catalyst SD-WAN 20.11 < 20.12.6.1  
  
- Catalyst SD-WAN 20.12.5 < 20.12.5.3  
  
- Catalyst SD-WAN 20.12.6 < 20.12.6.1  
  
- Catalyst SD-WAN 20.13 < 20.15.4.2  
  
- Catalyst SD-WAN 20.14 < 20.15.4.2  
  
- Catalyst SD-WAN 20.15 < 20.15.4.2  
  
- Catalyst SD-WAN 20.16 < 20.18.2.1  
  
- Catalyst SD-WAN 20.18 < 20.18.2.1  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.cisco.com/  
  
  
  
