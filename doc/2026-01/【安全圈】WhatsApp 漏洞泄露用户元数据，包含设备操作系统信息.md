#  【安全圈】WhatsApp 漏洞泄露用户元数据，包含设备操作系统信息  
 安全圈   2026-01-06 11:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
信息泄露  
  
  
WhatsApp的多设备加密协议长期存在元数据泄露问题，使得攻击者能够对用户设备的操作系统进行指纹识别，从而有助于精准投递恶意软件。近期研究显示Meta已进行部分修复，但透明度问题依然存在。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgjibmJHTxIiacssGuaeh7CiaRMt4uUsVSzSfFo3yka7iaHCz4mUgLOcylBNaRYXXEGmsKCppMCKRbWpA/640?wx_fmt=png&from=appmsg "")  
  
Meta旗下的WhatsApp拥有超过30亿月活跃用户，其使用端到端加密来保障消息安全；然而，其多设备功能却会暴露设备信息。  
  
在该设置下，发送方与接收方的每个设备建立独立会话，使用的是在设备而非服务器上生成的唯一加密密钥。  
  
密钥ID在实现上的差异，会揭示设备运行的是Android还是iOS系统，这对网络攻击链中的侦查环节至关重要。  
  
**攻击者可被动利用此漏洞，无需用户交互即可通过查询WhatsApp服务器获取会话密钥，从而识别操作系统类型，进而向Android设备部署精准漏洞利用程序和恶意软件，同时避免触及iOS设备或惊动受害者。**  
  
2024年初，Tal A. Be’ery在WOOT’24上的研究揭露了基于Signal协议实现的、按设备建立的会话会泄露设备数量、类型和身份信息。  
  
同年晚些时候，攻击者已能精确定位特定设备进行漏洞利用。2025年，Gabriel Karl Gegenhuber等人在WOOT’25上详细阐述了操作系统指纹识别方法：Android的签名预密钥ID每月从0开始缓慢递增，而iOS的模式则截然不同。  
  
Tal A. Be’ery使用定制工具验证了这一点，确认攻击者可将这些泄露信息串联起来：先检测操作系统，然后悄无声息地投递针对特定操作系统的恶意负载。  
  
近期，WhatsApp已将Android签名预密钥ID的分配方式更改为在整个24位范围内随机取值，从而阻断了该攻击途径。这一变动是通过监测工具发现的，标志着Meta改变了先前认为此问题”无法采取行动”的立场。  
  
然而，一次性预密钥仍然具有区分度：**iOS的ID起始值较低且每隔几天递增，而Android则使用完全随机的范围。**  
修复后经过调整的工具仍能可靠地检测出操作系统。  
  
这使得高级持续性威胁能够利用WhatsApp作为传播恶意软件的渠道，正如Paragon间谍软件案例所示。查询过程中不会向用户发出任何通知，从而保持了攻击的隐蔽性。  
  
批评者指出，此次修复措施的推出并未像处理另一个类似问题那样，向研究人员发出警报、提供漏洞赏金或分配CVE编号。CVE通过CVSS评分记录问题，而非用于指责；此类疏漏阻碍了问题的追踪。  
  
虽然修复措施在持续改进，但实现跨平台的完全随机化并提高CVE透明度，将能更好地保护数十亿用户，促进社区协作。在当前风险持续存在的情况下，用户应限制关联设备数量并监控账户活动。  
  
  
  END    
  
  
阅读推荐  
  
  
[【安全圈】《英雄联盟》证书过期登热搜！草台班子闯大祸？](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=1&sn=d369456917b21dd3d87047f028e3d9fa&scene=21#wechat_redirect)  
  
  
  
[【安全圈】黑客入侵员工网络时落入Resecurity蜜罐陷阱](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=2&sn=56bf0dc5381b8bb1a65ba9902ce5aeb2&scene=21#wechat_redirect)  
  
  
  
[【安全圈】自传播恶意软件GlassWorm利用VS Code扩展攻击macOS用户](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=3&sn=74679f09ae68daf09fdc27f17f57c6e9&scene=21#wechat_redirect)  
  
  
  
[【安全圈】别乱用！下载的Skills竟成后门，聊聊Skills不为人知的安全风险](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=4&sn=0e19f07eec0b7cf1b18c9a05a9f2e9f7&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEDQIyPYpjfp0XDaaKjeaU6YdFae1iagIvFmFb4djeiahnUy2jBnxkMbaw/640?wx_fmt=png "")  
  
**安全圈**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
←扫码关注我们  
  
**网罗圈内热点 专注网络安全**  
  
**实时资讯一手掌握！**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
**好看你就分享 有用就点个赞**  
  
**支持「****安全圈」就点个三连吧！**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
  
  
