#  一款集成各大漏洞检测的burp插件  
Tsojan  道一安全   2026-01-14 01:21  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWjKYvoSviaiaDUIGf1pH9H1bpSJRC3lIk08f5m6yibtkLDhFQwmCXicNMLFniaRrN0Xqvth9XWMQkn5bGQ/640?wx_fmt=gif "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**免责声明**  
  
道一安全（本公众号）的技术文章仅供参考，此文所提供的信息只为网络安全人员对自己所负责的网站、服务器等（包括但不限于）进行检测或维护参考，未经授权请勿利用文章中的技术资料对任何计算机系统进行入侵操作。利用此文所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责。本文所提供的工具仅用于学习，禁止用于其他！！！  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**工具简介**  
  
一个集成的BurpSuite漏洞探测插件  
  
本着市面上各大漏洞探测插件的功能比较单一，因此与  
  
TsojanSecTeam  
成员决定在已有框架的基础上修改并增加常用的漏洞探测POC，它会以最少的数据包请求来准确检测各漏洞存在与否，你只需要这一个足矣。  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**工具使用**  
  
  
加载插件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LSfo0gKmbYeV2YleVR9vfslegSOznbvvKbUpGGcZOibTMWtuSAPBy33Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
功能介绍  
  
  
面板  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LOAxvYJdeHbxFgMnqXf7GcPOwq7HNMlzJnwUdm2qFS1icFsTGvNl2brw/640?wx_fmt=png&from=appmsg "")  
  
  
自定义黑名单，插件不扫描黑名单的url列表，进行Reg匹配优先级第一。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LcNfoRcM21mTBPm8fcrsXMuhMQ0tnTFvGPu7MxYSKOnEKUeOgXI2jaw/640?wx_fmt=png&from=appmsg "")  
#### （2）主动探测  
####   
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LnOdduebPMs63lDLhRHMx0gD9Jgm0WkiaHlqThXr240v16RvuCUwFusw/640?wx_fmt=png&from=appmsg "")  
  
比如探测非根目录/，目录下面需要加/  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2L91y1wHGkC4s2icGDOcqPzoRdwxINAPRlojChGAVpf1ffVBicgJqJ4GIQ/640?wx_fmt=png&from=appmsg "")  
#### 3、fastjson >=1.2.80探测  
#### （1）本地环境  
####   
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2Lh61572kpFQPOvtDUkzT0k9eHXvtUzoq9AibNrIlE7H0pA2O3P70upAA/640?wx_fmt=png&from=appmsg "")  
#### （2）预查询DNSlog接口  
####   
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LqlUibwt3avliczcmQV1DI2TjYPaaqq5gJ07Gj7NaL993wHeBsbJ04Ehg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2Lp6XBhEoedGNB8dY6IibxNShiaLdPOEgarwSfn4jtmbOO0DUia2KDqXVeA/640?wx_fmt=png&from=appmsg "")  
#### （3）扫描  
####   
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LIDphGSCtVdySLwzgTAxRsDlicA9fueawuPJ1ibyxL9HibwBdzPe4pzvaQ/640?wx_fmt=png&from=appmsg "")  
#### （4）判断准确版本  
####   
#### 1.2.80版本探测如果收到了两个dns请求，则证明使用了1.2.83版本，如果收到了一个 dns 请求，则证明使用了1.2.80版本。  
####    
#### 4、DNSLog查询漏报  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LCzfx3Ea42a0icljGdPnAocAVRYmqMfLhnJsMvrLlJpSyUgM0KiaNAibzA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2L9pkHDmnOrps14WTnhQmI6sw0uAh03cWv19tzJtbngOyicu82HcibpSgA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LSlBUiasjKS4eHvlJzl7evCm2yC06fX6F0chXeowdT6xQGXQRYSq3Jog/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2LkickY2o77VIUOmXeYYwFyefIcIicS3D3Q9DzTJn6F7tFQ0x7Ohp0QQwg/640?wx_fmt=png&from=appmsg "")  
  
注意⚠️  
：扫描结束后才会在BurpSuite的Target、Dashboard模块显示高危漏洞，进程扫描中无法进行同步，但可以在插件中查看（涉及到DoPassive方法问题）。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubFUM2iadaDpCrjicQDpibYibY2L2LYuDqIOrowXPZFnLibWOsTBkcYlGJ4IDoI6iaR0UtqqYhqd1vInp1xQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**下载链接**  
  
  
https://github.com/Tsojan/TsojanScan  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**会员站介绍**  
  
  
之前推出过的资源超全、功能实打实的聚合信息平台，这次进行了四大板块的功能升级。除了**原有的文档视频资源更加丰富**  
之外，还新增了三大板块内容：**src监控模块、web指纹识别模块、资源共享模块**  
。  
  
  
  
**新增内容**  
  
**01 SRC资产监控初版上线**  
  
  
该模块能实现手动资产收集到自动化监控的跨越，已正式上线并开始运行。  
  
当前已上线的初版内容，监控范围覆盖**184家企业**  
的相关资产，包括带有证书的edusrc企业和常规src企业，**初版资产规模约300W+条**  
。  
  
系统采用渐进式扫描策略：首先使用2W 条高频子域名字典爆破进行首轮资产探测，预计**在15-20天内完成**  
常用资产的快速收集；随后将启动**百万级字典**  
进行全量深度探测。  
  
提前预告：后续会增加crt.sh等数据源获取数据。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qicTFxa8ta8Gv94ibJmd9wXSkxF6c3twf43ychIp3e27SDbftDRGU8tHA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=1 "")  
  
  
**02 上线「Web指纹识别」模块**  
  
  
在原有信息收集功能之外，新增了专业的 「Web指纹识别」模块。该模块集成超过 26000+ 条指纹规则，支持批量识别，并会不定期更新。当然，你也可以提交新的指纹规则，一起  
丰富系统指纹库，提升识别准确率与覆盖面。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qgjsrwgiadGg269LxqynLjA7hauszfw7eDOYFALOuHictBdoC23ib12vaw/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=2 "")  
  
  
**03 新增「资源共享」功能页**  
  
  
此前，诸如FOFA查询权限等资源以分散形式提供。本次更新将它们系统整合，设立独立的 「资源共享」功能区 ，目前包含：  
- **FOFA高级会员**  
：高级会员随便用，没有查询次数限制。  
  
- **多模型API Key**  
：支持GPT5.2、Gemini-3-Fro、GLM-4、DeepSeek-R1等多个主流模型的调用权限，基本可无限使用（供个人学习研究使用，禁止个人商用）  
  
- **某SDN平台会员**  
：文章免费查看  
  
- **Macked**  
：Mac软件共享区  
  
PS：只可自用，禁止外传，外传者一经发现不退费永久禁用。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q6yJTqTxrBG859r8PFGqeYH2n8zCz7G1kR4KRgLuD9eGU9tkXFrmspQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=3 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q5W1sGlWiardlffzLhYvrIBqwWWGj4cPOBicMCIMZxXZW8HYs1FBn5ibhQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=4 "")  
  
  
  
**04 学习资源大幅扩充**  
  
  
平台原有的视频与文档资源库已积累相当规模。  
  
本次更新在此基础上，重点补充了2000+ 条**安全类专项学习内容**  
，现总量达**15406条。**  
  
所有资源均可直接搜索，快速查找内容。后续会整理资源列表，方便大家集中浏览学习。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q9liaiazYnlhEe82FPcFPf6fvJuqrpQKvrViasneE9SgkcDibXqibiclsXbHQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=5 "")  
  
原有的文档类学习资料和全网高质量学习视频，规模达13000+ 条，领域覆盖：  
- **网络安全：**  
web安全、渗透测试、内网、红蓝对抗、安全运维、代码审计、逆向、信息安全、操作系统……  
  
- **编程开发：**  
web前端、JAVA、PHP、Python、微信开发、大数据/云计算C/C++/C#、Android、iOs、Asp/.Net、数据库、游戏开发、机器学习/人工智能……  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qh6LZicIZAMQx7Zd1KicIGf2rbZBgwEUibBJyNdOjVibrc9mD7mlBeoXKicg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=6 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qvXKFgS2YD17kyLekq4EYPx68RDOLhxQYfkl6ReibzPa0vibedicaF9xnQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=7 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qeIFCnPWqYHJHo44kcLV43KCic3p8l0bgDBCLkMicjX15LJcnX2eHpBhw/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=8 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qXLH3P0bkErHmrjqfk63VWhtu7icPIKuIvpPdAvnTtm1wGrqqh7Qg5Zg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=9 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qM4wBaoB6Xd3WajanHllpzkzicE1HL19eFjpp23mRQvYbuYBV8L5BsvA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=10 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qz8o1lcpNuqgS2lelpSST5Hpag26nOpxvGbXthJC8m2I3pxDqiaPib5Og/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=11 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q7ffIBZKkzeVenFrxPtxv894iaMpVFxWh6cHPYCAOoszkHwQ4P3uMgRQ/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=12 "")  
  
  
  
✦  
  
  
•  
  
  
✦  
  
  
  
**原有内容仍然可享有**  
  
1  
  
**四大资源站代下载**  
  
以下这4个资源站内的所有教程都可以人工代查代下载，VIP 站的搜索功能也能搜得到：   
  
瑞客论坛：  
www.ruike1.com  
  
IT教程吧：  
www.itjc8.com  
  
666PHP：  
www.666php.com  
  
666Root：  
666root.com  
  
  
2  
  
**ICP备案查询系统**  
  
- 输入公司名，网站自动查询其备案下的所有域名；  
  
- 可一键下载全部域名备案信息等企业资产，适用于资产梳理、信息收集环节；  
  
- 查询记录可留存，方便日后复用。  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q4d2v8cicibW0b00jpZsvEgkib8sOrB660nJf8zDMHucQSiclEOVHL4fiavw/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=13 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qicp7R5uBODvYsqnlnmdDibNiaXKPLDnGJ5iczoengPYx7Yia3qshVYcMNGQ/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=14 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qceUiafRq4jMoM3icAIclJfz2vL5ZoEZViah1f1wgwNOx5E7ZzXUCIFfNw/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=15 "")  
  
  
  
3  
  
**4000+ POC 漏洞库**  
  
- 覆盖 CMS、OA、组件中间件、设备、摄像头、国产系统等  
  
- EXP状态标注明确  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qkiaGCiaf269R9hic4n8DfgYmFYVV02jqhvssQkfj37gzGib2s5hC6XKv5A/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=16 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qQO2nsVAerO235IJNAC5aEZguBGHib2zUicRRoic1Due9qo28bWTHu73Vg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=17 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qb4OLEoejdo5VBmB1Twia6rpo9XiaNC8kj1kCLEP4fJVPDgiaWgcYwLNgg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=18 "")  
  
  
  
4  
  
**内部漏洞工具**  
  
- 工具还在优化中，待上线，提前透露有哪些：一键打点脚本、自动化测试框架、CMS利用工具、电子书搜索系统；  
  
- 所有工具仅限 VIP 用户使用，目前不公开售卖；  
  
- 工具可永久使用，支持持续更新和售后服务。  
  
  
  
  
5  
  
**专属福利**  
  
聚合平台不仅仅只是资源获取平台，更是一个持续进化的VIP社群。加入后你将获得：  
- **不定期赠送安全书籍**  
（已送5轮）；  
  
- **一次简历修改 & 职业咨询机会；**  
  
- **专属技术交流群：**  
每天都有高手在分享经验；  
  
- **搞钱小能手：**  
安全岗位内推、兼职项目机会。  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qat9tgo5iciaNzK3pRaC9U9pDPoDuBAl3BmAX9NiaKZ4ciaTQFtIyjhWfNg/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=19 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8q4j5fadNOGW5E0BhFKnGY0g3f7RRkYmibibFXQIYgz2JQo32mbQNNAvRA/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=20 "")  
  
✦  
  
  
•  
  
  
✦  
  
  
  
**加入方式**  
  
**年费79元，永久199元**  
  
****  
**购买后可看到帮会置顶中有帮主联系方式**  
  
**联系帮主帮你开通这个平台VIP会员吧~**  
  
****  
  
  
由于安卓支持小程序购买，苹果暂不支持  
  
因此下面将区分两者的购买渠道  
  
  
******👇**  
**安卓用户**  
**可小程序加入****👇**  
  
  
**安卓用户点击这里进入帮会小程序**  
  
  
  
**👇**  
**苹果/安卓用户**  
**可扫码加入****👇**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubHH7PCTpbFwXBQnR9O8Np8qA5dS1CPV8TlibLIsdrRAzwIEf1pbjPI1m4cb4QibXwulKeM6aEX3hPZA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&watermark=1&tp=webp#imgIndex=21 "")  
  
  
  
购买即可帮你开通资源站VIP会员  
  
扫码添加好友，加入后联系我开通 VIP 站权限  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/WAyrRuvrubGNIiaG0Mg4FZ85BtUFLg9pXeTIwxZmgUMKEvTFickkUThj7JYYZSEnc910eHc2j6ppDqceqNK9qEEg/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=22 "")  
  
  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjK9Euejia8GQQAicPWkJI7HfpDplIlc3tPr73ZYKHIdg9kIHpWaJia2tGA/640?wx_fmt=gif "")  
  
**点分享**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjXjW9bUCoUia7g4iaVGGGm5AKWRMoDMQoFDdJuiceofhPJ8SJpKSGToZcw/640?wx_fmt=gif "")  
  
**点收藏**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjAEe2Bq3UgWlgxribzfYtnQ6EVkxkao5qmK0xpaoycfHyGVl7zFicPGibw/640?wx_fmt=gif "")  
  
**点在看**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjDia9eCL6sIvuL17F5uKHsjx0GNc6estct1jOfWh4EtOcVsvzynOar1Q/640?wx_fmt=gif "")  
  
**点点赞**  
  
