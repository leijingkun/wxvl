#  React2Shell 图形化漏洞检测利用工具  
原创 Faithtiann
                        Faithtiann  SEVENTEENSEC   2026-01-16 02:47  
  
**点击蓝字 关注我们**  
  
**SEVENTEENSEC**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/bL2iaicTYdZn710IfjLq2GjCeJbfauy3eP0MNv9o4AHI2TSLSHMLH3qOCLBB8hZ7ia3VWiacBickgslKxBMxbNhmmDg/640?wx_fmt=png "")  
  
  
  
声明  
  
  
⊙  
本文作者：  
Faithtiann  
  
⊙  
本文字数：632  
  
⊙  
阅读时长：约7分钟  
  
  
注：  
本文仅供安全研究与学习之用，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，由使用者承担全部法律及连带责任，作者及发布者不承担任何法律及连带责任  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWhsVYfr3PwkLGFZ0FmNEX7zggeMXLUy8qFbzh3dNSx4qom5TrnWcTf3hAK5c0JIKgRezicIBr0weww/640?wx_fmt=gif&from=appmsg "")  
  
**前言**  
  
本工具是一个基于 Java Swing 开发的 GUI 工具，用于检测和利用 CVE-2025-55182 漏洞。该漏洞是 Next.js & React框架中的一个安全漏洞，允许攻击者通过特定的请求构造执行任意命令。最近测试遇到了需要检测  
CVE-2025-  
55182漏洞的场景，搜索到了darkfiv大佬的  
ReactExploitGUI项目，感觉挺好的，但是只有exe形式，因此用java swing重写了工具。  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWhsVYfr3PwkLGFZ0FmNEX7zggeMXLUy8qFbzh3dNSx4qom5TrnWcTf3hAK5c0JIKgRezicIBr0weww/640?wx_fmt=gif&from=appmsg "")  
  
**主要功能**  
  
1. 漏洞检测  
  
- 支持批量 URL 扫描  
  
- 多线程并发扫描，提高检测速度  
  
- 支持导入文件批量检测  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WBVPm7dX44icOxnHljPhFXmdufbHsNOZf84cuFkBk5ZhAicicH0gibDpWd4OcWzWyyrLibibhrkSVPlCtUA/640?wx_fmt=png&from=appmsg "")  
  
  
2. 命令执行  
  
  
- 支持执行 Linux/Windows 命令  
  
  
- 自动或手动选择目标操作系统  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WBVPm7dX44icOxnHljPhFXmdxPkbRaK0P8OPUTN9QqIpqD6pbDvrNyIScrSGwDh9kVpLrH2EttsWjQ/640?wx_fmt=png&from=appmsg "")  
  
  
3. 内存马注入  
  
  
- 支持注入简单命令执行内存马  
  
  
- 支持注入哥斯拉内存马  
  
  
使用哥斯拉连接前，请先下载配套连接插件：  
  
  
https://github.com/BeichenDream/GodzillaNodeJsPayload  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WBVPm7dX44icOxnHljPhFXmdSBDHoVbdA3gGm43FeRJv5JTaZT39TNjdEFHt83Kd4XkWian75P4fVkw/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WDXIfrLozuPWyEklPmx44D9ZVEJOugeHnBBpA7dGThFtLNddZJAIXmPZ1fnXcueRSf6DSzX0KOFsA/640?wx_fmt=png&from=appmsg "")  
  
  
4. 反弹 Shell  
  
  
- 支持 Linux/Windows 反弹 Shell  
  
  
- 支持自定义反弹命令  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WBVPm7dX44icOxnHljPhFXmd4vvuIqbNHViaWRiavUv4DIN2ZFDRDTLAWLPP6RY6gJECA19Kp5w7yFQg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/tibkFsH2X2WDXIfrLozuPWyEklPmx44D9NXJccdYrYicFozPHJ6eEfnPJ1FGL4CPdp6Egqy2iaKpo0hymMPWb9IBw/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWhsVYfr3PwkLGFZ0FmNEX7zggeMXLUy8qFbzh3dNSx4qom5TrnWcTf3hAK5c0JIKgRezicIBr0weww/640?wx_fmt=gif&from=appmsg "")  
  
**项目地址**  
  
https://github.com/Faithtiannn/CVE-2025-55182  
  
本项目参考了以下开源项目，感谢大佬：  
  
[ReactExploitGUI](https://github.com/darkfiv/ReactExploitGUI)   
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWhsVYfr3PwkLGFZ0FmNEX7zggeMXLUy8qFbzh3dNSx4qom5TrnWcTf3hAK5c0JIKgRezicIBr0weww/640?wx_fmt=gif&from=appmsg "")  
  
**安全提示**  
  
- 本工具仅用于企业内部安全测试和授权的渗透测试  
  
- 使用本工具时请遵守相关法律法规  
  
- 禁止对未授权的目标使用本工具  
  
- 测试完成后，请及时清理注入的内存马  
  
本工具仅供学习和授权的安全测试使用，请勿用于非法用途。使用本工具造成的任何后果，由使用者自行承担。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgwIJ2svTmD3XNSpBFCOE6gibb1htNqzfLXzmP84LEa6mO3ia2pU4h4DcmgYqQRoaveLfHicICiaFKAsg/640?wx_fmt=gif&from=appmsg "")  
  
  
**END**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgwIJ2svTmD3XNSpBFCOE6gibb1htNqzfLXzmP84LEa6mO3ia2pU4h4DcmgYqQRoaveLfHicICiaFKAsg/640?wx_fmt=gif&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaZvzQWGW8xGYma0eamqH2lV0u8S7elr8u3INibfYv25JMIaTQV20VtcK6AD6UxppuzNSmAic18pG3g/640?wx_fmt=gif "")  
  
01  
  
[你的电脑里，有个"内鬼"叫DLL-----DLL劫持学习](https://mp.weixin.qq.com/s?__biz=Mzk2NDAwNzczNg==&mid=2247483940&idx=1&sn=bdb67ffcfa9fc6a94075f4f8b3e6ba5b&scene=21#wechat_redirect)  
  
  
  
  
02  
  
[AI渗透测试框架使用](https://mp.weixin.qq.com/s?__biz=Mzk2NDAwNzczNg==&mid=2247483891&idx=1&sn=000106066ba122d65230c48cb0ff1ce6&scene=21#wechat_redirect)  
  
  
  
  
03  
  
[Frida App测试（android）](https://mp.weixin.qq.com/s?__biz=Mzk2NDAwNzczNg==&mid=2247483865&idx=1&sn=5a67c701b0399117808e9b2b3ad188f4&scene=21#wechat_redirect)  
  
  
  
  
  
