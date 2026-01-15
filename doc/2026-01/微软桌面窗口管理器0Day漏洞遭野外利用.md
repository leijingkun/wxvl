#  微软桌面窗口管理器0Day漏洞遭野外利用  
 FreeBuf   2026-01-15 10:37  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/qq5rfBadR38jUokdlWSNlAjmEsO1rzv3srXShFRuTKBGDwkj4gvYy34iajd6zQiaKl77Wsy9mjC0xBCRg0YgDIWg/640?wx_fmt=gif "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibNh0W8icIx5DkT4Jhenqdpb56JiacJ3r9RIcRiam0icMkUqicmED0jG2bQnMf72D6ictbV2oSJUFBf19EQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
微软在2026年1月13日的"补丁星期二"更新中修复了其桌面窗口管理器（DWM）中的一个关键0Day信息泄露漏洞，此前已监测到该漏洞在野外遭到主动利用。  
  
  
该漏洞编号为CVE-2026-20805，允许低权限本地攻击者通过远程ALPC端口暴露敏感的用户模式内存（特别是段地址）。在实际攻击中，这可能有助于构建进一步的权限提升攻击链，促使微软紧急为旧版Windows系统部署补丁。  
  
  
该漏洞被评为"重要"严重等级，CVSS v3.1基础得分为5.5（AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N）。虽然无法远程利用，但其低复杂度且无需用户交互的特点，使其成为恶意软件或入侵后操作的主要目标。  
  
  
微软威胁情报中心（MSTIC）和安全响应中心（MSRC）确认了漏洞利用行为，但指出目前尚未出现公开的PoC。  
  
  
攻击者利用DWM（负责窗口渲染的核心合成引擎）来泄露内存地址。这种泄露可能暴露内核指针或进程数据，有助于绕过ASLR等缓解措施。微软通过协调披露机制感谢内部团队发现该漏洞。  
  
  
**Part01**  
## 受影响平台及补丁  
  
  
该漏洞影响仍处于扩展支持阶段的旧版Windows系统。由于微软将这些更新标记为"必需"，管理员必须优先部署。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qq5rfBadR3ibNh0W8icIx5DkT4JhenqdpbWiannrGGx98cjAn2tGIYaGCyp374IMbfbL38UP3hrOial8Rfp9K8xmKQ/640?wx_fmt=png&from=appmsg "")  
  
  
请查阅MSRC更新获取完整生命周期详情。在此期间，建议限制本地低权限账户并通过EDR工具监控DWM进程。  
  
  
这轮补丁更新突显了在本地权限提升技术日益盛行的情况下，旧版DWM组件面临的持续风险。使用不受支持版本的组织面临更高的暴露风险。  
  
  
**参考来源：**  
  
Microsoft Desktop Window Manager 0-Day Vulnerability Exploited in the wild  
  
https://cybersecuritynews.com/desktop-window-manager-0-day-vulnerability/  
  
  
###   
###   
###   
  
**推荐阅读**  
  
[](https://mp.weixin.qq.com/s?__biz=MjM5NjA0NjgyMA==&mid=2651333596&idx=1&sn=a5f1d8decaf400a24f3b9e74a3a357e1&scene=21#wechat_redirect)  
###   
### 电台讨论  
  
****  
![](https://mmbiz.qpic.cn/mmbiz_png/qq5rfBadR3ibvNluUKZ6RPy7h2fbYibRbLQDHPFqj89KkFsXBRibx5YTLiaTUfFOy9PKicps3l56iazUPNQrwdhkZ7jA/640?wx_fmt=png&from=appmsg "")  
  
****  
  
  
  
  
