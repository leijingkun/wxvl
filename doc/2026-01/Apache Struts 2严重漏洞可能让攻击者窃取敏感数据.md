#  Apache Struts 2严重漏洞可能让攻击者窃取敏感数据  
会杀毒的单反狗  军哥网络安全读报   2026-01-13 01:02  
  
**导****读**  
  
  
  
Apache Struts 2 中发现一个严重的 XML 外部实体 ( XXE ) 注入漏洞，这可能会使数百万个应用程序面临数据窃取和服务器入侵的风险。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaHMibVWsnAacULrt5heQO7Cb3GL9kibtEIGy1lKFIWRu8REYbfV8pT9RmHWT0I667QRxWAecF4IssicQ/640?wx_fmt=png&from=appmsg "")  
  
  
该漏洞编号为 CVE-2025-68493，影响多个版本的广泛使用的框架，需要开发人员和系统管理员立即采取行动。  
  
  
该安全漏洞存在于 Apache Struts 2 的 XWork 组件中，该组件负责处理XML 配置解析。该组件无法正确验证 XML 输入，使应用程序容易受到 XXE 注入攻击。  
  
  
攻击者可以利用此漏洞访问存储在受影响服务器上的敏感信息，或发起拒绝服务攻击。ZAST.AI 的安全研究人员发现了该漏洞，并将其报告给了 Apache Struts 团队。  
  
  
该漏洞可能对数据机密性和系统可用性造成影响，被评为“重要”等级。  
  
  
该漏洞会影响全球各组织目前使用的各种 Struts 2 版本。运行这些版本中的任何组织都应立即优先进行安全更新。  
  
  
成功利用 CVE-2025-68493 可能导致数据泄露、服务器端请求伪造（  
SSRF  
）和拒绝服务。  
  
  
Apache 已发布Struts 6.1.1 作为修复版本。各组织应立即升级到此版本。该补丁保持了向后兼容性，确保顺利部署，不会破坏现有应用程序。  
  
  
官方安全公告：  
  
https://cwiki.apache.org/confluence/display/WW/S2-069  
  
  
新闻链接：  
  
https://gbhackers.com/critical-apache-struts-2-flaw/  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/AnRWZJZfVaGC3gsJClsh4Fia0icylyBEnBywibdbkrLLzmpibfdnf5wNYzEUq2GpzfedMKUjlLJQ4uwxAFWLzHhPFQ/640?wx_fmt=jpeg "")  
  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
  
