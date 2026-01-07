#  D-Link：遗留 DSL 路由器中存在已遭利用漏洞，速修复  
Bill Toulas  代码卫士   2026-01-07 10:04  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**已数年不受支持的D-Link DSL网关路由器中存在一个严重的命令注入漏洞CVE-2026-0625，目前已遭活跃利用。**  
  
  
  
该漏洞的根源在于CGI库对输入验证处理不当，导致 dnscfg.cgi 端点存在安全隐患。未经身份验证的攻击者可通过篡改DNS配置参数执行远程命令，由漏洞情报公司VulnCheck于12月15日向D-Link报送。此前，Shadowserver基金会的蜜罐系统已监测到针对该漏洞的利用尝试。VulnCheck透露，Shadowserver捕获的攻击手法此前似乎未被公开记录。该公司在安全公告中提到，"未经身份验证的远程攻击者可通过注入并执行任意shell命令的方式，实现远程代码执行。"   
  
与VulnCheck 协同调查后，D-Link公司确认以下设备型号及固件版本受影响：  
  
- DSL-526B ≤ 2.01  
  
- DSL-2640B ≤ 1.07  
  
- DSL-2740R < 1.17  
  
- DSL-2780B ≤ 1.01.14  
  
  
  
上述型号已于2020年达到生命周期期限，因此不会获得固件更新。D-Link公司强烈建议用户将受影响设备更换为受支持的新型号。D-Link公司目前仍在通过分析各版本固件，确定是否有其它产品受此漏洞影响。该公司解释称，"由于固件实现方式和产品代际差异，D-Link与VulnCheck在精准识别所有受影响型号方面都面临复杂挑战。当前分析表明，除了直接检查固件外，尚无可靠的型号检测方法。为此，D-Link正在对历史遗留平台和受支持平台的固件构建版本进行全面验证。"  
  
目前尚不清楚漏洞利用者的身份及其攻击目标。但VulnCheck指出，大多数消费级路由器的管理接口（如dnscfg.cgi等CGI端点）通常仅限局域网访问。若要利用该漏洞，可能需要通过基于浏览器的攻击手段，或目标设备已开启远程管理功能。  
  
使用已达生命周期路由器及网络设备的用户，应更换为受支持型号，或将它们部署在非关键网络环境中（最好进行网络隔离），并确保安装最新可用固件版本，同时启用严格的安全设置。  
  
D-Link公司提醒用户称，已达生命周期的设备不会获得任何固件更新、安全补丁或维护支持。  
  
  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[D-Link：注意 DIR-878 路由器中的多个RCE漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524471&idx=1&sn=2c5a11dba130eea68bd2080f69e5410e&scene=21#wechat_redirect)  
  
  
[D-Link 决定不修复这个严重漏洞，影响6万多台 NAS 设备](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247521440&idx=2&sn=1ddad70ced916d3fad493805c8d04d62&scene=21#wechat_redirect)  
  
  
[D-Link 修复D-Link D-View 8软件中的认证绕过和 RCE 漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247516588&idx=1&sn=981ba7ac004cc1b05279618b3be57e1f&scene=21#wechat_redirect)  
  
  
[D-Link 修复多个硬编码密码漏洞](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247506450&idx=3&sn=e93472d0452c2f5615ef9f11c0f4cb71&scene=21#wechat_redirect)  
  
  
[D-Link 修复VPN路由器中的多个远程命令注入漏洞，还有一个未修复](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247498584&idx=6&sn=ae761df7a81ed6965f4a2dcde6755e2e&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://www.bleepingcomputer.com/news/security/new-d-link-flaw-in-legacy-dsl-routers-actively-exploited-in-attacks/  
  
  
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
  
