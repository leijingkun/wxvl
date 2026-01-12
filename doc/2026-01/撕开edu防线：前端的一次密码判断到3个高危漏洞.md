#  撕开edu防线：前端的一次密码判断到3个高危漏洞  
原创 zkaq-eagle1  掌控安全EDU   2026-01-12 09:07  
  
# 一、任意文件下载  
  
在一次在 edu 网站逛街时，偶然发现了这个页面，呕吼，没见过诶，果断上网查一查。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZeZF7T7XjOQuVAjyKAve1lVzo2oz0QHbvWMwS56v0PGnLEziapp14YDg/640?wx_fmt=png&from=appmsg "")  
  
搜着搜着偶然看到一个链接，任意文件下载？  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZP66yVbxWSZjpm8yT606LVPyMAgMDxI6tID95RiagwqYPP3YZiaJOnLlA/640?wx_fmt=png&from=appmsg "")  
  
我们直接去构造请求包，竟然直接成功啦 ，怎么这么轻松？本来打算直接提交了，想了想这么简单，肯定还有其他的洞  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZsnbFkhwicHPP7cX9MiaJuNzRaMmoTc4CI2DC5JlCY5P6Y4BShk7G8PUg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZrQe0PmtXke1CdibMpvpybX3sKuUSRmfCOP1lGib7FGeFauZhpNia79rEA/640?wx_fmt=png&from=appmsg "")  
# 二、前端的一次密码判断到 SQL 注入  
  
SQL 注入真是测试了好长时间才测试出来，呜呜呜  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZjiaaMNtdFFObHQrqibdgKGh66cl6awYdEMba38ChQr9FB4S7GeAbpyJQ/640?wx_fmt=png&from=appmsg "")  
  
通过一组信息泄露，我们成功登录到了系统，因为我开着 BP，一些页面没有的参数在 BP 中可以看到。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZQaDwr9Ojp3aFnkw2e0TvsI7AEqcyTHgNZyDDQW5RN2TumL6c35BzSA/640?wx_fmt=png&from=appmsg "")  
  
由于页面内有信息，这里截了一张加载不全的图，我们直接去 BP 中的 history 找历史包  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZT9CNKuzYiaxicnCiccaEJMEeEVMzs2J3ibWJLk7pTzKd80wGdp7CstageA/640?wx_fmt=png&from=appmsg "")  
  
这里使用了一个 BP 插件，我觉得非常好用  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZdzibiaTkvJJbjsXQlSI7icHkV34Rzpx68HvConUsaINibFrYltHhnxGGkw/640?wx_fmt=png&from=appmsg "")  
  
  
**这里插入一条关于这个 SQL 注入的烦恼！**  
  
大家能猜到是什么吗？  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZYUdjwDdYe8ZxfsYcHCdia5ibmrtnZ9I8wVq7uKL7weLSQichjdXTFzic9Q/640?wx_fmt=png&from=appmsg "")  
  
就是这是一个 Orcal 数据库，我用 MySQL 的语法试了半年，没试出来，多亏了 AI 呜呜呜。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0ZSl5a5wgwFIgChicqvJVNCiaErpdRQ7yoZ3ghQeFCooj8GrAmaUFOZRPw/640?wx_fmt=png&from=appmsg "")  
# 三、数据库弱口令  
  
这里悄悄用了一下扫描器，因为前面测出来 Oracle 数据库信息，这里直接弱口令扫了一下，没想到扫出来了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcpg5FrMdwnIg476E6fAjP0Z5icwMAEukWLPiaoKq0ntKwBfp5ldkUW5gZkyF0vuFsiazyjQCzcswdqgA/640?wx_fmt=png&from=appmsg "")  
  
这里用数据库连接工具一直连不上Navicat 和其他工具都连不上。经过查询才知道 Oracle 数据库连接必须指定一个身份登录，我这里用的官方工具。  
# 四、小总结  
- 做渗透测试要细心，不放过任何一个可能点，说不定就会有 nday 出现，包括一个默认页面。  
- AI 目前是个不错的好帮手，我有很多问题都是问的 AI 帮忙解决的。  
- 系统初始密码一定要修改、有漏洞一定要修复与升级。  
- 以上内容均已修复，仅作学习交流，请勿恶意使用。  
