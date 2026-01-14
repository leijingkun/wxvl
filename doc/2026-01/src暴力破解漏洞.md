#  src|暴力破解漏洞  
原创 simple安全团队  simple安全团队   2026-01-14 06:31  
  
免责声明：本文所涉及的任何技术、信息或工具，仅供学习和参考之用。请勿利用本文提供的信息从事任何违法活动或不当行为。任何因使用本文所提供的信息或工具而导致的损失、后果或不良影响，均由使用者个人承担责任，与本文作者无关。作者不对任何因使用本文信息或工具而产生的损失或后果承担任何责任。使用本文所提供的信息或工具即视为同意本免责声明，并承诺遵守相关法律法规和道德规范。  
  
**前言**  
  
在挖掘src的过程中，碰到最多的页面想必就是登录页面了，若是碰上登录页面没有图形验证码的情况，离挖到漏洞就不远了，今天分享一个登录口暴力破解+弱口令的漏洞。  
  
  
**漏洞挖掘**  
  
1、访问系统登录页面，看到没有图形验证码，看样子像是能爆破  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icjPTM4gYeU88uCJAhpsaKWKUX6IqHTL86OlJ8C0MmkUwNtx4l8OmytzIfsUTgDd7gdOKhrAOMkkYkkqSDWVusQ/640?wx_fmt=png&from=appmsg "")  
  
2、输入用户名密码，登录，抓取请求数据包，确实没有验证码，有戏  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icjPTM4gYeU88uCJAhpsaKWKUX6IqHTL8PFYQ9ZDnqbtjDPmUMqy2Kfdnay0qQwhBGJXWZCic7e1Q4rsvLSZbiavg/640?wx_fmt=png&from=appmsg "")  
  
3、重放10次登录请求包，并未限制账号登录，说明存在暴力破解漏洞  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icjPTM4gYeU88uCJAhpsaKWKUX6IqHTL8p1c7PjpOFcRqSR5g8RLnDhXib8FEFsu8Z8icH305DOE92OKd38YQeqmw/640?wx_fmt=png&from=appmsg "")  
  
4、利用burpsuite的intruder模块对账号密码进行爆破  
  
四种爆破模式介绍：  
  
1）  
Sniper attack：用一个字典，针对一个位置进行爆破。  
  
2）  
Battering ram attack  
：用一个字典，针对所有位置进行爆破。  
  
3）  
Pitchfork attack  
：每个位置有每个位置对应的字典，比如用户名位置处配置用户名字典，密码位置处配置密码字典，爆破的时候会一行一行对应。  
  
4）  
Cluster bomb attack  
：每个位置有每个位置对应的字典，区别是会进行排列组合爆破，比如用户名字典中的一行会搭配密码字典中的所有行进行爆破。  
  
  
这里使用Sniper attack爆破模式，将密码固定为123456，然后对用户名进行爆破，用一个top500的用户名字典：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icjPTM4gYeU88uCJAhpsaKWKUX6IqHTL8RNBPRBibdlPStPQ62ZrMRz0CveIBYpyxibxvYmSM7bf56q5oWibMcTLrQ/640?wx_fmt=png&from=appmsg "")  
  
爆破成功的响应包长度与爆破失败的  
响应包长度有明显区别，可以根据这个迅速判断出哪些账号被爆破成功了。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icjPTM4gYeU88uCJAhpsaKWKUX6IqHTL8HhIL79fdzP29uDxJfh2Nfia9qfTAQswJ4WxAyI7Ppt1X6FTP9wicFErQ/640?wx_fmt=png&from=appmsg "")  
  
****  
**END**  
  
  
查看更多精彩内容，关注  
simple安全团队  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/icjPTM4gYeU9knqRr6SqMfGqvUw1mPvauicyfafmT09zDhrasdngVtzibclFPMmiaXwxvYycfFHZmDsXtiaS5WJ9KEQ/640?wx_fmt=gif "")  
  
