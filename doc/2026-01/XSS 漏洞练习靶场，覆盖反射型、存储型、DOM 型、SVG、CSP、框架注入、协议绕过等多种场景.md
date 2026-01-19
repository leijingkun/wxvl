#  XSS 漏洞练习靶场，覆盖反射型、存储型、DOM 型、SVG、CSP、框架注入、协议绕过等多种场景  
duckpigdog
                    duckpigdog  夜组安全   2026-01-19 00:00  
  
免责声明  
  
由于传播、利用本公众号夜组安全所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号夜组安全及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
**所有工具安全性自测！！！VX：**  
**NightCTI**  
  
朋友们现在只对常读和星标的公众号才展示大图推送，建议大家把  
**夜组安全**  
“**设为星标**  
”，  
否则可能就看不到了啦！  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2WrOMH4AFgkSfEFMOvvFuVKmDYdQjwJ9ekMm4jiasmWhBicHJngFY1USGOZfd3Xg4k3iamUOT5DcodvA/640?wx_fmt=png&from=appmsg "")  
  
## 工具介绍  
  
XSS-Sec 靶场项目是一个以“实战为导向”的 XSS 漏洞练习靶场，覆盖反射型、存储型、DOM 型、SVG、CSP、框架注入、协议绕过等多种场景。页面样式统一，逻辑清晰，适合系统化学习与教学演示。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2XXErq77fVGmdFe8daOm3SIDic7T64T7ZLrsL1k5Vr01KJvkU1lECZL4ibhyHobpTCt10sm2DExWRtQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2XXErq77fVGmdFe8daOm3SICUsgPtpWyHicGhNHdD93SCMh2qU4HU747WGicToZzib7TV1R6S9KlOHhw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2XXErq77fVGmdFe8daOm3SIAmtsVWQK4qLD0ohdnaDfXTH64yyHsFohUmicRxHo1S3yOSkfgZ1uD5w/640?wx_fmt=png&from=appmsg "")  
## 关卡总览（名称与简介）  
- Level 1: Reflected XSS — The basics.  
  
- Level 2: DOM-based XSS — Client-side manipulation.  
  
- Level 3: Stored XSS — Persistent payloads.  
  
- Level 4: Attribute Breakout — Escape the attribute.  
  
- Level 5: Filter Bypass — No allowed.  
  
- Level 6: Quote Filtering — Break out of single quotes.  
  
- Level 7: Keyword Removal — Double write bypass.  
  
- Level 8: Encoding Bypass — HTML entities are your friend.  
  
- Level 9: URL Validation — Must contain http://  
  
- Level 10: Protocol Bypass — Case sensitivity matters.  
  
- Level 11: JS Context — Break out of JS string.  
  
- Level 12: DOM XSS via Hash — The server sees nothing.  
  
- Level 13: Frontend Filter — Bypass the regex.  
  
- Level 14: Double Encoding — Double the trouble.  
  
- Level 15: Framework Injection — AngularJS Template Injection.  
  
- Level 16: PostMessage XSS — Talk to the parent.  
  
- Level 17: CSP Bypass — Strict CSP? Find a gadget.  
  
- Level 18: Anchor Href XSS — Stored XSS in href.  
  
- Level 19: DOM XSS in Select — Break out of select.  
  
- Level 20: jQuery Anchor XSS — DOM XSS in jQuery attr().  
  
- Level 21: JS String Reflection — Reflected XSS in JS string.  
  
- Level 22: Reflected DOM XSS — Server reflection + Client sink.  
  
- Level 23: Stored DOM XSS — Replace only once.  
  
- Level 24: WAF Bypass (Tags/Attrs) — Reflected XSS with strict WAF.  
  
- Level 25: SVG Animate XSS — SVG-specific vector bypass.  
  
- Level 26: Canonical Link XSS — Escaping single quotes issue.  
  
- Level 27: Stored XSS in onclick — Entities vs escaping pitfall.  
  
- Level 28: Template Literal XSS — Reflected into JS template string.  
  
- Level 29: Cookie Exfiltration — Stored XSS steals session cookie.  
  
- Level 30: Angular Sandbox Escape — No strings, escape Angular sandbox.  
  
- Level 31: AngularJS CSP Escape — Bypass CSP and escape Angular sandbox.  
  
- Level 32: Reflected XSS (href/events blocked) — Bypass via SVG animate to set href.  
  
- Level 33: JS URL XSS (chars blocked) — Reflected XSS in javascript: URL with chars blocked.  
  
- Level 34: CSP Bypass (report-uri token) — Chrome-only CSP directive injection via report-uri.  
  
- Level 35: Upload Path URL XSS — Independent lab: upload HTML, random rename, URL concat XSS.  
  
- Level 36: Hidden Adurl Reflected XSS — Independent lab: hidden ad anchor reflects adurl/adid.  
  
- Level 37: Data URL Base64 XSS — Blacklist filter; must use data:text/html;base64 in object.  
  
- Level 38: PDF Upload XSS — Independent lab: upload PDF, view opens HTML-in-PDF causing XSS.  
  
- Level 39: Regex WAF Bypass — src/="data:..." bypasses WAF regex.  
  
- Level 40: Bracket String Bypass — href reflects; use window["al"+"ert"] to evade WAF.  
  
- Level 41: Fragment Eval/Window Bypass — Echo HTML; split strings then eval or window[a+b].  
  
- Level 42: Login DB Error XSS — Independent lab: invalid DB shows error, SQL reflects username.  
  
- Level 43: Chat Agent Link XSS — Independent lab: chat echoes, agent clicks user link executes.  
  
- Level 44: CSS Animation Event XSS — Strong WAF: only @keyframes+xss onanimationend allowed.  
  
- Level 45: RCDATA Textarea Breakout XSS — Strong WAF: only textarea/title RCDATA breakout works.  
  
- Level 46: JS String Escape (eval) — theme string injection; escape with eval(myUndefVar); alert(1);  
  
- Level 47: Throw onerror comma XSS — Strong WAF: only throw onerror=alert,cookie  
  
- Level 48: Symbol.hasInstance Bypass — Strong WAF: only instanceof+eval chain  
  
- Level 49: Video Source onerror XSS — Strong WAF: only video source onerror  
  
- Level 50: Bootstrap RealSite XSS — Independent site: only xss onanimationstart  
  
  
  
## 工具获取  
  
  
  
点击关注下方名片  
进入公众号  
  
回复关键字【  
260119  
】获取  
下载链接  
  
  
## 往期精彩  
  
  
[一款自动化403/401绕过工具 | 请求头注入、谓词篡改等多种实战技巧2026-01-16](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496113&idx=1&sn=6e48fa8ac465f65cffbe9753de1b9c7f&scene=21#wechat_redirect)  
[Burp Suite插件 | 高级HTTP头修改安全头来绕过安全限制、不同来源或设备的请求2026-01-12](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496101&idx=1&sn=731e9d4fa9782a4bfdcc8622d4bf16b9&scene=21#wechat_redirect)  
[Webshell_Generate更新V1.2.6！ | 各类webshell免杀，支持蚁剑、冰蝎、哥斯拉等2026-01-09](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496095&idx=1&sn=201f62e956bc5954c4aecc6204f1f16d&scene=21#wechat_redirect)  
[新一代Webshell 管理与后渗透平台 | 去除通信流量强特征，支持自定义流量格式实现流量伪装2026-01-08](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496090&idx=1&sn=e56acbfd4147b9a2b07f2f73a333561b&scene=21#wechat_redirect)  
[Burp Suite插件 | AI连接本地工具、数据库或远程 Agent，辅助安全测试。2026-01-07](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496073&idx=1&sn=136cb9b4aff4f4975877bbd2df485a04&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OAmMqjhMehrtxRQaYnbrvafmXHe0AwWLr2mdZxcg9wia7gVTfBbpfT6kR2xkjzsZ6bTTu5YCbytuoshPcddfsNg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&random=0.8399406679299557&tp=webp "")  
  
