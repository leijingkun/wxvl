#  黑客利用0DAY漏洞利用工具包攻击 VMware ESXi 实例  
会杀毒的单反狗  军哥网络安全读报   2026-01-09 01:03  
  
**导****读**  
  
  
  
攻击者利用被入侵的 SonicWall VPN 来投放针对 VMware ESXi   
0day  
漏洞的工具包，这些漏洞可能在公开披露一年多之前就被利用了。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/AnRWZJZfVaHPDzSialzBAnD554e0WZcemL8nGKEonEa166LZ5hbWibwFSnKXu5kK5YLia5Jtn78U2ib1KPq8ibiaia2JA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
该攻击链包含一个复杂的虚拟机逃逸机制，并且似乎在相关 VMware 漏洞公开披露一年多之前就已经开发完成。  
  
  
对 2025 年 12 月观察到的攻击进行分析表明，该组织很早就知道三个后来在 2025 年 3 月披露的 ESXi   
0day  
漏洞，这表明他们长期以来一直在秘密利用这些漏洞。  
  
  
2025 年 12 月，Huntress 研究人员检测到一次入侵，导致 VMware ESXi 漏洞利用工具包的部署，最初的访问归因于被入侵的 SonicWall VPN。  
  
  
简体中文字符串和构建路径等证据表明，该工具包很可能是在 VMware 公开披露其缺陷一年多之前作为  
0day  
漏洞开发的，这表明攻击者拥有雄厚的资源。  
  
  
攻击者利用域管理员凭据进行横向移动，执行侦察任务，修改防火墙规则以阻止外部访问，同时保留内部流量，并预先准备数据以进行数据窃取。  
  
  
该工具包针对多达 155 个 ESXi 版本，并通过禁用 VMCI 驱动程序和未签名内核驱动程序实现虚拟机逃逸，这可能为勒索软件的攻击铺平道路。攻击最终在造成影响之前被阻止。  
  
  
VMware 于 2025 年 3 月发布的安全公告 VMSA-2025-0004 修复了三个已被积极利用的  
0day  
漏洞，这些漏洞可导致 ESXi 虚拟机逃逸和代码执行：  
- CVE-2025-22226  
    
(CVSS 7.1)：HGFS 文件系统中的越界读取漏洞，允许 VMX 进程内存泄漏。  
- CVE-2025-22224  
    
(CVSS 9.3)：VMCI 中的一个 TOCTOU   
漏洞，会导致越界写入，从而允许以 VMX 进程身份执行代码。  
- CVE-2025-22225  
    
(CVSS 8.2)：ESXi 中的一个任意写入漏洞，允许攻击者逃逸 VMX 沙箱并访问内核。  
![](https://mmbiz.qpic.cn/mmbiz_png/AnRWZJZfVaHPDzSialzBAnD554e0WZcemMBTibribicTjg2LxBWtXibFNVnDp6OctJrtQWoykib5njEzHSrNdPiakn5Ww/640?wx_fmt=png&from=appmsg "")  
  
  
攻击者依赖名为 MAESTRO 的编排器来管理完整的 VMware ESXi 虚拟机逃逸。它会禁用 VMCI 驱动程序，通过BYOD技术加载未签名的漏洞利用驱动程序，并协调漏洞利用过程。  
  
  
该驱动程序会泄漏 VMX 内存以绕过 ASLR，利用 HGFS 和 VMCI 的漏洞，将 shellcode 写入 VMX 进程，并逃逸到 ESXi 内核。  
  
  
然后，它会部署一个隐蔽的基于 VSOCK 的后门程序（VSOCKpuppet），从而能够从客户虚拟机持久远程控制虚拟机管理程序，同时绕过传统的网络监控，并恢复驱动程序以降低被检测到的风险。  
  
  
Huntress 的研究人员发现证据表明，该漏洞利用链可能至少从 2024 年 2 月起就已被使用。Huntress发布的报告指出：“这些漏洞利用二进制文件包含PDB路径，可以深入了解开发环境。”  
  
“ MyDriver.sys：  
  
C:\Users\test\Desktop\2024_02_19\全版本逃逸–交付\报告\ESXI_8.0u3\  
  
  
文件夹名称表明这是一个针对 ESXi 8.0 Update 3 的打包交付物。路径中的日期（2024 年 2 月 19 日）比 VMware 的公开披露早了一年多，证实这是作为潜在的  
0day  
漏洞利用程序开发的。  
  
  
该工具包的来源线索错综复杂。开发路径中包含简体中文，但README文件却是英文的，这表明它可能是为了更广泛地分发或销售而开发的。  
  
  
驱动程序文件中提到了“XLab”，这是一个通用名称，可能只是巧合，目前尚无证据表明它与任何组织有关联。  
  
  
总而言之，该工具包使用了中文元素，技术水平很高，而且在披露之前就可能已经掌握了  
0day  
漏洞，这些都指向一个资金雄厚的开发者。  
  
  
报告总结道：“此次入侵展示了一个复杂的、多阶段的攻击链，旨在突破虚拟机隔离并攻破底层 ESXi 管理程序。攻击者通过信息泄露、内存损坏和沙箱逃逸等一系列攻击手段，实现了所有虚拟机管理员都梦寐以求的目标：从客户虚拟机内部完全控制管理程序。”  
  
  
报告还指出：“从 PDB 路径中揭示的开发时间线表明，该漏洞可能在 VMware 公开披露前一年多就已作为  
0day  
漏洞存在，这凸显了拥有充足资源并能利用未修补漏洞的攻击者所构成的持续威胁。”  
  
  
技术报告全文：  
  
《虚拟机大逃亡：ESXi漏洞利用实战》  
  
https://www.huntress.com/blog/esxi-vm-escape-exploit  
  
  
新闻链接：  
  
https://cybersecuritynews.com/vmware-esxi-exploited-toolkit  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/McYMgia19V0WHlibFPFtGclHY120OMhgwDUwJeU5D8KY3nARGC1mBpGMlExuV3bibicibJqMzAHnDDlNa5SZaUeib46xSzdeKIzoJA/640?wx_fmt=svg "")  
  
**今日安全资讯速递**  
  
  
  
**APT事件**  
  
  
Advanced Persistent Threat  
  
BlueDelta黑客攻击微软OWA、谷歌和Sophos VPN，窃取用户凭证  
  
https://gbhackers.com/bluedelta-hackers-2/  
  
  
UAT-7290 黑客攻击南亚关键基础设施实体  
  
https://cybersecuritynews.com/uat-7290-hackers-attacking-critical-infrastructure/  
  
  
特朗普暗示美军抓捕马杜罗行动期间制造加拉加斯停电  
  
https://gbhackers.com/trump-signals-possible-cyber-involvement-in-caracas-power-loss/  
  
  
黑客利用  
0DAY  
漏洞利用工具包攻击 VMware ESXi 实例  
  
https://cybersecuritynews.com/vmware-esxi-exploited-toolkit  
  
  
  
**一般威胁事件**  
  
  
General Threat Incidents  
  
WhatsApp蠕虫通过联系人自动消息功能在巴西传播Astaroth银行木马  
  
https://securityaffairs.com/186685/malware/astaroth-banking-trojan-spreads-in-brazil-via-whatsapp-worm.html  
  
  
研究人员揭示 AuraStealer 如何使用高级规避技术和 MaaS 模型窃取数据  
  
https://www.esecurityplanet.com/threats/gendigital-research-exposes-aurastealer-infostealer-tactics/  
  
  
GoBruteforcer僵尸网络攻击Linux服务器  
  
https://www.infosecurity-magazine.com/news/gobruteforcer-botnet-linux-servers/  
  
  
新型网络钓鱼攻击冒充DocuSign，在Windows系统上部署隐蔽恶意软件  
  
https://cybersecuritynews.com/new-phishing-attack-impersonate-as-docusign/  
  
  
Kimwolf僵尸网络利用代理服务器传播  
  
https://www.cybermaterial.com/p/kimwolf-botnet-uses-proxies-to-spread  
  
  
超过一百万 Chrome 用户遭窃取信息，疑似存在虚假 ChatGPT 和 DeepSeek 扩展程序  
  
https://hackread.com/fake-chatgpt-deepseek-extensions-spy-chrome-users/  
  
  
Phantom Shuttle Chrome 扩展程序被抓到窃取凭证  
  
https://www.cysecurity.news/2026/01/phantom-shuttle-chrome-extensions.html  
  
  
三个恶意 NPM 包瞄准开发者登录凭证  
  
https://gbhackers.com/npm-packages-3/  
  
  
**漏洞事件**  
  
  
Vulnerability Incidents  
  
CISA警告：微软 PowerPoint 代码注入漏洞已被攻击者利用  
  
https://cybersecuritynews.com/microsoft-powerpoint-code-injection-vulnerability/  
  
  
Coolify披露11个严重漏洞，可能导致自托管实例上的服务器完全被攻破  
  
https://thehackernews.com/2026/01/coolify-discloses-11-critical-flaws.html  
  
  
Cisco ISE漏洞允许远程攻击者访问敏感数据——  
PoC  
已发布  
  
https://cybersecuritynews.com/cisco-ise-vulnerability-sensitive-data/  
  
  
Linux电池实用程序漏洞允许绕过身份验证和篡改系统  
  
https://gbhackers.com/linux-battery-utility-vulnerability/  
  
  
最近修复的 HPE OneView 漏洞正被利用 (CVE-2025-37164)  
  
https://www.helpnetsecurity.com/2026/01/08/hpe-oneview-cve-2025-37164-exploited/  
  
  
针对 Trend Micro Apex Central 中的未经身份验证的远程代码执行漏洞 (CVE-2025-69258) 已发布 PoC  
  
https://www.helpnetsecurity.com/2026/01/08/trend-micro-apex-central-cve-2025-69258-rce-poc/  
  
  
GitLab修复了多个允许任意代码执行的漏洞  
  
https://gbhackers.com/gitlab-patches-multiple-flaws-allowing-arbitrary-code-execution/  
  
  
最高级别的 n8n 漏洞允许随机用户接管服务器  
  
https://www.securityweek.com/critical-vulnerability-exposes-n8n-instances-to-takeover-attacks/  
  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
