#  趋势科技：速修复这个严重的 Apex Central RCE漏洞  
Sergiu Gatlan  代码卫士   2026-01-12 10:20  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**趋势科技修复了位于 Apex Central 本地版中的一个严重漏洞CVE-2025-69258，可导致攻击者以系统权限执行任意代码。**  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
Apex Central 是一款基于 web 的管理面板，帮助管理员统一管理趋势科技多款产品和服务（包括杀毒、内容安全和威胁检测），并部署多个组件如杀毒模式文件、扫描引擎和反垃圾邮件规则等。  
  
该漏洞可导致在目标系统上无权限的威胁人员通过将恶意DDL 注入低复杂攻击活动中的方式获得远程代码执行权限。趋势科技在上周发布的一份安全公告中提到，“趋势科技 Apex Central 中存在一个 LoadLibraryEX 漏洞，可导致未认证的远程攻击者将受攻击者控制的 DLL 加载到一个关键的可执行文件中，从而导致在受影响设备上以系统权限执行由攻击者提供的代码。”  
  
Tenable 公司发现并报送了该漏洞，该公司提到，未认证的远程攻击者可向监听 TCP 端口20001 的 MsgReceiver.exe 进程发送一条特殊构造的消息，“可导致在系统权限的安全上下文中执行由攻击者提供的代码”。  
  
虽然存在一些缓解因素，如易受攻击的系统被暴露给互联网攻击，但趋势科技公司仍然督促客户尽快修复系统。该公司提到，“除了及时应用补丁和更新后的解决方案外，建议客户查看关键系统的远程访问权限并确保策略和边界安全是最新状态。然而，尽管利用可能需要满足多个特定条件，但趋势科技公司强烈建议客户尽快更新至最新版本。”  
  
此外，趋势科技还发布了可遭未经身份验证攻击者利用的两个拒绝服务漏洞CVE-2025-69259和CVE-2025-69260。该公司还在三年前修复了另外一个严重的 Apex Central 漏洞CVE-2022-26871，提醒客户称该漏洞已遭在野利用。  
  
****  
****  
  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[趋势科技证实本地系统中的两个严重 Apex One 漏洞已遭利用](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247523757&idx=2&sn=a7158cc27029a29dc4b1bc6553c14569&scene=21#wechat_redirect)  
  
  
[趋势科技修复已遭利用的Apex One 0day漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247517701&idx=1&sn=242a34391add46cf11121fbb52e0558b&scene=21#wechat_redirect)  
  
  
[趋势科技修复又一个已遭利用的Apex One漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247513971&idx=2&sn=e4ccf27e2628af5d0016b4b5f9385131&scene=21#wechat_redirect)  
  
  
[趋势科技修复已遭利用的 Apex Central 0day](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247511227&idx=2&sn=3003d8d5ba53390d7d87aef04901daaa&scene=21#wechat_redirect)  
  
  
[趋势科技称 Apex One EDR 平台的两个0day已遭在野利用](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247507161&idx=1&sn=17864c45b313691348a041867c8e49bf&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://www.bleepingcomputer.com/news/security/trend-micro-fixes-critical-rce-flaw-in-apex-central-console/  
  
  
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
  
