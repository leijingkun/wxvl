#  WatchGuard 防火墙发现严重远程代码执行漏洞  
Rhinoer  犀牛安全   2026-01-09 16:00  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qvpgicaewUBmMMwaqibcJJpIx8sVQHD98icPMPcZAzwI27UOtiaibdEtM4QDunMQpT59GoK9xft56PLyYf0jR8cOygQ/640?wx_fmt=png&from=appmsg "")  
  
超过 115,000 台暴露在外的 WatchGuard Firebox 设备仍未修补针对严重远程代码执行 (RCE) 漏洞的补丁，该漏洞已被攻击者积极利用。  
  
该安全漏洞编号为CVE-2025-14733，影响运行 Fireware OS 11.x 及更高版本（包括 11.12.4_Update1）、12.x 或更高版本（包括 12.11.5）以及 2025.1 至 2025.1.3 的 Firebox 防火墙。  
  
成功利用漏洞后，未经身份验证的攻击者可以远程在易受攻击的设备上执行任意代码，进行无需用户交互的低复杂度攻击。  
  
正如 WatchGuard 在周四发布的安全公告中所解释的那样，当其发布 CVE-2025-14733 安全更新并将其标记为已被实际利用时，未打补丁的 Firebox 防火墙只有在配置了 IKEv2 VPN 的情况下才会受到攻击。WatchGuard 还警告说，即使移除了存在漏洞的配置，如果仍然配置了到静态网关对等体的分支机构 VPN (BOVPN)，防火墙仍然可能面临风险。  
  
NVD发布的一份安全公告解释说：“WatchGuard Fireware OS iked进程存在越界写入漏洞。该漏洞可能允许未经身份验证的远程攻击者执行任意代码，并且会影响使用IKEv2的移动用户VPN以及配置了动态网关对等体的分支机构VPN。”  
  
WatchGuard 已分享入侵指标，帮助客户识别网络中受感染的 Firebox 设备，并建议发现恶意活动迹象的用户轮换易受攻击防火墙上所有本地存储的密钥。此外，对于无法立即修补易受攻击设备的网络防御人员，WatchGuard 还提供了一种临时解决方案：禁用动态对等 BOVPN、添加新的防火墙策略，并禁用处理 VPN 流量的默认系统策略。  
  
周六，互联网安全监督组织 Shadowserver 发现超过 124,658 个未打补丁的 Firebox 实例暴露在网上，截至周日仍有 117,490 个实例暴露在外。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qvpgicaewUBmMMwaqibcJJpIx8sVQHD98iciaLpib9EKAYu5aWq0ySkUptLy21XwXO6IMv3VibjKicL2v986EJo0zVG2g/640?wx_fmt=png&from=appmsg "")  
  
WatchGuard 发布补丁一天后，CISA将CVE-2025-14733添加到其已知利用漏洞 (KEV) 目录中。  
  
美国网络安全机构还命令联邦民事行政部门 (FCEB) 机构（行政部门非军事机构，例如能源部、财政部和国土安全部）在一周内，即 12 月 26 日之前，按照具有约束力的操作指令 (BOD) 22-01 的规定，修补 Firebox 防火墙。  
  
此类漏洞是恶意网络攻击者常用的攻击途径，对联邦机构构成重大风险，”CISA警告说。“请按照供应商说明采取缓解措施，遵循适用于云服务的BOD 22-01指南，或者如果无法采取缓解措施，则停止使用该产品。  
  
9 月，WatchGuard修复了一个几乎完全相同的远程代码执行漏洞(CVE-2025-9242)，该漏洞影响 Firebox 防火墙。一个月后，Shadowserver发现超过 75,000 台 Firebox 防火墙存在 CVE-2025-9242 漏洞，其中大部分位于北美和欧洲。随后，美国网络安全和基础设施安全局 (CISA) 将该安全漏洞标记为已被积极利用，并命令联邦机构采取措施保护其 Firebox 设备免受持续攻击。  
  
两年前，CISA 还命令美国政府机构修复另一个正在被积极利用的 WatchGuard 漏洞（CVE-2022-23176），该漏洞影响 Firebox 和 XTM 防火墙设备。  
  
WatchGuard 与超过 17,000 家安全经销商和服务提供商合作，为全球超过 250,000 家中小企业提供网络安全保护。  
  
  
信息来源 ：  
BleepingComputer  
  
