#  AdonisJS 9.2 框架存在严重漏洞，可导致任意文件写入和RCE  
Ddos  代码卫士   2026-01-06 09:54  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**以注重人体工程学设计和运行速度著称的热门Node.js全栈 web 框架 AdonisJS 的文件上传处理中存在一个严重漏洞（CVE-2026-21440，CVSS评分9.2），可导致远程攻击者覆写敏感系统文件，甚至实现远程代码执行 (RCE)。**  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
该漏洞影响用于解析多部分表单数据的核心组件 @adonisjs/bodyparser 包。当开发人员使用 MultipartFile.move(location,options) 函数保存已上传的文件时，该系统依赖于本不应信任的信任。  
  
安全公告解释称，“如未提供 options.name 参数，则系统默认为未清理的客户端文件名称，并通过 path.join(location,name) 构建目标路径。”这就导致攻击者提供一个包含遍历序列（如 ../../）的构造的文件名称。由于系统将该恶意名称与目标目录连接，因此该文件能够“突破默认的或由开发人员选择的预期目录”，最终存放到该进程能够访问的服务器文件系统上的任何地方。更具风险的是，默认设置具有过度许可性：“若未提供 options.overwrite参数，则系统默认启用覆写模式，导致文件被覆写。”  
  
这种“任意文件写入”漏洞的影响远超简单的破坏行为，如果攻击者能够覆写特定文件，则实际可控制服务器。分析报告提到，“如果攻击者能够覆写应用代码、启动脚本或者配置随后被执行/加载的配置文件，则很有可能实现RCE。”  
  
虽然并不一定能够实现RCE，具体取决于文件权限和部署布局，但攻陷整个系统的可能性很大。安全公告也提到，之前的文档版本可能演示了可导致开发人员陷入这种“易受攻击代码路径”的示例，也加剧了该问题的严重性。  
  
该漏洞影响使用 bodyparser 包的大量安装版本：  
  
- @adonisjs/bodyparser 10.1.1及之前版本  
  
- 早于11.0.0-next.6的预发布版本11.x  
  
  
  
维护人员已为这两个主要版本发布修复方案。建议开发人员立即更新其依赖关系至10.1.2版本和11.0.0-next.6版本。此外，若需官方版本注释和补丁，可参见该项目的GitHub 仓库。  
  
  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[MongoDB库中存在多个漏洞，可用于在Node.js服务器上实现RCE](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247522334&idx=2&sn=474bb8a99d7a850a95b1ba847ff41044&scene=21#wechat_redirect)  
  
  
[Node.js 修复多个漏洞，可导致RCE和HTTP请求走私](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247512799&idx=1&sn=4c911268f2a7aa2fa486878c0d088a2b&scene=21#wechat_redirect)  
  
  
[Node.js 沙箱易受原型污染攻击](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247508579&idx=2&sn=90cb9a086322e582c71840a9be1eed6a&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://securityonline.info/cve-2026-21440-new-adonisjs-9-2-critical-flaw-allows-arbitrary-file-writes-and-rce/  
  
  
题图：Pixabay License  
  
  
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
  
