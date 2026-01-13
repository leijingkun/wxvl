#  React Router严重漏洞可用于访问或修改服务器文件  
Abinaya  代码卫士   2026-01-13 04:05  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Az5ZsrEic9ot90z9etZLlU7OTaPOdibteeibJMMmbwc29aJlDOmUicibIRoLdcuEQjtHQ2qjVtZBt0M5eVbYoQzlHiaw/640?wx_fmt=gif "")  
    
聚焦源代码安全，网罗国内外最新资讯！  
  
**编译：代码卫士**  
  
**安全研究员在 React Router 中发现多个严重漏洞，可导致攻击者通过目录遍历访问或修改服务器文件。这些漏洞影响 React Router 生态系统中的多个程序包，且CVSS v3评分为9.8分，属于严重级别。**  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
其中最严重的漏洞是CVE-2025-61686，位于使用未签名cookie 时的 createFileSessionStorage() 函数中。攻击者可操纵会话 cookie，强制该应用程序在指定的会话目录范围外读写文件。  
  
React Router和 Remix 生态系统中的多个包受影响，如下：  
  
- @react-router/node 7.0.0至7.9.3版本受影响  
  
- @remix-run/deno 2.17.1及更早版本受影响  
  
- @remix-run/node 2.17.1及更早版本受影响  
  
  
  
该漏洞可导致攻击者通过恶意会话 cookie 触发目录遍历攻击。虽然攻击者无法直接检索文件内容，但成功利用该漏洞可导致攻击者读取匹配会话文件格式标准的文件，修改可被应用逻辑返回的会话数据。攻击者能否访问敏感的配置文件取决于服务器的权限，而攻击的效果取决于 web 服务器进程权限以及文件系统的访问控制。开发人员必须立即更新至如下版本：  
  
- @react-router/node 7.9.4或后续版本  
  
- @remix-run/deno 2.17.2或后续版本  
  
- @remix-run/node 2.17.2或后续版本  
  
  
  
补丁通过在会话存储机制内执行正确的路径验证和清理，修复了该目录遍历漏洞。GitHub 安全公告提到，受影响的组织机构应当立即更新至已修复版本、查看服务器文件权限和访问控制的情况。审计未签名 cookie 使用的会话存储实现情况。监控可疑的会话 cookie 模式。在可行的情况下执行其它的文件系统限制。  
  
  
 开源  
卫士试用地址：  
https://oss.qianxin.com/#/login  
  
  
 代码卫士试用地址：https://sast.qianxin.com/#/login  
  
  
  
  
  
  
  
  
  
**推荐阅读**  
  
[React RSC 新漏洞可导致 DoS 和源代码泄露](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524668&idx=2&sn=bd7c744b1ab80e6eeb119c4307ab5cdd&scene=21#wechat_redirect)  
  
  
[速修复！React满分漏洞同时影响 Next.js，可导致未认证RCE](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524584&idx=2&sn=d601040ac12c82888e8edba633d2fc02&scene=21#wechat_redirect)  
  
  
[热门 React Native NPM 包中存在严重漏洞，开发人员易受攻击](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247524330&idx=2&sn=bc54e02a8f815ed78b67d3135a9f9607&scene=21#wechat_redirect)  
  
  
[热门NPM库 “coa” 和“rc” 接连遭劫持，影响全球的 React 管道](https://mp.weixin.qq.com/s?__biz=MzI2NTg4OTc5Nw==&mid=2247508946&idx=1&sn=273c58d08a4225306a567cf6a150f40c&scene=21#wechat_redirect)  
  
  
  
  
  
**原文链接**  
  
https://cybersecuritynews.com/react-router-vulnerability/  
  
  
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
  
