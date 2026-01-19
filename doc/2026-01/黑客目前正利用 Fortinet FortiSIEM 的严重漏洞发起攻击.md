#  黑客目前正利用 Fortinet FortiSIEM 的严重漏洞发起攻击  
原创 ZM
                    ZM  暗镜   2026-01-19 05:25  
  
Fortinet FortiSIEM 的一个严重漏洞，其概念验证利用代码已公开，目前正被攻击者利用。  
  
据渗透测试公司 Horizon3.ai 的安全研究员 Zach Hanley 报告的漏洞 ( CVE-2025-64155 ) 称，该漏洞是两个问题的结合，允许使用管理员权限进行任意写入，并可将权限提升至 root 访问权限。  
  
Fortinet周二发布安全更新以修复此漏洞时解释说：“对FortiSIEM中操作系统命令（‘操作系统命令注入’）漏洞[CWE-78]中使用的特殊元素处理不当，可能允许未经身份验证的攻击者通过精心构造的TCP请求执行未经授权的代码或命令。”  
  
Horizon3.ai发布了一篇技术文章，解释说该问题的根本原因是 phMonitor 服务上数十个命令处理程序暴露出来，这些处理程序可以在未经身份验证的情况下远程调用。该公司还发布了概念验证漏洞利用代码，该代码允许通过滥用参数注入来覆盖 /opt/charting/redishb.sh 文件，从而以 root 身份执行代码。  
  
该漏洞影响 FortiSIEM 6.7 至 7.5 版本，可通过升级至 FortiSIEM 7.4.1 或更高版本、7.3.5 或更高版本、7.2.7 或更高版本，或 7.1.9 或更高版本进行修复。建议使用 FortiSIEM 7.0.0 至 7.0.4 以及 FortiSIEM 6.7.0 至 6.7.10 的客户迁移至已修复的版本。  
  
周二，Fortinet 还分享了一个临时解决方法，供无法立即应用安全更新的管理员使用，该方法要求他们限制对 phMonitor 端口 (7900) 的访问。  
  
两天后，威胁情报公司 Defused 报告称，威胁行为者目前正在积极利用 CVE-2025-64155 漏洞。  
  
Defused 警告称： “Fortinet FortiSIEM 漏洞 CVE-2025-64155 正在我们的蜜罐中遭受积极的、有针对性的利用。 ”  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mibm5daOCStibAvvfq7ob445u4G6ibc2lhSC6boBRBYGsMfEiaA5Z41goibVfyDvPMEMVBIWWcboQwVdJEoic9xP9o5A/640?wx_fmt=png&from=appmsg "")  
  
Horizon3.ai 还提供入侵指标，帮助防御者识别已被入侵的系统。正如研究人员解释的那样，管理员可以通过检查 /opt/phoenix/log/phoenix.logs 中的 phMonitor 消息日志，查找包含 PHL_ERROR 条目的行中的有效载荷 URL，从而找到恶意滥用的证据。  
  
Fortinet尚未更新其安全公告，也未将该漏洞标记为已被利用的攻击。BleepingComputer也联系了Fortinet发言人，以确认有关该漏洞已被利用的报道，但尚未收到回复。  
  
11 月，Fortinet发出警告，称攻击者正在利用FortiWeb 零日漏洞(CVE-2025-58034)；一周后，该公司确认已悄悄修复了第二个 FortiWeb 零日漏洞 (CVE-2025-64446)，该漏洞也成为广泛攻击的目标。  
  
2025 年 2 月，该组织还披露，中国 Volt Typhoon 黑客组织利用 FortiOS 的两个漏洞（分别追踪为 CVE-2023-27997 和 CVE-2022-42475）在荷兰国防部军事网络上部署了 Coathanger 远程访问木马恶意软件。  
  
  
  
