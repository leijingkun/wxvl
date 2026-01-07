#  苹果新型TCC绕过漏洞，macOS面临自动化攻击风险  
 FreeBuf   2026-01-07 09:37  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/qq5rfBadR38jUokdlWSNlAjmEsO1rzv3srXShFRuTKBGDwkj4gvYy34iajd6zQiaKl77Wsy9mjC0xBCRg0YgDIWg/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR3ibvNluUKZ6RPy7h2fbYibRbLiaPN0TxXXS0IFdZWfAvgD1sfRcLtbEIKAKictx61DbVhJ9II3IfrOBEQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
研究人员发现macOS存在一个高危安全漏洞（CVE-2025-43530），攻击者可借此完全绕过透明化、同意与控制（TCC）保护机制。该漏洞通过VoiceOver屏幕阅读器框架中的com.apple.scrod服务实现攻击。  
  
  
**Part01**  
## 漏洞技术细节  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qq5rfBadR3ibvNluUKZ6RPy7h2fbYibRbLG6pOhBVAd6icjU3vAjFl97P5NsxknlemSg10CALXwlib4n8Q2VUVojKQ/640?wx_fmt=png&from=appmsg "")  
  
  
VoiceOver作为苹果为视障用户开发的内置辅助工具，运行时具有特殊系统权限，可广泛访问用户数据。攻击者利用该服务可执行任意AppleScript命令，并向包括Finder在内的任何应用发送AppleEvent，从而规避TCC安全控制。  
  
  
**Part02**  
## 攻击原理分析  
```

```  
  
该漏洞存在两种攻击方式：首先，攻击者可将恶意代码注入苹果签名的系统二进制文件，此过程无需管理员权限。验证逻辑错误地信任所有苹果签名代码，无法区分合法系统进程与遭篡改进程。  
  
  
其次，通过"检查时间与使用时间差"（TOCTOU）攻击，攻击者可在安全验证与执行之间操纵应用程序来绕过验证检查。两种方式结合可轻松实现完全TCC绕过。  
  
  
**Part03**  
## 漏洞危害评估  
```

```  
  
成功利用该漏洞后，攻击者能在无用户通知或授权的情况下：读取敏感文档、访问麦克风、与Finder交互、执行任意AppleScript代码，致使macOS的TCC保护机制完全失效。  
  
  
苹果已在macOS 26.2中通过实施更强大的基于权限的验证系统修复该漏洞。新补丁要求进程必须具有特定"com.apple.private.accessibility.scrod"权限，并通过客户端审计令牌直接验证该权限，而非采用基于文件的验证方式。此方案同时消除了代码注入漏洞和TOCTOU时间差问题。  
  
  
据GitHub上发布的jhftss报告显示，该漏洞已有公开可用的PoC，表明很可能存在活跃攻击。所有macOS用户应立即升级至26.2或更高版本以防范此高危TCC绕过漏洞。  
  
  
**参考来源：**  
  
New macOS TCC Bypass Vulnerability Allow Attackers to Access Sensitive User Data  
  
https://cybersecuritynews.com/macos-tcc-attack-bypass-vulnerability/  
  
  
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
  
  
  
  
