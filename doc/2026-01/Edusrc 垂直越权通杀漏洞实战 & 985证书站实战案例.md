#  Edusrc 垂直越权通杀漏洞实战 & 985证书站实战案例  
原创 猎洞时刻
                    猎洞时刻  猎洞时刻   2026-01-19 00:01  
  
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
  
通杀站点 一：  
  
  
漏洞介绍：这是一个通杀漏洞  
，第一、第二、第三  
都是同款漏洞，该系统对外可以可以注册，但是注册的是普通权限，然后通过js提取到admin路径，拼接访问，使用普通权限可以  
垂直越权  
使用admin功能，从而操控整个前台新闻展示页面。  
  
  
1.1 未授权+垂直越权-证书站  
  
该站点是一个985证书站。  
  
  
http://www.xxxx.edu.cn/Center/?topOrgld=1#/home/HomePage  
  
这里通过注册：使用->注册功能进入系统，验证亦可使用已经注册好的账号121xxxx/123456Ff  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWdPuLibVfPzTekG7CCZTNLiagQOLzLdRwia6nLKicVIOHibVh5ib3DzzevQdg/640?wx_fmt=png&from=appmsg "")  
  
  
进入后台，因为注册的是普通用户，因此无任何有效信息；  
  
于是进行查看f12，找一找js，有没有其他信息。  
  
  
通过  
  
JS-在:http://www.xxxx.edu.cn/Center/js/app.6aa1f718.js下包含  
  
有大量路径:  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW4UXNVLicOjeJILrymYGCV7QTBJu6NTrElYpibKHI3ekBe91V8ZkwVb7w/640?wx_fmt=png&from=appmsg "")  
  
  
Config：  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWlSb8sKyAQg4HpOLL7DeBQqibpykylCyv7ialtpgBjv0icolLrc70BSHIA/640?wx_fmt=png&from=appmsg "")  
  
  
  
所有配置信息一览无余，基本没做任何过滤处理-尝试拼接，使用普通用户身份成功未授权访问了admin的页面：  
  
  
但是这种页面未授权，不代表存在危害，  
还要看这里的功能、api能否操控。  
  
http://www.xxxx.edu.cn/AdminIndex.jsp  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWrs4sYGMSMeEfmj7ypI1dW8FTcOxx0YyDSag2wWyQwQ53oD07R3mP1g/640?wx_fmt=png&from=appmsg "")  
  
  
  
继续拼接完整的地址：  
  
http://www.xxxx.edu.cn/Center/#/admin/HomeConfigPage  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWHcMF0ggKibbhkodCbvicNEqrxetYxWwcIiaEa2fEkiciccHUiavpmM56j5Hw/640?wx_fmt=png&from=appmsg "")  
  
  
点击中心动态：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW6RcRgKTdxQXOWryk7bh9ibcK6Go9y9xZOrAtKeTe4UCXcQKiafPox9iaw/640?wx_fmt=png&from=appmsg "")  
  
  
越权成功，给前端新闻页面添加一个新的栏目：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW456pYNGogicXLFPicq4PhCLUanaRQWUbKtNA3rCxv6mx3h3sicgdDtzDQ/640?wx_fmt=png&from=appmsg "")  
  
  
其他浏览器访问，成功增加一个新的栏目“中心动态动态”，证明后台可以控制这个前台页面。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWMt3INNPmjrSg5wicl25SIt9WTdrNtOXRSI5qJIJaEJgSLoOuxXZNp6g/640?wx_fmt=png&from=appmsg "")  
  
  
下面这个接口可以编辑网站文章：  
  
http://www.xxxx.edu.cn/Center/#/admin/ArticlePage/:id  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWTYxbBias1HzR2I6jyjnMw1W7iaiaibG4O2RO6YCZjk9ES3TKXITdqYg4Wg/640?wx_fmt=png&from=appmsg "")  
  
  
  
admin【添加文章】，被恶意操作会造成舆论影响。  
  
访问:http://www.xxxx.edu.cn/Center/#/applicationCenter/Manager，创建课程：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWuibHMemNvqcu6T8pEpZQkqANux0eb94Sp34icb8ccalW3cR5ImOG8libw/640?wx_fmt=png&from=appmsg "")  
  
  
下面这个是评价中心配置接口：  
  
http://www.xxx.edu.cn/Center/#/config/QuestionnaireConfigPage  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWKYdR3jT9QB2UtXJRaianNUBQqOYkaTC4sEGJFYQm6iaIpMdgkRUXZPPw/640?wx_fmt=png&from=appmsg "")  
  
  
演示了修改轮播图：  
  
/dataCenter/#/admin/ConfigPage/:id  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWld6QGuPwyGzhlayFFibPtWTPzjLRlJrLXqVrQuDqQZh7VlZ8X719aSA/640?wx_fmt=png&from=appmsg "")  
  
  
访问站点，页面主页成功被修改为绿色面具图片：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW1A5lKyjaicq5zIXibicTYiaaOagrLSjqcvC3eUibiaBURE5g9vPIr99OZ2Bg/640?wx_fmt=png&from=appmsg "")  
  
  
该漏洞因为可以直接修改官网前台页面，可以直接造成恶劣影响，因此证明漏洞存在后，给予恢复一切正常，随后提交漏洞。  
  
  
通杀站点 二 ：  
  
2.1 未授权敏感信息泄露  
  
https://xxx.edu.cn/Center/?topOrgld=1#/cmsHomePage  
  
  
和上面那个一样，也是一个经典的前台主页。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWicnONORaYeCo5Y6qIZKvMmIeyI8xVB67P3YqBoUURrobA2NBoCvL8Iw/640?wx_fmt=png&from=appmsg "")  
  
  
  
然后使用js提取插件跑出这些接口：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWnhd9UtY1xpUHyQ0Ts8dP3sgK52L8cno1xBk3Eum9vfqN4I0auQFNyQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
对路径进行拼接，可以看到系统管理员看到的，得到配置信息以及操作和上面一样，但是后面发现下面接口是有敏感信息的——  
  
访问  
  
https:/xxxx.edu.cn/Center/?Orgld=1#/approval/AccountApprovalPage 通过抓取返回包可以看到成员的身份证信息：  
  
包含个人姓名、学校、身份证号、手机号等敏感信息。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWj7HDRz6RXNoicPtsZWpoiakxkTLbsaYpFDAQQicEmKB7xWI8ezoYuqzibw/640?wx_fmt=png&from=appmsg "")  
  
  
  
通杀站点 三：  
  
3.1 未授权+垂直越权  
  
http://xxxx.com/Center/?topOrgId=1#/home/HomePage  
  
一样的方法，访问某个学校的前台新闻主页。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWGUrjQKJyFlKIW8NxnIHabGtZN3Lck25GbSF1If0kwfgP1FxyGFAbbQ/640?wx_fmt=png&from=appmsg "")  
  
  
https://xxxx.com/Center/js/app.0109cfd8.js  
  
  
可控管理员操作路径包含但不限于以下：  
  
https://xxxx.com/Center/#/admin/HomeConfigPage  
  
依旧尝试修改栏目，比如添加“中心概貌111”、“实验教学111”  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWnWP6RjxZ6Dia1Y6K7DjwrNUZlGeZJHURUqB2xic4HHcUYYUy7cvM0oXw/640?wx_fmt=png&from=appmsg "")  
  
  
刷新前台页面：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWQLYvib8F6cvdsp256YWczqIics8TeHxI4avpQYfqc0ydY3hUknDSF7Xg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
继续：https://xxxx.com/Center/?topOrgId=1#/home/HomePage  
  
进行垂直越权和未授权添加广告模块  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWNibMFZPdjTYcFvbgmTLd4k1QlW8QbarbUCyHBTOC2HLOKIjOs30Z70g/640?wx_fmt=png&from=appmsg "")  
  
  
可对网站文章进行编辑，删除等操作，  
  
访问：  
  
https://xxxx.com/Center/#/admin/ArticlePage/:id；  
  
访问：  
  
https://xxxx.com/Center/#/config/EvaluationQuestionnaireConfigPage  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWvvlgpFBff6DwicnrytRBIVZ4voTa1deNgTiby8fPTxJ0HdnwGOILPpKg/640?wx_fmt=png&from=appmsg "")  
  
  
演示轮播图修改——Center/#/admin/SwiperConfigPage/:id：  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWiayKvd5J0DVibhxKrZpjT2p9lyw2rgkKM4LrmFX4Be8a5iaGfyUzIeH0w/640?wx_fmt=png&from=appmsg "")  
  
  
于是页面主页成功变成了随便找的图片  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWwmwb3v6nLx9FhzyDbuicJI8b837ZmGw7iblxL9d88ma5K1GVWuuQfVZA/640?wx_fmt=png&from=appmsg "")  
  
  
挖掘四 小程序：  
  
4.1 逻辑漏洞权限提升登陆  
  
某教育厅管辖的小程序：  
  
进入小程序：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWsmTPp8OIhRshaCNtd2NWO8x8BbMLgM0CgRbpGgukks7hjzGqeVj9BQ/640?wx_fmt=png&from=appmsg "")  
  
  
这里需要微信code授权，填好基本信息：  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW0YwtaZibOlHelekQDoXURU0x5NpWuonTMDNia8PMBugzHfFZoxFUic7lw/640?wx_fmt=png&from=appmsg "")  
  
  
可以看到“我的”主页没有配置任何明显权限操作。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWzYEJsKFlenr3Ob72tQwkGsUYvkYwpJt3XIRcLnBnLQFWSrGDXI82Xg/640?wx_fmt=png&from=appmsg "")  
  
  
启动BP拦包，点击刷新登录，修改返回包如下：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWOB4uZ4yWpaNytzD79Xibta7pdeTDMlmjia71NXO0e7Z3yDwic8R5MZiarQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
HTTP/1.1 200 OK  
  
Server:JSP3/2.0.14  
  
Date: Wed, 01 May 2024 16:53:56 GMT  
  
Content-Type: text/html; charset=utf-8  
  
Content-Length:280  
  
Connection: close  
  
Cache-Control: private  
  
X-AspNet-Version: 0  
  
X-Powered-By: WAF/2.0  
  
Ohc-Cache-HIT: km4cm76 [1], csix76 [1]  
  
Ohc-File-Size:280  
  
X-Cache-Status: MSS  
  
  
{“admin”:”0”}  
  
  
  
经测试发现:admin = 0/1/2/3/4 分别对应不同等级:  
  
有查看一般教师但无明显管理权限/校级/县级/市级权限。  
  
admin=4级别最高。  
  
  
然后我们把返回包的0改成4，重新进入后发现页面完全不同的，可以操控各种东西。  
  
比如以下页面。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWMyRicSIuZVGFq4gYCSloibfc6ZKSxxOYqvQFkt9gDwTiccibYad3ericmPg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
可以随意查看教师管理  
  
  
再点击管理员：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWKyT2RIBng6MCHYeL2M7hvg2ibWvwpia2pd5saycqVN2aXoy0y5dEv55g/640?wx_fmt=png&from=appmsg "")  
  
  
  
抓这个包。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW71S7wmF4hzlJbeeaA56yHTqPpGTOkmkvRyajn9gpyH9gAhVyrjBeFw/640?wx_fmt=png&from=appmsg "")  
  
在重放器内还可以看到所有人的注册信息，包含各种权限的帐号。一次性泄漏所有人信息。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWWljzCMY0Z8O771xgjObRVMqJqa4RRw4US6DRKP7XMHjnibv1I1YZLHg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
挖掘五 公众号：  
  
  
5.1 任意密码修改  
  
点公众号这个业务办理：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxWrD7OKxTNwfp4zAYOZMuk1RV4Qyj77ybxiaolxCxwzZ5g2bz1xyLqPbQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
在进行密码修改的时候。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW6Kw7ibvEGrhNMTBKs7PDAzQchpqLFrGawNGeIvCILWS4rjRa6mSyTxw/640?wx_fmt=png&from=appmsg "")  
  
  
  
抓包发现这个接口：  
  
POST /htwx/updatePwd HTTP/2  
  
Host:   
  
Cookie: JSESSIONID=36PcPHdjD4Ksk9d7YDJwgqelT;ISESSIONID=36PcPH47dyv3rCrwH6j2Dmw9l3YA29YDJwgqellT  
  
Content-Length: 45  
  
Accept: /  
  
X-Requested-With: XMLHttpRequestUser-Agent: Mozilla/5.1(Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like GeckoChrome/107.0.0.0 Safari/528.20 NetType/WlFl MicroMessenger/7.0.20.1781(0x6700143B)WindowsWechat(0x6309092b)XWEB/8555 FlueContent-Type: application/json;charset=UTF-8  
  
Sec-Fetch-Site: same-origin  
  
Sec-Fetch-Mode: cors  
  
Sec-Fetch-Dest: empty  
  
Accept-Encoding: gzip, deflate  
  
Accept-Language:zh-CN,zh;q=0.9  
  
  
{'newpsd":"123456Aa","yhbh":"0xxxx0022",”captcha”:”yojv”}  
  
  
很明显是关于用户密码修改的api，并且它都没有鉴别原来的  
oldpassword旧密码  
参数，也是直接出洞  
  
  
  
  
  
如果需要挖洞实战src培训(含一对一技术和入职解答)和安全考证，欢迎看下面介绍嗷～  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/d6JIQYCSTHicIRaIcZLWwicfRNtX2EtKxW1XoXamveu5Mexh52X2rPm7tEjhdoXOicTkbE7SbTUlPclGGulD4mnkA/640?wx_fmt=png&from=appmsg "")  
  
  
## 《往期内容》开局一个登陆口渗透二十多种方式-公开课  
## 竟然还有搞网络安全的学生不了解校招？真要以后后悔莫及？  
  
## Webpack打包js.map泄露导致的通杀0day  
  
  
## API Fuzz结合AI实战获取数据库密码漏洞-实战证书站案例  
  
  
## 【edu实战】记⼀次⼩程序泄露16 W+敏感信息，接管18000+账号的 漏洞挖掘  
  
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
  
  
  
