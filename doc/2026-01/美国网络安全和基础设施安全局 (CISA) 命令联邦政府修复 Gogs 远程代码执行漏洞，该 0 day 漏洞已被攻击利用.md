#  美国网络安全和基础设施安全局 (CISA) 命令联邦政府修复 Gogs 远程代码执行漏洞，该 0 day 漏洞已被攻击利用  
Rhinoer
                    Rhinoer  犀牛安全   2026-01-16 16:00  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qvpgicaewUBnsgul0KeLl5TTQWVH1kE1EcSqdLOz4NlVcMOybTferbaCmbl22IETWdc0cQmg0vVPpibE4XgK6KCg/640?wx_fmt=png&from=appmsg "")  
  
美国网络安全和基础设施安全局 (CISA) 已下令政府机构保护其系统免受 Gogs 高危漏洞的攻击，该漏洞曾被用于零日攻击。  
  
Gogs 的设计初衷是作为 GitLab 或 GitHub Enterprise 的替代方案，它使用 Go 语言编写，通常在线开放以进行远程协作。  
  
该远程代码执行 (RCE) 安全漏洞被追踪为CVE-2025-8110，它源于 PutContents API 中的路径遍历弱点，允许经过身份验证的攻击者通过符号链接覆盖存储库外部的文件，从而绕过针对先前已修复的 RCE 漏洞 (CVE-2024-55947) 实施的保护措施。  
  
攻击者可以利用此漏洞，创建包含指向敏感系统文件的符号链接的仓库，然后使用 PutContents API 通过符号链接写入数据，从而覆盖仓库外部的目标。通过覆盖 Git 配置文件（特别是 sshCommand 设置），攻击者可以强制目标系统执行任意命令。  
  
Wiz Research 在 7 月份调查影响客户面向互联网的 Gogs 服务器的恶意软件感染时发现了该漏洞，并于 7 月 17 日向 Gogs 维护人员报告了该缺陷。三个月后，即 10 月 30 日，他们确认了 Wiz 的报告，并在上周发布了针对 CVE-2025-8110 的补丁，该补丁在所有文件写入入口点添加了符号链接感知路径验证。  
  
根据 Wiz Research 分享的披露时间表， 11 月 1 日出现了第二波针对此零日漏洞的攻击。  
  
在调查这些攻击活动的过程中，Wiz 的研究人员发现超过 1400 台 Gogs 服务器暴露在网上（其中 1250 台仍然暴露），以及超过 700 个实例显示出被入侵的迹象。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qvpgicaewUBnsgul0KeLl5TTQWVH1kE1EXia9t1NmCKeM03TfsGgegFVS3L4YwBKfBXH4kYDD3xA0YeabfqDIYVg/640?wx_fmt=png&from=appmsg "")  
  
CISA 现已确认 Wiz 的报告，并将该安全漏洞添加到其已被利用的漏洞列表中，并命令联邦民事行政部门 (FCEB) 机构在三周内（即 2026 年 2 月 2 日之前）进行修补。  
  
FCEB 机构是指美国非军事行政部门机构，例如能源部、司法部、国土安全部和国务院。  
  
此类漏洞是恶意网络攻击者常用的攻击途径，对联邦机构构成重大风险，”CISA警告说。“请按照供应商说明采取缓解措施，遵循适用于云服务的BOD 22-01指南，或者如果无法采取缓解措施，则停止使用该产品。  
  
为了进一步减少攻击面，建议 Gogs 用户立即禁用默认的开放注册设置，并使用 VPN 或允许列表限制服务器访问。  
  
此外，想要检查 Gogs 实例是否存在入侵迹象的管理员应该查找 PutContents API 的可疑使用情况，以及在两次攻击期间创建的具有随机八字符名称的存储库。  
  
  
信息来源：Bleepingcomputer  
  
