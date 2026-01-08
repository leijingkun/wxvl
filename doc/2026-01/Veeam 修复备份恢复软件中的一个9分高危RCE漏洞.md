#  Veeam 修复备份恢复软件中的一个9分高危RCE漏洞  
会杀毒的单反狗  军哥网络安全读报   2026-01-08 03:11  
  
**导****读**  
  
  
  
Veeam 发布了针对多个备份和复制软件漏洞的补丁，其中包括一个编号为 CVE-2025-59470（CVSS 评分为 9.0）的严重远程代码执行漏洞。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaEpjtYj1pVRHj12wukB8OVpd20Ooy8obJWIreUxhvIEicaJHViaM1ttayleLBwELGVoGGPzLBjBzFDA/640?wx_fmt=png&from=appmsg "")  
  
  
备份或磁带操作员可以通过滥用恶意间隔或顺序参数，以 postgres 用户身份实现远程代码执行。  
  
  
postgres “此漏洞允许备份或磁带操作员通过发送恶意间隔或顺序参数，以用户身份执行远程代码执行 (RCE)  
   
。”安全公告中写道。  
  
  
Veeam 磁带操作员是 Veeam Backup & Replication 的一个受限用户角色，旨在管理基于磁带的备份操作，而无需完整的管理权限。  
  
  
该漏洞是在内部测试中发现的。  
  
  
供应商表示，备份和磁带操作员角色拥有很高的权限，遵循安全准则可以降低漏洞利用的可能性，因此该问题被降级为高严重性级别。  
  
  
Veeam 还修复了三个漏洞：通过恶意备份以 root 身份进行的远程代码执行 (CVE-2025-55125，CVSS 评分为 7.2)、通过密码以 postgres 身份进行的远程代码执行 (CVE-2025-59468，CVSS 评分为 6.7) 以及以 root 身份写入文件 (CVE-2025-59469，CVSS 评分为 7.2)。  
  
  
Veeam Backup & Replication 13.0.1.1071解决了这些漏洞。  
  
  
目前尚不清楚上述缺陷之一是否已被用于实际攻击中。  
  
  
2025 年 3 月，该供应商解决了一个严重漏洞，编号为 CVE-2025-23120（CVSS 评分为 9.9），该漏洞影响其备份和复制软件，可能导致远程代码执行。  
  
  
新闻链接：  
  
https://securityaffairs.com/186630/security/veeam-resolves-cvss-9-0-rce-flaw-and-other-security-issues.html  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/AnRWZJZfVaGC3gsJClsh4Fia0icylyBEnBywibdbkrLLzmpibfdnf5wNYzEUq2GpzfedMKUjlLJQ4uwxAFWLzHhPFQ/640?wx_fmt=jpeg "")  
  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
  
