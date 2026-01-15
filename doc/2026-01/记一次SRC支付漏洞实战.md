#  记一次SRC支付漏洞实战  
原创 锐鉴安全  锐鉴安全   2026-01-14 23:01  
  
，  
  
zai  
  
  
cizhi'cizhici  
  
  
s  
  
“证书站的未授权漏洞，忆校园青春  
[阅edu证书站的未授权漏洞，忆校园青春](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486254&idx=1&sn=993cd82bceba042301a009c28ca2d251&scene=21#wechat_redirect)  
  
  
  
点击蓝字 关注我  共筑信息安全  
  
  
  
  
  
  
￼  
  
免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任！  
  
  
  
  
1  
  
  
  
  
背景  
  
  
本次实战的案例，源于某高校的人脸采集系统，听着都感觉危害很大，因为全是敏感信息，如身份证、人脸等，拿到就是高危漏洞。从无账号到登录系统，靠的就是fuzz，详细的过程见实战过程。  
  
  
  
  
  
  
2  
  
  
  
  
实战过程  
  
  
通过一系列的信息收集，高校的人脸采集系统引起了作者注意，为什么？因为有敏感信息。  
  
  
  
  
连Hunter、Fofa都没索引到这个系统，作者靠灯塔拿到了，灯塔确实好使，关键还免费，需要的师傅可以文末获取下载链接！  
  
  
  
  
系统的首页如下图，可以看到，只用一个登录按钮。  
  
  
￼  
  
  
  
  
看下findsomething，也没有找到“注册”功能相关的关键字，同时也跑了下接口，并无接口未授权问题。  
  
  
￼  
  
  
  
  
  
本次案例的关键操作来了，首先抓包观察下登录系统的数据包情况，随意输入账号密码，点击登录。  
  
  
￼  
  
  
  
可以看到登录的数据包中有个login关键字，秉着试试的心态！  
  
  
作者将login改为register，惊喜时刻，注册账号成功。  
  
  
￼  
  
  
  
使用注册成功的账号登录系统。  
  
  
￼  
  
  
  
可以看到获取到了身份凭证。  
  
  
￼  
  
  
  
登录到了个人信息首页。  
  
  
￼  
  
  
  
任意用户注册账号漏洞拿下，这个fuzz操作确实有点妙。都登录系统，肯定得把全量的功能测一遍，一般可以测试sql注入、越权、文件上传等漏洞。  
  
  
  
  
co  
  
  
点击蓝字 关注我  共筑信息安全  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP6LllD9Qm4I2eKvyHt1WlNDd8O4wJKfGhV48dQHTMk8icXxCBI5BKxPqWQOfwFWxPtG2e8iazqUssJg/640?wx_fmt=png&from=appmsg "")  
  
**免责声明：**  
请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息、工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任！  
  
  
**1**  
  
  
  
**背景**  
  
在企业SRC所收录的各类安全漏洞中，支付业务相关的漏洞因其直接的经济利益关联，必然处于高风险漏洞的核心关注点。  
  
  
支付环节逻辑复杂，涉及前端展示、后端处理、第三方网关对接等多个模块，任何细微的逻辑不一致都可能被攻击者利用，造成资产损失。  
  
  
所以掌握此类漏洞挖掘方法是比较重要的，本次特分享一个某企业SRC的支付逻辑漏洞。希望对你有所帮助。  
接下来看实战。  
  
  
  
**2**  
  
  
  
实战过程  
  
本次的案例源于某基金购买小程序。进入个人中心，先随便充个2分钱。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUD3AficqhIyuaMKEMfkS2xJcNK7ZrJ06qCm03EM5A78nGUHWQ0YoXDpw/640?wx_fmt=png&from=appmsg "")  
  
  
可以看到充值后显示的金额知道了“分”这个单位，那假设超过了“分”这个单位，更小的单位如“厘”，后端会怎处理呢？会不会可能有问题，基于此思路，开展验证。  
  
  
将充值金额改为0.001，如下图。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUQ9wDc74OLwiciaFSA6GiabIwibAWfNJmq8icBUribxljKojqhwccFu7mYdicA/640?wx_fmt=png&from=appmsg "")  
  
  
某宝直接报错，因为第三方支付是只能充值到“分”的这个单位。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUj0WEmicibs11tiawUx4u3xMbqWCuQzSEJlibHqQ9JiaShwwfcpQVdTwibB4w/640?wx_fmt=png&from=appmsg "")  
  
  
尝试一下充值  
0.019，看后端会如何处理。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUdDiaXfAhykufIR6fvsicpMT4v8LVcMDMl7Efh6edHiaxwlrrbCPhQ5XCA/640?wx_fmt=png&from=appmsg "")  
  
  
此时，后端会将0.009忽略掉。前端显示充值金额为0.01元。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUya9yk3jprXTkt3fIAetqzicq5pmZJX9EtIkNbNB1lqP2lZmqndAHUgQ/640?wx_fmt=png&from=appmsg "")  
  
  
继续下一步，完成支付，后台显示充值金额显示为0.02元。惊喜时刻，出洞。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/RLTNmn7FBP56ccxvWWNkKETqMiaN0GubUPA1ibX48NHcyXia4UcrLiby0xLfeBrcG9OK4sic2RoeDJIXicaKAS75qJAg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
3  
  
  
  
经验总结  
  
本次分享结束，过程比较简单，但思路值得借鉴！  
  
  
有经验的师傅已经对此篇文章进行了总结，这个就是比较经典的“四舍五入”支付漏洞，后端的逻辑校验不够严谨导致了此问题，虽然前端显示是0.01，但  
系统本身后端已将金额进行了四舍五入至0.02。  
  
  
  
  
  
  
  
  
往期好文推荐  
  
[记一次"高危"逻辑漏洞挖掘实战](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487704&idx=1&sn=f2f296b6870ed8ddbaa6d142c93ea114&scene=21#wechat_redirect)  
  
  
[记一次SRC渗透测试实战](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487518&idx=1&sn=9d4498c7829a31051be9ba9813d367ec&scene=21#wechat_redirect)  
  
  
[值得细品的任意账号接管漏洞](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487582&idx=1&sn=182f126192bc17124e50cbbc36c59b91&scene=21#wechat_redirect)  
  
  
[更新|帆软、用友、泛微、蓝凌等常见OA系统综合漏洞检测工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487310&idx=1&sn=f27b0f1199b3f52b086d1c7dc18685d0&scene=21#wechat_redirect)  
  
  
[Web渗透测试综合工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486715&idx=1&sn=1d507a1a89b525e5c1ad4d75d0d9eedc&scene=21#wechat_redirect)  
  
  
[Swagger漏洞检测工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487229&idx=1&sn=07acbbae48db8862efe4bb8062056a96&scene=21#wechat_redirect)  
  
  
[Java漏洞专项检测工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487205&idx=1&sn=e0d8353342f7d33fc3ce5bd5e747c861&scene=21#wechat_redirect)  
  
  
[渗透测试集成工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247487068&idx=1&sn=e343013c9f5a4f736865236e6dfd4bf0&scene=21#wechat_redirect)  
  
  
[AntiDebug_Breaker最新版,Hook必备](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486930&idx=1&sn=2684c990c45276e6fb23de5c48a1c85a&scene=21#wechat_redirect)  
  
  
[有趣的Fuzz+BucketTool工具等于双高危！](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486814&idx=1&sn=1d114995e1cf5745e824d2018cdbd615&scene=21#wechat_redirect)  
  
  
[推荐一款资产“自动化”筛选工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486679&idx=1&sn=288fe9f4312c3267c33765cfa9e3ab06&scene=21#wechat_redirect)  
  
  
[EDU SRC学号、账号等敏感信息收集工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486657&idx=1&sn=847f77601dd94eed5a5b04b5bf8176a1&scene=21#wechat_redirect)  
  
  
[Jeecg-boot最新漏洞检测工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486651&idx=1&sn=70d0e3d5ac4920684f52a4ee52531854&scene=21#wechat_redirect)  
  
  
[js.map文件还原组合工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486582&idx=1&sn=a08daa5cc01f04b813d827e5d8bf444b&scene=21#wechat_redirect)  
  
  
[Nacos漏洞检测专项工具,攻防必备](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486606&idx=1&sn=595219ca87296bd633d89d0e966d40a6&scene=21#wechat_redirect)  
  
  
[微信公众号，微信小程序，钉钉,飞书等第三方平台接管工具](https://mp.weixin.qq.com/s?__biz=MzkxMjg3NzU0Mg==&mid=2247486471&idx=1&sn=1a9ddf83c74be73ab70bbbf202771a19&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/RLTNmn7FBP6LllD9Qm4I2eKvyHt1WlND18ovUTvvzp4MagwzrEIAu6ZHoicVWA2YvfmEgZicxv4tvVibeFB8T7w3A/640?wx_fmt=gif&from=appmsg "")  
  
  
