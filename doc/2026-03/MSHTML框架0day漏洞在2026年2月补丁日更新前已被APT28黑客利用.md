#  MSHTML框架0day漏洞在2026年2月补丁日更新前已被APT28黑客利用  
会杀毒的单反狗
                    会杀毒的单反狗  军哥网络安全读报   2026-03-03 01:02  
  
**导****读**  
  
  
  
据报道，与俄罗斯有关联的 APT28 在微软修复 MSHTML   
0day  
漏洞 CVE-2026-21513 之前利用了该漏洞，这是一个安全功能绕过漏洞。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/PaFY6wibdwyKka9pw2ibFHFqveuIxwTvic0aawM9WBhNBjVia31iaGMib1xvks7H2waPnAbr7oaicvSeNvibpsicOk3g8SkibDj6P03qia5lUlo67BzQs4/640?wx_fmt=jpeg&from=appmsg "")  
  
  
漏洞编号为CVE-2026-21513，允许攻击者绕过安全机制并执行任意文件。该漏洞的 CVSS 评分为 8.8，影响所有 Windows 版本。  
  
  
该漏洞是一种绕过 Internet Explorer 安全控制的漏洞，当受害者打开恶意 HTML 页面或 LNK 文件时，可能导致代码执行。攻击者可以通过打开恶意 HTML 或 LNK 文件触发此漏洞，从而绕过保护措施并可能执行代码。微软并未透露太多细节。  
  
  
Akamai 的安全研究人员发现，俄罗斯黑客组织 APT28 在微软于 2026 年 2 月发布补丁之前就已开始利用该漏洞。  
  
  
Akamai 的研究人员使用 PatchDiff-AI 分析该漏洞的原因，将 CVE-2026-21513 追溯到 ieframe.dll 中的超链接导航逻辑。他们发现，URL 验证机制的缺陷使得攻击者的输入能够到达 ShellExecuteExW，从而在浏览器沙箱之外执行代码。  
  
  
研究人员使用 MSHTML 组件重现了该漏洞，并找到了一个漏洞利用样本 document.doc.LnK.download，该样本于 2026 年 1 月上传，并与 APT28 基础设施相关联。  
  
  
Akamai发布的报告指出：“通过将易受攻击的代码路径与公开的威胁情报进行关联，研究人员发现一个利用此功能的样本：document.doc.LnK.download。该样本于2026年1月30日首次提交至VirusTotal，就在2月份的‘补丁日’之前不久，与活跃的俄罗斯威胁组织APT28的基础设施相关 。”  
  
  
该有效载荷使用精心构造的 Windows 快捷方式 (.lnk) 文件，该文件在标准的 LNK 结构之后直接嵌入了一个 HTML 文件。执行后，它会连接到wellnesscaremed[.]com，该域名归属于 APT28，并在该攻击活动的多个阶段中被广泛使用。  
  
  
该漏洞利用程序依赖于嵌套的 iframe 和多个 DOM 上下文来操纵信任边界，绕过 Mark of the Web (MotW) 和 Internet Explorer 增强安全配置 (IE ESC)。通过降低安全上下文，它会触发易受攻击的导航流程，从而允许攻击者控制的内容调用 ShellExecuteExW 并在浏览器沙箱之外执行代码。  
  
  
报告总结道：“虽然此次攻击活动利用了恶意 .LNK文件，但任何嵌入MSHTML的组件都可能触发易受攻击的代码路径。因此，除了基于LNK的 网络钓鱼之外，还应预料到会出现其他传播机制。”  
  
  
微软通过加强超链接协议验证来解决该问题，以防止file://、http:// 和 https:// 链接到达 ShellExecuteExW。  
  
  
技术报告：  
  
https://www.akamai.com/blog/security-research/2026/feb/inside-the-fix-cve-2026-21513-mshtml-exploit-analysis  
  
  
新闻链接：  
  
https://securityaffairs.com/188782/security/russia-linked-apt28-exploited-mshtml-zero-day-cve-2026-21513-before-patch.html  
  
  
****  
扫码关注  
  
军哥网络安全读报  
  
**讲述普通人能听懂的安全故事**  
  
  
