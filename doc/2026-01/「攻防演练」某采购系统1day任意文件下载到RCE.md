#  「攻防演练」某采购系统1day任意文件下载到RCE  
Alivin  神农Sec   2026-01-08 01:00  
  
扫码加圈子  
  
获内部资料  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXLicr9MthUBGib1nvDibDT4r6iaK4cQvn56iako5nUwJ9MGiaXFdhNMurGdFLqbD9Rs3QxGrHTAsWKmc1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=png&wxfrom=13&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
#   
  
专注于SRC漏洞挖掘、红蓝对抗、渗透测试、代码审计JS逆向，CNVD和EDUSRC漏洞挖掘，以及工具分享、前沿信息分享、POC、EXP分享。不定期分享各种好玩的项目及好用的工具，欢迎关注。加内部圈子，文末有彩蛋（知识星球优惠卷）。  
#   
  
文章作者：  
Alivin  
  
文章来源：https://forum.butian.net/share/1792  
  
01  
  
0x1   
某采购系统1day任意文件下载到RCE  
# 0x01 前言  
  
某1day漏洞，从拿代码到审计getshell，全历程。后来review发现影响很多站点。  
# 0x02 黑盒测试任意文件下载  
  
前台看到有相关附件下载，如下：  
  
![-w870](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp1qpiatyceegqyyPjicM1b7SHwwqSkyGd6SpUYHIowbWw6Iogt6rtOChQ/640?wx_fmt=png&from=appmsg "")  
  
点击下载功能，发现URL长这样：  
```
/xxxx/downloadFiles?downloadInfo={files:['uploadfiles/bd/doc/xxxxxx.pdf']}

```  
  
直觉告诉我这里存在任意文件下载，于是进行测试。  
```
/xxxxx/downloadFiles?downloadInfo={files:[%27/WEB-INF/web.xml%27]}

```  
  
![-w374](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANphDhQNwdWWHpzZ9jclODaKQ2T2xzwuTonTdpJS0knUXOUIYRChethng/640?wx_fmt=jpeg&from=appmsg "")  
  
果然可以，然后分析web.xml  
文件，然后看具体代码位置。  
  
偶然看到该接口可以批量下载，如下：  
  
![-w692](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpaNVd3wTRqlic8otpiaINLoMibcibOich8BRDWcJuTBx9ytIMk1I3Wp8cbhg/640?wx_fmt=png&from=appmsg "")  
  
那先下载DownloadFilesServlet  
进行分析，看一下具体怎么批量下载。  
  
![-w1304](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpwICnEZocydQeFKh6jxuwOJicnWUqmc9mrtQ9ibYUzCZXHulnffHzuukg/640?wx_fmt=jpeg&from=appmsg "")  
  
结果却找不到该类，然后看了下还有其他批量下载的接口，比如：  
  
![-w680](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpmibcJUgfjArkicGTfqTiaMNO17XFJVeU8n8Sz8ibvXJ4Z7Pk4s8XMcjfxQ/640?wx_fmt=png&from=appmsg "")  
  
尝试下载，发现：  
  
![-w805](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpOD0wFFXncibRia2fdAanE4OjiaB4AvuknkNPty4hNUpgAicTRcv1DRCQ3w/640?wx_fmt=png&from=appmsg "")  
  
开整，分析。  
# 0x03 代码审计  
## 3.1 任意文件下载  
  
因为没有拿到DownloadFilesServlet  
 代码，所以只能分析替代品WcDownloadFilesServlet.class  
 代码了，，看看其怎么批量下载，然后拿到完整代码进行分析。  
  
（1）首先从前端传入downloadInfo  
，然后处理成json，如果json为空，提示下载失败。  
  
![-w1100](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpSftf5PKUszficVBjZpJrjvSTicyIKM1a4WLyWSTekrKqVE9wvGk3Qs0A/640?wx_fmt=jpeg&from=appmsg "")  
  
（2）84-86行代码中，把json解析好以后，传入了downLoadFiles  
函数。  
  
![-w552](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp1ZCQK8lonrZNOuUBmDLDPoReRyl0H0ACiaYhGJ1yIYpiaCoCNEyJj5xw/640?wx_fmt=jpeg&from=appmsg "")  
  
（3）跟进downLoadFiles  
函数的实现如下：  
  
![-w950](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpeUf5eNJG37o9FXO34Jfjur2FWL8vSpwXDelQBUAVf6ibDAQ4OZl2ufA/640?wx_fmt=jpeg&from=appmsg "")  
  
跟进下getAllFiles  
方法，看一下其获取的所有文件的方法。  
  
![-w757](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpO116lInibGhnAzITp1ml17hv3wlyiaENBynkmCUibyZvHLaJG0Mlaicmjw/640?wx_fmt=jpeg&from=appmsg "")  
  
概括一下：  
- 如果输入的内容中有?、*一类的字符，会认为你是通过正则类型匹配下载文件，然后解析正则，下载你想要的文件  
  
- 如果文件名不存在正则，那么就会拼接目录，递归解析目录，然后把所有目录中的文件的绝对路径都解析出来，形成一个数组fVector  
  
然后回到downLoadFiles  
中，如果返回的files.size()  
为1  
，则调用downSimple  
,即只下载单一代码。否则进行压缩后下载。  
  
![-w679](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpia5Aic4x9N44mZJ2tf10ibeicBNkv3KLxQBAAOYB6RVicHowSpxkSj9eiaAQ/640?wx_fmt=jpeg&from=appmsg "")  
  
（4）跟进downZipFile  
看一下其实现  
  
![-w1182](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpzU8NOYjNZkMqxdRibBqlUvic7ql7Sl0qxjzD5fKic2SvBAYbCAOfKPqYw/640?wx_fmt=jpeg&from=appmsg "")  
  
![-w1251](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpkfGFjeAIcvGezpSFhuV4R5C98wFWhCoOCHicTe9gmibbWmY73VPwEZdA/640?wx_fmt=jpeg&from=appmsg "")  
  
到此已经看到了下载压缩文件的全流程了。那么要利用，则只需要传递参数：/xxx/wcdownloadFiles?downloadInfo={files:['/WEB-INF/']}  
 ,即可打包下载WEB-INF  
目录了。压缩web根目录会超时，无法完成下载，所以只能退而求其次，下载WEB-INF  
目录，但该有的代码还是可以看到的。  
  
全部代码：  
  
![-w970](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp0iazMo3VK9dQd0g7WQ9DicibGlibibK4KXcIkCcJicfb4tnvUU0yTtaGV1tg/640?wx_fmt=jpeg&from=appmsg "")  
  
（5）上面没提到单一文件下载的downSimple  
，给大家看一下，没啥技术含量。  
  
![-w1040](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpLN1uJAMqDqo6QVyhGzr5taAdGwzZlQ8cITgWO3ibqhUQ9Bh1NqBcFfw/640?wx_fmt=jpeg&from=appmsg "")  
## 3.2 任意文件上传-  
  
有了完整的代码以后，导入idea，进行分析，侧重能拿权限的接口。  
  
![-w942](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpD4IbXFyTcnAbkAX03C6hMwkFWnqNlDmNOgO0AlrMTstiaf5ZEYKpH8g/640?wx_fmt=jpeg&from=appmsg "")  
  
找到对应代码进行分析如下：  
  
（1）获取Web根目录，然后拼接路径  
  
![-w1275](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpM9yvpxicwo8tGibEkWG3Bibyn47jfkOuZRF6eCxTBu4qIPXF8oZ8Mib93Q/640?wx_fmt=jpeg&from=appmsg "")  
  
此时目录为[Web Real Path]/uploadfiles/[savePath用户传入]  
  
（2）解析上传请求  
  
![-w843](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpn3SN6XZtUpXOBKV8dTzalNXoicMAaE0QEtleCCe2YIyVZlpcEGMnnYA/640?wx_fmt=jpeg&from=appmsg "")  
  
（3）文件保存  
  
![-w1121](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpFibZ3ZPgDiaQjlicWP2ibtkkYDK0eWCeNNYJmkiaZJXsiaqzZUOiadmwdtj4A/640?wx_fmt=jpeg&from=appmsg "")  
  
![-w618](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpr3utHNoIFUrLJN6qzTg4Xbibib1wkuDYz9GEhSjXGS5Qj4nGkpL1KGNQ/640?wx_fmt=jpeg&from=appmsg "")  
  
最后返回给前端。  
  
任意文件上传分析完成，然后进行测试，却....  
  
![-w1320](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpBQl7rUoCwygnCXZmGjJCHsmv6gPibvz0vYiaicRhEhyHiaGGibiajRXiaHZZQ/640?wx_fmt=jpeg&from=appmsg "")  
  
至少应该是显示上传失败才对啊。到这，这个洞派暂时派不上用场，但在内网却起到了很大作用。  
  
内网见到同样的CMS时，直接抄起poc就打，然后就成了。这台机器在我们后来的横向中起到了很关键的作用，因为密码是通用密码且能读到明文。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpbQF2NqFQV6X9lXwyk3RMwnWkZlB9xafEewz8Qia9TrZLa3Lnwn3JxTA/640?wx_fmt=png&from=appmsg "")  
  
后来测试，该漏洞在互联网上也是影响很多资产。  
## 3.3 任意文件上传2-webUploadServlet  
  
找这个接口，主要是我在前台用过，且真实可以上传文件，但不返回文件名，而且上传什么上去都是.png  
，所以进行分析。  
  
最开始没找他的代码的原因是因为class  
目录没这个类。  
  
![-w668](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpce8bwgdvZo74QcJjHzZesnlFxfLyT7GHOUfqbdlWuYmyd5zWyvXaAA/640?wx_fmt=jpeg&from=appmsg "")  
  
就很离谱，然后问了某大佬，说可能在依赖的jar包里面，于是在lib目录下找到了名称为项目名的jar包。果然找到了对应的代码，如下：  
  
在解析POST请求时，定义不同的action用于处理不同内容。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp3yOlhSRTibBQDxUK7oWrX5prm0dEQCuNLyRiaFdJEN5Itia3WUjWk8iaiaA/640?wx_fmt=png&from=appmsg "")  
  
首先先看下uploadFile  
的实现：  
  
（1）处理上传请求，具体如下：  
  
![-w1066](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpX4BksOZmt1FQ6tKZ3sndoRmPvWRu3DHkXYFYmp66iaOZUicuiad3kE2ibQ/640?wx_fmt=jpeg&from=appmsg "")  
  
（2）拼接路径，完成上传，具体逻辑如下：  
  
![-w929](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpWnqM9FMVpDbYzdasZGSzwGztO8Q6bGGMInibTJbVK5cetGwMmPRQs6w/640?wx_fmt=jpeg&from=appmsg "")  
  
其实这里面有个坑点，判断chunks  
不为空，则取chunk  
作为最终文件名，这怕是少写了个字母，最开始我没注意这点，走了很多弯路。  
  
（3）漏洞利用-1，只需要构造数据包，满足以上逻辑要求即可。  
  
![-w1289](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpXMdgonky2V0hZvEQC7Iv1hy3Rgiavx4uV9X2B2SxtkxpmYib9ULO19ZQ/640?wx_fmt=jpeg&from=appmsg "")  
  
然后shell地址为：/uploadfiles/attach/chunk/test/tst.jsp  
,测试：  
  
![-w1348](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpqFpcTCVxp8oMH0GGX5bia6o04fAoicA5mAz6Mr2cSrzeklRKHx6VsJwQ/640?wx_fmt=jpeg&from=appmsg "")  
## 3.4 任意文件上传2+文件移动接口  
  
上面提到说当时踩了chunks  
和chunk  
这个坑，当时是上传以后，死活也访问不到上传后的文件。  
  
然后发现有个action  
分支为：checkChunk  
，涉及文件操作，它是这样实现的：  
  
![-w987](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpGyM4jakhqp1FFNPeCLnOeaeVXWbTs0ab5qiaZbcUBicoibEx7JerdgZicg/640?wx_fmt=jpeg&from=appmsg "")  
  
把用户传入的fileUuid  
拼接进了路径，和我们上面看到上传里面的(String)var10.get(var10.get("id"))  
的值对应上，到var6  
这里就是同一个目录，然后接着 http请求、fileUuid  
的值和chunks  
的值被传入了mergeFile  
函数，跟进一下看看实现。  
  
![-w1116](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpH5mXSHDyQkGJptiaibtnTYY0RX8bI9bMIoPB7e89wtuwlZicNdzHic2BUg/640?wx_fmt=jpeg&from=appmsg "")  
  
![-w693](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpEVNWCVw2EpOrJwFcd1d04sxboGRx8mqv3Zsvjo5LqUAHficLeTT6F2g/640?wx_fmt=jpeg&from=appmsg "")  
  
所以，无论我们的storePath  
写什么内容，最后保存的文件名都可控，所以，这必然存在漏洞。  
  
利用方法：  
  
![-w1257](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp4RPPhy72Cj3JDQ4OibpoMhbWy4bTgpl8rP3kFbs9MXGx0A0ROsoyqVQ/640?wx_fmt=jpeg&from=appmsg "")  
  
此时因为没chunks  
，所以传的文件访问不到。  
  
![-w1305](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpRRfPuo0CiazOqCBficwlZJzOI5IjgVGgPn9ymHfuqa0rpq0IOkuYiau5Q/640?wx_fmt=jpeg&from=appmsg "")  
  
![-w1327](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANpvab7iavdhSFOicwa234AYKkjs6qPXQww5COibZXRYfiajxNxyO8G1dlC0Q/640?wx_fmt=jpeg&from=appmsg "")  
  
是这么存的，所以需要用到另一个接口进行移动文件，测试如下：  
  
![-w1361](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp5mDeHJsx6icycHHavo3brCTSwNIITVe9hxLUOfzmE2iczY52zyzowV7w/640?wx_fmt=jpeg&from=appmsg "")  
  
然后访问shell：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXtYNm62qFQKLFPjsB9iaANp20htTpk6ib2KkdLLNU6CzWz72zB9DvtaCplyUzmERLU1F8rD7eMUvicQ/640?wx_fmt=png&from=appmsg "")  
  
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
  
星球人数少于1400人 50元/年  
  
星球人数少于1600人 65元/年  
  
（新人优惠卷20，扫码或者私信我即可领取）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWXXxypODTreRYn1aKwInjsYxnFkfcLeKvNE9kEj6GLp4I2r5ZUOoWaqJqXdgLj44b2Mk84u3uRbQ/640?wx_fmt=png&from=appmsg "")  
  
欢迎加入星球一起交流，券后价仅50元！！！ 即将满1400人涨价  
  
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
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXB0ZRnKw5s176V4kZq0rxykrfBcW53oGHsniaCaOdlKlKoL9xyNq5Ka9HZPYQD2LsLEOYrVib6Mnvw/640?wx_fmt=jpeg&from=appmsg "")  
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
  
  
  
