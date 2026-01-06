#  手把手CVE漏洞编号获取&漏洞提交  
原创 神农Sec  神农Sec   2026-01-06 01:00  
  
扫码加圈子  
  
获内部资料  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXLicr9MthUBGib1nvDibDT4r6iaK4cQvn56iako5nUwJ9MGiaXFdhNMurGdFLqbD9Rs3QxGrHTAsWKmc1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=png&wxfrom=13&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
#   
  
专注于SRC漏洞挖掘、红蓝对抗、渗透测试、代码审计JS逆向，CNVD和EDUSRC漏洞挖掘，以及工具分享、前沿信息分享、POC、EXP分享。不定期分享各种好玩的项目及好用的工具，欢迎关注。加内部圈子，文末有彩蛋（知识星球优惠卷）。  
#   
  
  
01  
  
0x1   
手把手CVE漏洞编号获取&漏洞提交  
## 一、 CVE漏洞简介  
  
CVE（Common Vulnerabilities and Exposures）的全称是公共漏洞和暴露，是公开披露的网络安全漏洞列表。IT人员、安全研究人员查阅CVE获取漏洞的详细信息，进而根据漏洞评分确定漏洞解决的优先级。  
  
在CVE中，每个漏洞按CVE-2024-0067、CVE-2024-10011、CVE-2024-10021这样的形式编号。CVE编号是识别漏洞的唯一标识符。CVE编号由CVE编号机构（CVE Numbering Authority，CNA）分配，CVE编号机构主要由IT供应商、安全厂商和安全研究组织承担。  
  
CVE官方网站链接地址：https://www.cve.org/  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxuqS2YN9ugakiapmt7jF4LWKKhlABdhJxhckMoSrEJagic0BmP02bAYng/640?wx_fmt=png&from=appmsg "null")  
  
##   
  
  
## 二、 如何找CVE对应的资产  
#### 1、CVE和CNA平台  
  
CVE的资产范围，其实主要是看你提交到CVE对应的哪个平台，最开始只是CVE官方平台，到后面随着漏洞提交数量变多，CVE平台就增加了很多下面的单位，也有对应的审核CVE漏洞并且给对应CVE漏洞编号的权利。但是随着漏洞数量的增加，MITRE将漏洞编号的赋予工作转移到了其CNA（CVE Numbering Authorities）成员。CNA涵盖5类成员。目前共有69家成员单位。  
  
1.MITRE：可为所有漏洞赋予CVE编号；  
  
2.软件或设备厂商：如Apple、Check Point、ZTE为所报告的他们自身的漏洞分配CVE ID。该类成员目前占总数的80%以上。  
  
3.研究机构：如Airbus，可以为第三方产品漏洞分配编号；  
  
4.漏洞奖金项目：如HackerOne，为其覆盖的漏洞赋予CVE编号；  
  
5.国家级或行业级CERT：如ICS-CERT、CERT/CC，与其漏洞协调角色相关的漏洞。  
  
但是不同的下面单位，收录的公司资产范围不一样，就比如国内常用于提交CVE的VDB平台：https://vuldb.com/zh/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxiaFR7dnXrDGxKW34ya9YQM4n15Sy1UELqKMqIuBSxSjicuPBYZuYgeFQ/640?wx_fmt=png&from=appmsg "null")  
  
我在这个平台提交了很多通用型的漏洞，小公司的都没有收，且漏洞危害不行的也没有收，收了我几个通用型高危漏洞，比如：SQL注入、文件上传之类的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx1hKibXvXsIEibiateb6M4TY423v4I0q9T2DE5CxnE5BF6kdxdywJEenVw/640?wx_fmt=png&from=appmsg "null")  
  
给师傅们看下我被拒绝的都是什么理由，反正目前我收到的都是说资产不属于他们这边的，让我提交到CVE官方平台审核，但是一般直接提交到CVE那边，审核时间周期比较长，相对于这个提交到CNA这些下面单位，审核速度会比较快，一般1-2个星期就可以审核了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx2xibiboO8XyPnsKGeFqdnwX3t0A1rNGs3MO7RVRjR2eVcltx9xnMLISQ/640?wx_fmt=png&from=appmsg "null")  
### 2、找什么资产漏洞  
#### CMS源代码审计漏洞  
  
首先网上很多思路都是说在github上寻找开源cms进行代码审计 ，或者百度查找开源cms，进行代码审计。这个很多人都是这样的思路，那么肯定很多漏洞被挖了，且这个要考验师傅们的代码审计功底，这对很多白帽师傅不是很擅长。  
  
但是这里我前面还是会给师傅们分享下如何找网上公开的 cms框架系统进行代码审计，最近的的方法就是直接在GitHub搜cms即可，下面都是github开源可以下载的源码，且对应的各种不同的语言，看师傅们自己选择即可了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx5BKia5sQptMEHS0yFUOgsNJ6iaLznZWOkvJKpHAxFjoicDERbia8r2DehQ/640?wx_fmt=png&from=appmsg "null")  
  
随便点进去一个，下载对应的CMS系统源码即可，从而审计或者直接交黑盒测试的POC都可以获得CVE编号。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxskeu64ibj7Bthy6DXX1QhRmcy1zzN6UKXnt53Lzribuzg5xwFjZ1zFkw/640?wx_fmt=png&from=appmsg "null")  
  
这里分享几个别的平台下载源码的平台，供师傅们使用：  
1. https://down.chinaz.com/class/5_1.htm  
  
1. https://www.yydsym.com/code  
  
1. https://www.jeecg.com/doc/download  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxVEdu0ErLhDiaWOMDJWJBVaIO0Mv6XjEROVqeSRtZYOPNsrUp6sGVFpA/640?wx_fmt=png&from=appmsg "null")  
  
还有就是直接使用Google浏览器、百度浏览器搜：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx2NQAL7um0Cvzxk5wpicWlo5ZiaVCK7ylMDD8IgHibibV1qqM8UFibRtgA3g/640?wx_fmt=png&from=appmsg "null")  
#### CNVD思路  
  
其实挖CVE跟CNVD差不多的，只不过对于一些资产要求在国外有些影响力的那种资产才收，跟CNVD要求提交的通用型漏洞得满足实缴5000万资产一样的，这里拿国内某大公司CRM系统来举例，这个公司资产大家可以去CVE搜下，肯定是有对应的CVE漏洞的提交记录。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxsUeNQiao4LKatiaCS7JiadUZ6HsI3LzROJ3eRUIekCt03ZaLbgGw3Httw/640?wx_fmt=png&from=appmsg "null")  
  
所以大家要是不知道这个公司在CVE收不收的情况下，大家可以先理由Google浏览器搜索下。  
  
还有一种就是可以直接拿exploit-db漏洞平台和CVE官方平台进行搜索，看看有没有人交，且看看有没有人提交对应系统的漏洞，这样就可以避免漏洞的重复了。  
  
exploit-db漏洞平台地址：https://www.exploit-db.com/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx4thJic53EV58x35Y0A9UIa1UfNvqQRpbAEOcJv5OicoTAVkfjN5ibPEtA/640?wx_fmt=png&from=appmsg "null")  
  
相关查重问题在CVE官方平台,链接：https://www.cve.org/CVERecord/SearchResults?query=  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx5sEnJ6e2WqCznp0zMlQNv696ggugdlYzbLrdylCOUCQRRZIZq0uRWw/640?wx_fmt=png&from=appmsg "null")  
  
对于资产，师傅们可以参考CNVD的来，像下面的 好几个亿的资产缴纳，且公司的影响范围比较大，相关系统资产也多，大概率那边都是收的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx2VUSibibhF63dWicnfp4KoDA4AE10ss6oDQD9KvibkWMDZgg4zyyVGQXAw/640?wx_fmt=png&from=appmsg "null")  
  
然后找对应的公司资产的话，就可以直接在知识产权里面找，例如下图：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxAoannsCZJKXlzd6ZCPiaFK6I4zINrfdBFCaLvbgnZLc7ice78Aiax9xibQ/640?wx_fmt=png&from=appmsg "null")  
  
像这里面的系统都是可以进行测试的，一般都是可以利用空间搜素引擎进行检索，然后去挨个找漏洞，找到了就可以再去利用搜素引擎进行检索关键字进行模糊匹配，然后打个通杀漏洞，就可以提交给CVE平台了。  
  
像这个公司的一个CRM系统资产，在国外也有蛮多的系统使用，提交到CVE基本上都是会收的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxo84J6bhTJWx0MMGriaroFiaKQBicIiadsMmrianc0g39pmCNOoQHWWX08WQ/640?wx_fmt=png&from=appmsg "null")  
## 三、 GitHub创建public漏洞复现文档  
  
打开GitHub地址：https://github.com/，先使用自己的个人信息邮箱等注册GitHub的账号。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxDxmbGf54fppm5VZ7BQ0bIQicBkZhicOiaLpWlAR8opIyG9QrOPMeb46LQ/640?wx_fmt=png&from=appmsg "null")  
  
先在GitHub注册完成以后，然后在主页的右边点击新建仓库。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxbCAnicro7930r4S8W6YuYF4U68x7e0ibwKzSGWbwCeB8SctfAl4ibicwDw/640?wx_fmt=png&from=appmsg "null")  
  
新建一个以CVE命名的仓库即可，这里最重要的就是下面要选择公共可见，也就是public。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx4K9C4DkibFNN2WR7nFErDU6z4KkwrwdBZr07pyc7vbNSlQ2fytmB2YA/640?wx_fmt=png&from=appmsg "null")  
  
然后点击议题，再点击创建议题即可进行写漏洞复现报告了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxjfrgdYUqUPUNLxFazW4CXQhZxziac4jXnZIF1louHs86TcyQKsFa8Ww/640?wx_fmt=png&from=appmsg "null")  
  
但是漏洞复现的报告必须得是全英文的，因为老外看不懂中文。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxlKVCIiauTuBJNFnj0aCYkpv1ibLAhyTmib2ibETts4TOObsuhgG5fuAulg/640?wx_fmt=png&from=appmsg "null")  
  
写完英文版本的漏洞复现以后，直接选择这个的复制，后面需要提交GitHub的漏洞复现地址。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxChmfPGDiciawlCnq8Hd8gpkiaszTLGuhraKtF2G0SjZTljbjZyqnib6rUw/640?wx_fmt=png&from=appmsg "null")  
## 四、 CVE漏洞提交  
### 1、CVE官方平台提交  
  
我这里在文章中会给师傅们手把手教两种平台方式提交的步骤，都非常详细，其中也包括我踩过的一些坑等。第一种先介绍下直接在CVE官方平台提交，网上这个方法也是最多的，因为最先开始就是在这个平台提交，但是这个提交审核流程周期很长，优点是收录的公司资产范围广。漏洞提交网站：https://cveform.mitre.org/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxvb3iaVWLF2NI4OhPhxticKy5SLxOLGzbYibT9nS4joyL7oBvgz4M1569w/640?wx_fmt=png&from=appmsg "null")  
  
申请表中会要求填写漏洞类型、产品厂商、受影响产品或代码及版本、厂商是否已确认该漏洞、攻击类型、影响类型、受影响组件、攻击方法或利用方法、CVE描述建议等。  
  
1、选择下面这个，意思是填申请表申请CVE编号，然后下面填写自己的邮箱。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxEJz3nxDu5QpgpPrepyo5sL5kiauXXENJ9f8fcLpbPILOVeoossQWDrQ/640?wx_fmt=png&from=appmsg "null")  
  
2、填写1，也就是申请一个CVE编号，下面两个都勾选上。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxczN0oochOUMR3rHh42thfcWZT6iaBlibZzjficUiaialk5eX9Uy5O7FdiapQ/640?wx_fmt=png&from=appmsg "null")  
  
3、提交对应漏洞详细信息，具体看截屏。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxQUGXLJOEiavMJAYBmccIfM1vY5v8xVQvicx5JD2IQMKicaeicnMmkpmxFg/640?wx_fmt=png&from=appmsg "null")  
  
4、再下一步选择内容，看下面的截屏。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxRJzZXCo0PlATwTw1ibgMM70vsFahtRBTFj7LTiaQ6Ha0baObbZzOWT8A/640?wx_fmt=png&from=appmsg "null")  
  
5、这里最重要的就是GitHub的漏洞复现地址，建议使用GitHub的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxG4LWYtdibNLYoiclAetq7yBaibpwZshsG99kibCfmgd5vTVYWqN0xpmyyg/640?wx_fmt=png&from=appmsg "null")  
  
填写好验证码提交即可 ，提交后一般马上会收到官方的确认邮件，说明那边已经收到你的邮件了，后面等着官方进行CVE漏洞审核即可。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxSSibpj80hj1Z1BmK79s9WUXeP3IyoFl7HDfMDyY1LJeVlpdKstuy7XA/640?wx_fmt=png&from=appmsg "null")  
### 2、VDB(CNA成员单位)平台提交  
  
这里第二种提交方式就是在VDB平台提交，网站地址：https://vuldb.com/zh/。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxtiakViaOJfbxTmDiauU2bedwwFCGUCxmIVD2rj7RTuvviaW8RypicF7RticA/640?wx_fmt=png&from=appmsg "null")  
  
点击下面的添加按钮，进行CVE漏洞提交。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxp9y2qhAibr8cyPtOOlfqxJibANUUY1LHsriaxicG30T4W1jTqFUU2jwZQA/640?wx_fmt=png&from=appmsg "null")  
  
这里因为是中文了，跟上面添加CVE漏洞一样，最下面的描述就是填漏洞的一个简单描述，用于后面CVE漏洞描述。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxg69FvQcxSibcXapsLOReb8egrBltmGic0AEgNtz6qmE1drfYVtUS9S3A/640?wx_fmt=png&from=appmsg "null")  
  
最下面这个直接填写GitHub的漏洞复现地址 即可。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxicgJ1lbfNicVhlu4pUhORuVYHKeylzDOUbLPFLJWicZzUicbYHEGI8VuqQ/640?wx_fmt=png&from=appmsg "null")  
  
提交完漏洞后，VDB官方会发邮件过来。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx77NSpyvanYYZTTiawAib62wD8FVvlOPoSmsbbFOYJp2g80ODhL85gu8g/640?wx_fmt=png&from=appmsg "null")  
  
大概一两天内，VDB会先进行一遍资产审核，看看该漏洞资产范围是不是属于那边的，要不然就会直接pass，意思是让你提交到别的CNA成员单位，或者提交到CVE平台。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxAaWz4EdyQgGUu669Nrib2mZXtliaWUibwKgaImWibdYszwyGywpibpYg70A/640?wx_fmt=png&from=appmsg "null")  
  
邮件内容：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxIxgG6zEyhTmTY607msg9kj9aZRCR4YbfQeia6xDtSyl5x3Et6GycBjQ/640?wx_fmt=png&from=appmsg "null")  
## 五、 CVE漏洞通过&申请公开cve编号  
### 1、CVE漏洞通过  
  
通过上面一系列的漏洞提交操作，剩下的就是等着CVE平台审核通过了，我这里以VDB平台审核通过为例，给师傅们演示下，正好今天通过了三个CVE通用型漏洞。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxOkgAUibEGZBtbnHs1rEFbvt57U4gtFBXPgPIpmbtp8CV29J10hYcJicA/640?wx_fmt=png&from=appmsg "null")  
  
VDB漏洞通过之后，会给你发一封邮件，意思就是说漏洞已经通过了，且帮你成功申请了CVE漏洞编号。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxyvFIXKFlhYHDYexcZZT6O8k9XqumQNofq9icYOzCxsPQ0oAv4HlwePQ/640?wx_fmt=png&from=appmsg "null")  
### 2、申请公开cve编号  
  
获取cve编号之后，别人是查询不到的，因为没有公开。接下来申请公开cve编号，网站：**https://cveform.mitre.org/**  
  
选择公开cve编号。输入自己的邮箱。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxMVgfYrpD8V5dq8I4tvE84xuk6jbFktu1P1N6SvS14GEFAJWjFJbgzA/640?wx_fmt=png&from=appmsg "null")  
  
详细的相关操作直接看我下面的 这张截屏即可。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxeXt6diaULricwHIhv0882hs9Cbsmkjo39BibnhFjs1y6FgYPPib6P1GEAw/640?wx_fmt=png&from=appmsg "null")  
  
提交完成后，显示的CVE界面。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxmSEnczibo7BtC7asAbSTNfqCh8FOm9l1ZHQgm3sOodmrOdicMIP5hgUg/640?wx_fmt=png&from=appmsg "null")  
  
然后会给我们发送一封邮件，意思就是说那边已经收到我们申请公开CVE编号了，那边等待审核。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxicM4F5YlvNM7MXL8WtibBccjJJLT3du4JVTckem2HI2fnbpUr6e9icbRA/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmx21Yo0xjDIicNUx9VtLEHHoP5Cib0iaarnoOZH8lcpfsFrRYypfvSc1B3A/640?wx_fmt=png&from=appmsg "null")  
  
后面CVE会发一封邮件过来，提示你申请的CVE编号已经成功公开，并且可以查询了，查询CVE编号网站：https://www.cve.org/CVERecord?id=  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxHHVXKv7BJMibLQxTgCrkcnb7UjCJpFcsicRw1Rh2K2iaYuPhbXwBvOaWg/640?wx_fmt=png&from=appmsg "null")  
  
通用型的系统SQL注入漏洞，CVSS给评分了高危。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXicPrvOEIMIlseE7xxPHXmxeaJICHwP4tgSkb7QhSbgJiaZUwMN1sUoV4Wn1rge7bO7icEqE2gwKoVg/640?wx_fmt=png&from=appmsg "null")  
  
  
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
  
  
  
