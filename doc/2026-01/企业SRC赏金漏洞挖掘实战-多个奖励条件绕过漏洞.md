#  企业SRC赏金漏洞挖掘实战-多个奖励条件绕过漏洞  
原创 猎洞时刻  猎洞时刻   2026-01-12 00:01  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9evFcNH31Pjh0f83GEqsibSQsGS8uUrBPLU6VJbjw8CTibOgsYYOhqqKpaQHb9BicrJcCOYhZG0tYOg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**免责声明**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/bL2iaicTYdZn6mG6TyJornrhz9JticBo3Nx4zhzUFXcggEDw1lkfzMI0KuLp7dW4dDCvbfgAKlLSX3yGmYg0gtXcw/640?wx_fmt=gif&wxfrom=5&wx_lazy=1 "")  
  
  
```
本公众号“猎洞时刻”旨在分享网络安全领域的相关知识，仅限于学习和研究之用。本公众号并不鼓励或支持任何非法活动。
本公众号中提供的所有内容都是基于作者的经验和知识，并仅代表作者个人的观点和意见。这些观点和意见仅供参考，不构成任何形式的承诺或保证。
本公众号不对任何人因使用或依赖本公众号提供的信息、工具或技术所造成的任何损失或伤害负责。
本公众号提供的技术和工具仅限于学习和研究之用，不得用于非法活动。任何非法活动均与本公众号的立场和政策相违背，并将依法承担法律责任。
本公众号不对使用本公众号提供的工具和技术所造成的任何直接或间接损失负责。使用者必须自行承担使用风险，同时对自己的行为负全部责任。
本公众号保留随时修改或补充免责声明的权利，而不需事先通知。
```  
  
一、挖掘站点一：  
  
1.1 领取非预期内的奖励漏洞  
  
  
某小说APP存在任务领取非预期GET，通过该参数利用，用户每天可多领取书币，用于付费书籍阅读——  
  
启动模拟环境：  
  
抓取该页面的数据包接口：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjEgfdm2qPexibEJQEtgmlyvUibXYRE9RtlplsSW7ULO2mZr1RKyjQVwhg/640?wx_fmt=png&from=appmsg "")  
  
  
每个task任务对应相应参数值：  
  
比如首次开通会员任务的  
ID  
值为1000028  
  
新人签到福利的  
ID  
值为1000031  
  
读小说新人奖励ID为 1000032  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjSx6MkUpsaTibmQXYEtaFYDRKRLGAEicUfNgCwFhy4ia2UE6YvjH0VlEEA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjsGuiaRpr8biaNGaydvww8yr1eeRvEpich4ycuNz5eCJMc9nlXT4yDRK5g/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjtTarRYdibBBPHs75xqDeicMnzkqbmKibRZk4S6NJUDSh39tqbHAdUaxkw/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
这里找到一个某接口数据包中不曾有的参数： {"task_id":1000029}，请求——  
  
这个id值在自己的任务栏目中并不存在，属于是额外的任务id。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLj95X6yRmAibAvJMpFMMiayCbSxk2icg4m7MMvPXCBhVWlk3x6t9Ve3j4yg/640?wx_fmt=png&from=appmsg "")  
  
  
然后使用该id，在新人签到福利里面进行领取的时候抓包，进行id替换为1000029这个额外id，于是成功领取了id为1000029的任务福利。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjjBaRn5Jiapvooz3KKcLOUFT8TGkkiaQGq1U3STuIw7RTuJZWa4ib3foibQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjzHzOKHyuOicqPkUFYfJpbSEDhzbBibaEyOZc7QbSuI0Slj9Wa1MIdSbw/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
随着对该产品进行了深层次研究，发现通过POST   
  
/xxxx/xxxx/redcandle/task/done接口回传的数组，可以构造任意满足阅读任务的请求及  
不在阅读任务清单中的任务请求  
，完成书币无条件利用——  
  
  
  
其次该漏洞，除了在历史数据包中去发现额外的任务id，然后拼接id去领取之外，还可以进行id的爆破便利领取。  
  
  
  
1.2 修改time时间强行完成任务，领取奖励漏洞  
  
观察——  
  
发现这些任务id，还有对应的领取时间，需要完成阅读时间，才能领取对应的奖励。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjIibFzoXuEBgnxKlYDssJPSV0QKzw3l74wlAvwjCXooypTjVqCjSRiclw/640?wx_fmt=png&from=appmsg "")  
  
  
  
这个其实就是【福利中心】任务二请求，但是通过构造：  
  
POST   
/xxxx/xxxx/  
redcandle/task/done?redcandle_read_time=  
  
即可绕过校验，也就是把这里传到前端的json拿来用即可：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjQ60vMtw48MfyMH8iaM7YreWnWwCPf6QwOsaqhcUQsJpr0xgdz3F6sgQ/640?wx_fmt=png&from=appmsg "")  
  
  
领取书币，这里有个计算公式：  
  
redcandle_read_time=60，  
  
就是1分钟，通过计算  
  
redcandle_read_time=60*180=10800，  
  
即redcandle_read_time>=10800即可GET到任务中所有书币{250个}，这是预期任务内的利用：  
  
  
于是计算到时间为10800，就足够领取全部奖励，然后在进行抓包，在下面api接口，直接修改原本的时间，修改为10800，然后发包。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjqAVB0LA4h8Ejek0PiaBjM6QBicqUxry6vnRrkL7iccvQk1yOzw1icuw54g/640?wx_fmt=png&from=appmsg "")  
  
  
成功领取全部奖励。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLj2fnuU0GcMNYqSB5lnRicBwTZb0KQxsyFU14BRfjn2mq6h7hFaKBw58A/640?wx_fmt=png&from=appmsg "")  
  
  
  
任务条被拉满，奖励随便领取：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjOrc13g9LHkXoWZF7r5VoClibx9icn8KMPP0anLuPPDI6iaA78XUSribNwg/640?wx_fmt=png&from=appmsg "")  
  
  
  
1.3 强行修改阅读数量，领取奖励  
  
  
非产品设计内的利用：参数遍历到这个：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjaaT96P5TnjHRCcFibFuuoX5uxxOm2ftFN7eyiaw3LxLNLkX1ZCZhrcpQ/640?wx_fmt=png&from=appmsg "")  
  
  
这个是书籍数量的任务埋点，但如果笔者阅读完以下，会提示已读完：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjPghBzYICiclwqYSXWmuZ4fFrYTUFcpRPtw3qbIFx0PCv6lfodHggSfw/640?wx_fmt=png&from=appmsg "")  
  
  
并没有弹窗发放书币，并且在任务说明里也是没有这一项的，  
说明是开发预留设计，至少是暂未上线的活动任务  
，这里利用构造API为：  
  
POST   
/xxxx/xxxx/  
/redcandle/task/done?redcandle_read_count=6   
  
对比，这里的6，就是完成阅读6本书籍。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjxf4ACOh5rUXCUWsD4qNicib296nUjsgPxNzCuc2iacjun9ibxibdicS1lEKQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
刷新——  
  
成功完成阅读全部6个书籍，回到奖励页面，  
可以直接领取阅读完成奖励。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjv1Smczd99ibrA1slzwiaKb0dicAOfYuNGh8t4CiaPabzx52jucvDXRm11Q/640?wx_fmt=png&from=appmsg "")  
  
  
突出危害点：  
  
书币使用条件和充值口的是等价的——  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjOl6bcEiaiarj3T50deicmMbibmfBduTYrluSyIehqL8tzn5iaQn1ZTGbJjg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjibGePO987fSZjNL0nnf2icFxf4ryxiaACPsHTDnAmH2IAXTEED7GyPjYQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
这个漏洞关键点在于可以利用遍历的参数回显的key进行构造，只要满足数值大于回显里的要求就能直接获取奖励。  
  
  
二、挖掘站点二：  
  
  
XX某应用存在三个接口越权-  
  
【第一个接口是创建预测，用别人的清单id发起预测，干扰别人的预测缓存，攻击者能通过响应成功与否来探测哪些清单id是有效的】  
  
【第二个接口获取别人预测结果】  
  
【第三个接口，直接返回基于别人清单推荐的达人列表，而且带着完整的报价细节】——  
  
  
https://xxxx.cn/ad/creator/user/author-lists/detail?id=7584237899622498342  
  
这是帐号一的id  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjPbt718dIuAnR4O2LNaCRrX4ImjibBvSX0VhwDOhdAyUSKedf7DPicZCQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
然后账号二，创建了另一个清单ID： 7584254656496255027  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjbJ3BFmYJeXf0oxfl5BPdCgPQBXY69mVuMA0DQbOQCH4eq4xj7VtwzQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
在以下API中替换为第一个达人清单ID进行越权，  
用别人的清单id发起预测  
：  
  
  
POST /xxx/api/gsearch/recommend_for_star_authors  
  
{"page":1,"limit":20,"platform_channel":1,"recommend_strategy":10,"extra_info":{"author_list_id":"7584237899622498342"}}  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjGP9qgtcBsaNVEO6vWS0AMRia46MFxicR4Uq7ibNZ4wpJ8wrMtuYApribPw/640?wx_fmt=png&from=appmsg "")  
  
  
自己的达人清单数据：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjrszwqUShtwRrhx9LdG6XSxu16AUKZYRVoKCneKaAdl3GaWiaQTicnXRw/640?wx_fmt=png&from=appmsg "")  
  
  
这里后面发现可以请求这个page：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjiaCKOkDq2H3SkF2eIEiayh8YZvEfEugOXU3mQdAvFbNbTl6JZuetX9WQ/640?wx_fmt=png&from=appmsg "")  
  
  
但是其他api接口向服务端发起请求，判断完才可主动发起请求，所以直接去访问page是得不到的。  
  
  
清单ID不存在时：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjibk4ewCrr4Mia2YcZbIeWFvUVUhswU3qRxsfD9Cp1DKsGSCptWbqM9zA/640?wx_fmt=png&from=appmsg "")  
  
  
  
证明存在IDOR，api2：  
  
GET/xxx/api/gauthor/demander_get_latest_author_list_predict?list_id=7584237899622498342 HTTP/2——  
  
  
获取别人预测结果  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjrt8niaaTZjeLdzOHeRNd81DZUbia9BjChsJed8P9RC3f5whvvHyElSVg/640?wx_fmt=png&from=appmsg "")  
  
  
攻击者利用他人的list_id反复发起预测请求-可能污染该清单相关的预测模型数据，影响清单所有者在后续获取预测结果的准确性，而list_id与uid相关联，高频调用可导致其他用户账号异常封禁（审核可自测）。清单id不存在时：证明存在IDOR。 api3：   
  
  
POST /xxx/api/gauthor/demander_create_author_list_predict  
  
{"list_id":"7584237899622498342","first_industry_id":0,"first_category":20,"top_rate":50}  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjUymgok4LPEG3M9ff4JdYZY53GC7UMe9kT5ZhUoFdR2XKsAeVTKniaUQ/640?wx_fmt=png&from=appmsg "")  
  
  
操作别人清单里的预测动作，证明存在IDOR：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLj2Jrc01XlrkFsp7Udxfbn1oc7IAx32HF90I5ZFoDGa3Mo4FFOn0sH8w/640?wx_fmt=png&from=appmsg "")  
  
  
  
如果需要挖洞实战src培训(含一对一技术和入职解答)和安全考证，欢迎看下面介绍嗷～  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibib9dGhbbLFuSBu3FTFItLjkFIFUztsDV4ScIgzQib6K04suXJ9hENmNPvpT5oMicoqYVChzT3045vg/640?wx_fmt=png&from=appmsg "")  
  
  
## 《往期内容》开局一个登陆口渗透二十多种方式-公开课  
## 竟然还有搞网络安全的学生不了解校招？真要以后后悔莫及？  
  
## Webpack打包js.map泄露导致的通杀0day  
  
## 某次攻防突破打点和内网刷分  
## SRC | 一次路径可控造成0click的账号接管  
  
  
## 挖洞实战打法-在线编程平台沙盒逃逸RCE  
  
  
## 记一次XXE漏洞实战和getshell-system权限  
## 小程序为什么好挖？某x多个高危实战案例  
## 某985证书站跨平台、跨IP的通杀漏洞  
## 众测实战 | 一次正负叠加导致的支付漏洞  
## 实战一个小细节导致Mysql、Redis沦陷  
## 记一次swagger的深度测试造成第三方API  
## 记一次从抖音日到edu站点的经历  
## 如何在日常渗透中实现通杀漏洞挖掘  
## 记一次实战日穿整个系统getshell-共九个漏洞  
## 记一次非常严重的全校越权信息泄露  
## XSS 如何乱杀企业SRC -公开课视频  
## SRC第二期公开课！！超全信息收集！不听白不听！  
## &lt;干货&gt;微信小程序特有通用漏洞&小程序强开F12开发工具&小程序反编译&Accesstoken泄露  
  
  
  
  
**关于网络安全低价考证**  
  
  
  
网安证书超低价！！  
网安证书超低价！！  
网安证书超低价！！  
，  
包括 CISP、NISP一级、NISP二级、CISP-PTE、CISP-PTS、CISSP、CISP-IRE 、CISP-IRS 、ISO27001、PMP 、CISAW 等各种网安证书。  
价格肯定要比大部分机构等都要低，咱们这边一直都是做的性价比  
  
有需要的师傅们可以随时来找我。加我二维码，  
再往下面就有二维码。  
  
  
目前我们这边已经输送众多pte、cisp、nisp等网安学员、抄底价格包含培训费用、多次补考费用、考证报名费用，巨额保证金，稳定考证没问题！！（  
如果你自己知道理想的合理价格，可以自己带着期待的价格找我，你报价，合适直接成交！）  
  
  
详细了解可以看点击下方链接：  
  
[关于网络安全超低价格考证CISP、PTE、NISP](https://mp.weixin.qq.com/s?__biz=MzkyNTUyNTE5OA==&mid=2247489280&idx=1&sn=fce79dc1802d25c10f6becd0151ea1d7&scene=21#wechat_redirect)  
  
  
  
  
**猎洞时刻第三期漏洞挖掘培训**  
  
  
  
   
     
猎洞时刻SRC挖洞培训第三期开设以来也是广受圈内朋友们的支持，也是和团队们共同进步了许多！团队排名、学员战绩、入职大厂也越来越多！  
  
    我们是一个综合性的培训，不仅仅是搞企业SRC、Edusrc、CNVD等，也更注重学员的综合挖洞能力，让你能够在安全这个行业上面扎根立足，对于学生还会额外提供校招讲解和资源、规划，更好的入职大厂。  
 目前第三期上车享受低价优惠，后续第四期涨价不会有任何影响！报名即永久！包永久一对一解答！  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**一、课程覆盖内容有哪些？**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
《猎洞时刻SRC挖洞培训第三期》课程原价1699元，现在报名  
还临时可享部分优惠  
，越早越便宜！并且是永久每期均可学习！（考证也可加我）  
  
  
注意！注意！注意！目前第三期接近完结，快开设第四期，到时候会进行涨价，如果想要目前第三期价格优惠，快快滴滴我哦~一次报名永久，包含永久一对一解答！！！  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9XP1icfbxx4tSm3LXJWMmF6wkBASUnGtVTLJFdwLRiafq5oc8QjqibWWogTsgtJQdlJlODzq0nbtUXQ/640?wx_fmt=png&from=appmsg "")  
  
  
【核心权益】  
  
一次付费，永久学习：  
  
报名后，可永久免费学习所有未来新课（如第四、五期），即使未来涨价也与你无关。  
  
免费赠送过往课程：立即获得第一、二期全部录播课。  
  
学习无忧：直播在晚间进行，方便上班族与学生党；每课均有录播，并提供一对一答疑。  
  
课程安排与服务：  
  
直播：每小几天一次，通常在晚八点半，时长约1.5-2.5小时。  
  
录播：每节课均提供录播，可随时回看。  
  
答疑：提供永久一对一学习答疑服务。  
  
【课程主要覆盖方向】：  
  
覆盖企业赏金SRC，众测赏金，EDUSRC，CNVD和工作项目渗透挖掘。内容方面主要是Web挖洞、小程序挖洞、APP挖洞、JS逆向、云安全、护网培训、项目漏洞实战风险等内容。  
  
以上内容为第三期主体方向，后面第四期、第五期会添加更多新内容，并且无二次收费。  
一次报名可以永久学习！  
  
第一期 16节课  
  
第二期 22节课  
  
第三期 36～37节课 （当前）  
  
第四期预计 45～55节课  
  
我已经准备好了很多好东西，  
会逐渐更新到新的一期，现在价格优惠最低一千多，这么多课程，划算下来，每节课可能也就小几十块，而每节课又是几年的浓缩积累。  
第四期预计还会进行涨价（毕竟每天全天一对一技术解答也很累。）  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibpibeBcibIlUib98f64ZLv5Egt7R7ZyhgJbEWFuFH5O2Sibh0PLTzHWnbtx6W64JVWicJNhVFl5ocvStg/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=10 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqraQvPbruXQlV55lOfd4QEYhBOdc37Fia6hLhuVoK7EZiagWIvnz7Yw7JxQ/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibbJTibIOrGnEiawBlz0AcDT3QOK6czfBmj5hoFoTlZToic07DvBqmib2HZ7DMo9ib0LdM7EHRSaH0u4WQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqraJjzlCb9pATMAG9xd3ao85iaVKriaicKiamIrD4vNibMNBOfIoGlwqIhOdJQ/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqra2BvjYc96Z6QfgmRXaNBYWuZyv4LhVwEAANnJ1p2OgXjbWhVnsCu0Fw/640?wx_fmt=jpeg&from=appmsg "")  
  
每节课课件都是认真对待，写了好几天进行总结。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibpibeBcibIlUib98f64ZLv5EgzCxAestTPnvbvc5llLtb4sbNGNz8pbQibicsLcicicfBjiaSLAicNQ4WMqbw/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=14 "")  
  
每一期课程内容会根据学员的需求而动态添加新内容和技巧！  
每节课都是几年下来的浓缩课，  
超多实战案例和技巧  
，每节课都能学  
超多量内容  
！  
单课动辄十几二十个技巧、实战案例！几十页课件  
  
（以下为随便几个课件页数和文字，量大这一块你不用担心，我不私藏，  
无保留教学  
）  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqraLteA4U115miaSvTneHhBhf5WxUU3qSwKs9VydtlnvRrwEXJwLtSGRng/640?wx_fmt=png&from=appmsg "")  
  
【猎洞SRC挖洞培训第三期课表内容】  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9dHhRYYdr5LlxEEUonFBH61RfibJ2qjgvTria6wzaWuXLGkaNnp2LwNHibyFxiafRCpgt1pLbasdM5eg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**二、业内少见被学员喊着涨价的课程**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
     如果说，别人骂割韭菜的培训，是有很多，  
但是有几个能做到让学员心甘情愿天天喊着让去涨价？一切皆是因为超量的对学员进行付出和解惑。  
如果有聊天造假，欢迎锤我，都是群里大全发的，我不玩什么造假套路技巧忽悠别人，一切可查。我只想多教一点，为学员真正提供帮助，才能更好有回馈（每天都有已报名学员推荐给身边朋友的）  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicVgicZmS5Yd5mD2hUJQ4YcQRuBKavQ2gFWMEVJbOUIpImxTfObIQqibp2TPbSq9nEXNiaYCbFR69UKw/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=6 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicVgicZmS5Yd5mD2hUJQ4YcQhQtWEMAzzVicLYRL0SIeia7iaIMgQEtq4cGXYGtz9l9PSN1YN8wOEssFw/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=7 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHic5niaibkFgk2ic5Yf4eG876ibQc967mehPT1jSSgnfQC7brLCws707Af3ektOQwmzUYO9BB4LlASRyDA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDtcibmWLNcJGuqoLES2R3Qcmm4kjXeFKePNpiaAkicYRwPnUoZ7srtv91A/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**三、包含永久一对一在线技术解答**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
    我这边不管是你做挖洞学习疑惑，还是找工作，还是说你迷茫了学不会了，简历不会写了，遇到麻烦事情了，  
都是可以找我一对一解答，全天16h在线。  
  
我这边从来不怕说什么学不会，你有什么网安技术问题、入职、项目问题尽管问我就好，就怕你报名了躺平不学习，  
除此之外没什么学不会的，不懂就多问我，根据方向好好去干就没什么问题，还有其实什么安全厂商并不多难，不必自己看低自己的！有能力的话  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqra9kOJkA0KaYNibyKT7omCwFHAqoQroice5W81gVMUCnZOkZibvR0mQkjng/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqrayVibr1qeSMrS2ia22IU96VWJqicv7KCNfCEMSDwt7ia6ViaymJib5lvl80pw/640?wx_fmt=png&from=appmsg "")  
  
      
  
除了上面的简历、入职解答，平常还有超级超级多的  
挖洞技术  
解答，每天都在解答，内容太多了，我这里就不放出来了。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**四、课程的特色什么？除了挖洞，有没有就业入职帮助？**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
    课程最主要的特色更注重的是永久一对一解答售后服务，尤其是对于学生党入职安排可以有巨大帮助。  
  
 关于学员的入职结果，请看下面的内容：  
  
　我们是一个综合性的挖洞培训，  
不仅仅是搞SRC、赏金，也更注重学员的综合挖洞能力，让你能够在安全这个行业上面扎根立足，  
对于学生等还会额外提供校招讲解和资源、规划，更好的入职大厂。  
  
　　在安全厂商学员就职层面，也就是常说的几个安全大厂，无论是  
长亭、奇安信，还是深信服、绿盟、安恒、启明、360，均有学员遍布  
，很多的都是从基础薄弱的学生、实习生、或者在职小厂员工成功就职安全厂商正式工。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicVgicZmS5Yd5mD2hUJQ4YcQOFvcTeZOJs35zkDNDjcOFC0WpBaz2ia3mF8CHsCdC4IsFjJJRh46L8g/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH84CZG0rxw3p3txiar8hJibcTIW1TWkYEQyTzhlTNG0ULg55ST3qoFJicKqdG2duTK3vnSyGJSCVB84w/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH8NuPia6aIOENP4XGImm3Ycrn5D3XHRicyCjJXdLklxpSDSAoYppBCkgIlVlIILh3nicRHFOqBDIKdDg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH8NuPia6aIOENP4XGImm3Ycrh9yBcBMDSRckD2796FbMX3iacAwAbzUgdTlT4MtDUFcQUykkWcNgk4g/640?wx_fmt=png&from=appmsg "")  
  
  
拿到的入职Offer非常多，  
我这里就不一一举例子了。  
  
多个安全公司学员很多时候能够互相碰面：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqraCrjkJic5V2eficy807RVxHG3WQic1FibuoUmic1lb92TKic6Wianed7jnJszA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**五、学员在各种SRC的一些成绩**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
企业赏金SRC  
  
  
在企业SRC  
，有不少学员位列2025年榜前十，下面举一些反馈例子，真实可查。  
  
(该部分内容实在太多，可以查看我往期发的技术文章下面有很多学员实战成果介绍。或者  
加我微信CengLnH 看我朋友圈反馈。下面仅列出近期部分学员反馈。)  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqraO5bf3yJibLStSPvnphsLbLzcW7NRcnsoljHjZ9IVib3TWDV549OiaT9Yw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib3GRAia2zFygWLpcrE9JC8Fg8iaiapVRr2GVXFWoiajAyp8ItA5sRvb8NicwDHmoCWqcHnQZpsy0QkPkQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib3GRAia2zFygWLpcrE9JC8Fw5XGrCvluFPyBcZ5MUveM9ZsnSVaBdFhbeL3kmibcrQK9kKwzibvCZIA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHica68o8ricbIGvbVRGsvCBojIWz5ZnHcntAXwm2yibBK51JXM5c0kzjTia0psDvtfy22vDQPq3QiaFWgQ/640?wx_fmt=png&from=appmsg "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHico4s7YfpmcHmWDlhInfXQ3onZicYXP4Uj0ouTBT5XjibfTpA5kiaZzewcDnlKhicLxy12Oa2lm7jhU0g/640?wx_fmt=other&from=appmsg&randomid=vkc2323j&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
第二个学员也同样的拿到了一个高危的六千元漏洞。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibuYDC1YReFUiaeeCxEEPVGLtLoVj3RuVDBBbgqHziabLlOEyPQEXkgRibQr4gJXo39gNh5gVc18Fprg/640?wx_fmt=other&from=appmsg&randomid=x58y2mgy&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDlfN3VwRiarcAwicLwKMjD158eXSM6yU0rbpSM8ABFvWpv8DLtz8Cg6xQ/640?wx_fmt=png&from=appmsg "")  
  
  
EDUSRC  
  
在Edusrc也是战绩卓著，团队内核心学员edusrc六个月破  
四千多分  
，证书30本+，  
个人2025年总榜第一  
， 欢迎搜索猎洞时刻团队可查。还有多个成员实现从0破百。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibpEgic63GHZhCdicOCYb3h4SKY9evTu4UnzflxjJ9ADqlZhlKqLVNgtAmmMfPNCblsoicibxS3xjhu3A/640?wx_fmt=png&from=appmsg "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicsfiaZKESbNhgIqu5tfwALY74flREur5Db0xDhQQNkhPwOQa5m0TMlSYYw6A9df8DaRucXxkalafw/640?wx_fmt=other&from=appmsg&randomid=i0fdn69u&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
25年新创EDUSRC团队，  
目前EDUSRC 2025年榜第四。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibpEgic63GHZhCdicOCYb3h4Sb0xQG5wppkBlfezYeWmfYbPsqb9ETUeR8EX6e8Yne8qsjibfI3WQGSA/640?wx_fmt=png&from=appmsg "")  
  
在  
全总榜单第七  
！！还会更加进步！冲击更高排名！！  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHibbJTibIOrGnEiawBlz0AcDT3CDvvX8u73dCHyLsfnCsoIcHoxq8688VP6vriaRAHC582WibZXLAXibDeA/640?wx_fmt=png&from=appmsg "")  
  
CNVD  
  
在CNVD方面，也是不少学员手动挖掘很多证书。  
  
还有些同学，在群里直接分享方法带人去刷CNVD。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicsfiaZKESbNhgIqu5tfwALYzWcagp19avqg68yMJXCg9StedSvztuxtGT6WGBHBiaibHIYEckicljtdQ/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**六、有没有内部众测赏金项目？**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
我这边也是有对接的公司，有内部的众测项目，因此拿到项目也会分配个学员们一起挖洞，挖到就是你得钱。其次，对于线下，线上渗透项目，包括护网，拿到名额也会优先分配给学员。  
  
  
并且内部项目还会拿出非常不错的实战案例进行分享讲解：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH8UxOWM97eAicfmjMhgKL4uNUEyD7ZsjTqict6k3HloxdkjFLE1Jsh1AglfJxZTrSrJtGq5GBguqI4Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDAR31rRdbfpEhN5P7MRoDISicnF3ZLp4rjxXhhwcMfRL7icVicUekMTe5g/640?wx_fmt=jpeg&from=appmsg "")  
  
  
下面是最近内部项目，学员挖到漏洞后的众测赏金。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDorJzbKMZ7vyeRwEKNEXdagFAcEyEtREtL3xFSHvT1PG3kKYfib2x9ow/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDDvYyIh9pGoMtiawNnyRNSicM4R7CffhCINVrSvSK9e6JIgpolggFYtNA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**七、报名赠送永久知识星球**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
报名课程，即可赠送永久知识星球，内容含有众多漏洞报告、破解漏扫和插件工具、SRC工具、POC和安全文档等。  
  
  
已经加入知识星球的同学，报名课程可以抵扣星球费用金额。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDmPAflHHSvJIAcXWk2qnBv5ozGYEfAbgxoUEw4kGQGJlu4Bvko4c20A/640?wx_fmt=png&from=appmsg "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9XP1icfbxx4tSm3LXJWMmF6FLfpsSWbNzwzQJza2ibjh5l0t3uicD8DeibFlUfgLvXmn2ZRiadKlnAc6g/640?wx_fmt=other&from=appmsg&randomid=e7wdqp7u&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9XP1icfbxx4tSm3LXJWMmF6k8MJLUSTKbCwbEwE2yejib6SYER4uY4BtrtZUnb6SeSvuRt3AjLwLvA/640?wx_fmt=other&from=appmsg&randomid=qa6i6vdr&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9XP1icfbxx4tSm3LXJWMmF6DJ5I3VEY7k9SF6SUquUR3YJclSqSdNUCpjSxCcYylIHeicacZexfG5A/640?wx_fmt=other&from=appmsg&randomid=0xmlp1a5&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9XP1icfbxx4tSm3LXJWMmF6JibDHMf1cBZRic6MoEicRWSc8EICPuAGKMFwq388JKMxyGarX66EdPd5Q/640?wx_fmt=other&from=appmsg&randomid=65pre9gh&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**八、有没有公开课？**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
有的兄弟，有的。BiliBili搜索猎洞时刻就能找到。有兴趣的可以看一下我下面讲的第二期的部分课程公开课   
企业SRC挖掘XSS、微信小程序漏洞全解  
和  
信息收集  
。  
  
  
第三期的内容质量只会更高于公开课的第二期。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTH9uGoiaibSicrHjpP7JPHMnqramJXxyX2HsCD0KhfAg1hV8CANLa3xLIJCN6MTK4sNPAyKR5JwXzaVkA/640?wx_fmt=png&from=appmsg "")  
  
  
XSS在企业SRC挖掘讲解：  
（强烈推荐）  
  
https://www.bilibili.com/video/BV1GD9kY7EMq/?spm_id_from=333.337.search-card.all.click  
  
  
【[猎洞时刻]登录口总结二十种漏洞，手撕登录框！】  
  
https://www.bilibili.com/video/BV1qeBMB2ERd?vd_source=bdced03c03ba60be0c5314d5c66b1d2d  
  
  
微信小程序漏洞渗透全解  
  
https://www.bilibili.com/video/BV1wQJUzrEa7/?spm_id_from=333.337.search-card.all.click&vd_source=a64e6e96032bc2466c05095c13084241  
  
  
接信息收集讲解：  
  
https://www.bilibili.com/video/BV1sE4GeYEMg?spm_id_from=333.788.recommend_more_video.2&vd_source=a64e6e96032bc2466c05095c13084241  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**九、内部学员还享受更低价考证，无忧虑！**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
市面价格参差不齐，很多都并不稳定。我这边可以保证是低价格并且还是大型稳定机构。学员可以直接享受最低价格。(非学员也可以咨询我，也有不少优惠)  
 我这边可以直接给你报内部低价！不可能贵。  
  
包括 CISP、NISP一级、NISP二级、CISP-PTE、CISP-PTS、CISSP、CISP-IRE 、CISP-IRS 、ISO27001、PMP 等证书。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkD2p12icGXflo3iazArKWia66MVibnaeBGqGsEus31u17nufbYLwVz4hm9XA/640?wx_fmt=png&from=appmsg "")  
  
**十、报名课程还有哪些其他福利？**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
1.   
在平时，无论是学习挖洞，还是已经在岗工作，遇到的任何安全相关技术问题、入职等，可以直接咨询我本人，只要我会的内容都会进行一对一讲解。  
  
  
2. 新人需要各种安全相关的视频资源等内容，可以直接找我要，比如网安其他方向，代码审计、内网渗透之类的资源。  
  
  
3.   
报名即刻赠送原本108￥的纷传圈子  
(同知识星球一样)，内容覆盖渗透测试常用工具、漏洞报告、网安书籍等内容。  
  
  
4. 会有不定时其他大佬进行课程分享，  
加量不加价  
。  
  
  
5. 目前是课程第三期，报名后可以永久每期学习，无二次收费，并且之前第一期，第二期赠送，后面开设第四期、第五期等均可学习。  
  
  
6. 专项培训，遇到  
护网  
等重大项目，会进行专项培训学员能力，尽量让大家都能拿到一个比较好的护网结果。  
  
  
7. FOFA key会员 共享，md5收费接口代查询。  
  
**最后说明&如何联系我**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHib5UZnJLVwTu2juhB5R9MkDhCwGEfqVweNiaOOlf4el0JSxHZPdIwXMT0CsLzEWZwIibqhgV7BAGk5w/640?wx_fmt=png&from=appmsg "")  
  
  
1.   
感谢各位师傅们的观看，无论是否报名、考证，只要是网安圈子的都可以加我扩列。  
  
加我吹水、聊天、加安全群、技术交流均可！  
  
2. 如果是课程报名、安全考证也欢迎大家咨询我，一定给大家尽力的优惠。  
  
3. 注册公司、非安全相关人员、黑灰产推广不要加我。  
  
4. 本课程仅用于教学网络安全白帽子，要遵守法律法规，如果有坏心思进行违   法犯罪，  
从事黑灰产等人员，本团队拒收！  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHic5niaibkFgk2ic5Yf4eG876ibQXibxaVZB0pVqjmQ2DEfI4poua2oHrDribPsnSsqu7KSiadS9syO7cCzzQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
