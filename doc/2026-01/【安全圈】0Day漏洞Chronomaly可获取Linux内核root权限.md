#  【安全圈】0Day漏洞Chronomaly可获取Linux内核root权限  
 安全圈   2026-01-07 11:00  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
漏洞  
  
  
![banner](https://mmbiz.qpic.cn/sz_mmbiz_jpg/aBHpjnrGyljCiaBs266N3yUHtjyakZzFbyQcrLEpfFULm2zYoDrUQbHs53WNlUgnMHA2Y3m2Nnvicy8xku9ZMh6Q/640?wx_fmt=jpeg&from=appmsg "")  
网络安全研究员farazsth98披露了Linux内核中一个可被攻击者用于提权的安全漏洞新发现。该漏洞编号为CVE-2025-38352（CVSS评分7.4），是Android内核深处的一个高危权限提升（EoP）漏洞。虽然该漏洞最早于2025年7月披露，但现已被武器化。  
  
该漏洞是内核POSIX CPU计时器中的竞态条件问题，具体会破坏任务清理流程，使攻击者能够触发内存损坏。成功利用此漏洞可能导致系统崩溃、拒绝服务以及权限提升。  
  
研究人员已针对该漏洞开发出名为"Chronomaly"的公开PoC。该PoC由研究员farazsth98编写，主要针对Linux内核v5.10.157，但专家警告称它"应该适用于所有存在漏洞的v5.10.x内核"，因为它不依赖特定的内核内存偏移量。这使得它成为本地攻击者获取root权限的高度可移植且危险的武器。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/aBHpjnrGyljCiaBs266N3yUHtjyakZzFbuFyV67RK4Oshw3gic2laicWicDndWjjDqmv2W7KqgtFyvqXoNwpgUxWWA/640?wx_fmt=jpeg "")  
  
谷歌的安全公告证实，该漏洞已在0Day攻击中被检测到——这意味着黑客在补丁公开发布前就已开始利用。"有迹象表明以下漏洞可能正遭受有限的针对性攻击"，这表明高级威胁组织可能正在针对特定高价值目标使用它们。  
  
2025年9月的更新将安全补丁级别提升至2025-09-05，修复了该0Day漏洞以及数十个其他漏洞。强烈建议用户立即检查更新。  
  
  
 END   
  
  
阅读推荐  
  
  
[【安全圈】百度网盘出 Bug 开屏页显示小说内容，百度回应不是安全问题是苹果商店更新提醒功能显示错](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073585&idx=1&sn=0a3f98f5c7dcbd74e3877c0b769585cd&scene=21#wechat_redirect)  
  
  
  
[【安全圈】俄罗斯关联黑客滥用 Viber 攻击乌克兰军方和政府机构](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073585&idx=2&sn=f5a6587ade873e9799a66d967838f87b&scene=21#wechat_redirect)  
  
  
  
[【安全圈】WhatsApp 漏洞泄露用户元数据，包含设备操作系统信息](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073585&idx=3&sn=cc23d3a13250549d9a85b4399df1b886&scene=21#wechat_redirect)  
  
  
  
[【安全圈】威胁行为者声称泄露 NordVPN Salesforce 数据库及源代码](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073585&idx=4&sn=ce36b82edc5a856930b413e719701e26&scene=21#wechat_redirect)  
  
  
  
  
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
  
  
  
  
