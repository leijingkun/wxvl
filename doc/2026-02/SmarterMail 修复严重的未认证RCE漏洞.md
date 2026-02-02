#  SmarterMail 修复严重的未认证RCE漏洞  
Ravie Lakshmanan
                    Ravie Lakshmanan  代码卫士   2026-02-02 10:10  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**SmarterTools公司已修复SmarterMail邮件软件中的两个漏洞，其中一个编号为CVE-2026-24423，CVSS 评分9.3，可导致任意代码执行后果。**********  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
CVE.org对该漏洞的描述为："SmarterTools SmarterMail 9511版本之前的软件中，ConnectToHub API方法存在未经身份验证的远程代码执行漏洞。攻击者可将SmarterMail指向恶意HTTP服务器，该服务器会下发恶意操作系统命令，这些命令将被存在漏洞的应用程序执行。"  
  
该安全漏洞已于2026年1月15日发布的Build 9511版本中修复。该版本还修复了另一个已遭活跃利用的严重漏洞（CVE-2026-23760，CVSS评分：9.3）。  
  
此外，SmarterTools公司还修复了一个中危漏洞（CVE-2026-25067，CVSS评分：6.9），该漏洞可使攻击者利用NTLM中继攻击实现未经授权的网络认证。该漏洞被描述为影响"每日背景预览"端点的未授权路径篡改问。  
  
VulnCheck在警报中指出："应用程序会对攻击者提供的输入进行Base64解码，并在未经验证的情况下将它用作文件系统路径。在Windows系统上，可解析UNC（通用命名约定）路径，导致SmarterMail服务向攻击者控制的主机发起出站SMB认证尝试。该漏洞可被滥用于窃取凭据、NTLM中继攻击及未授权网络认证。"  
  
该漏洞已在2026年1月22日发布的Build 9518版本中完成修复。鉴于过去一周内SmarterMail软件已有两个漏洞遭活跃利用，用户务必尽快更新至最新版本。  
  
****  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[SmarterMail 认证绕过新漏洞被用于劫持管理员账号](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524955&idx=1&sn=34fffd85602bd5f7fa445177131ff399&scene=21#wechat_redirect)  
  
  
[SmarterMail 严重漏洞可导致服务器遭完全接管](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524768&idx=1&sn=9637d0eb4df20ac66e8c9fa5c1d84ddd&scene=21#wechat_redirect)  
  
  
[SolarWinds 修复四个严重漏洞，可导致未认证RCE和认证绕过](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247525028&idx=3&sn=70181900e6f00cf38ce9655f395495d9&scene=21#wechat_redirect)  
  
  
[思科修复已遭利用的 Unified CM RCE 0day漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524955&idx=2&sn=922edf69046bb2a552b3f58f4f21f882&scene=21#wechat_redirect)  
  
  
[OpenSSH 严重漏洞可导致 Moxa 以太网交换机易受RCE攻击](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524868&idx=2&sn=734a45fb6b2c137eff46cd0261228384&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://thehackernews.com/2026/01/smartermail-fixes-critical.html  
  
  
题图：Pixa  
bay Licens  
e  
  
  
**本文由奇安信编译，不代表奇安信观点。转载请注明“转自奇安信代码卫士 https://codesafe.qianxin.com”。**  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSf7nNLWrJL6dkJp7RB8Kl4zxU9ibnQjuvo4VoZ5ic9Q91K3WshWzqEybcroVEOQpgYfx1uYgwJhlFQ/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/oBANLWYScMSN5sfviaCuvYQccJZlrr64sRlvcbdWjDic9mPQ8mBBFDCKP6VibiaNE1kDVuoIOiaIVRoTjSsSftGC8gw/640?wx_fmt=jpeg "")  
  
**奇安信代码卫士 (codesafe)**  
  
国内首个专注于软件开发安全的产品线。  
  
   ![](https://mmbiz.qpic.cn/mmbiz_gif/oBANLWYScMQ5iciaeKS21icDIWSVd0M9zEhicFK0rbCJOrgpc09iaH6nvqvsIdckDfxH2K4tu9CvPJgSf7XhGHJwVyQ/640?wx_fmt=gif "")  
  
   
觉得不错，就点个 “  
在看  
” 或 "  
赞  
” 吧~  
  
