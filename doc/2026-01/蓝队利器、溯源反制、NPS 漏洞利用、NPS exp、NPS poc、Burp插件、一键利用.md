#  蓝队利器、溯源反制、NPS 漏洞利用、NPS exp、NPS poc、Burp插件、一键利用  
原创 weishen250  W小哥   2026-01-08 03:30  
  
# npscrack burp插件  
  
  
蓝队利器、溯源反制、NPS 漏洞利用、NPS exp、NPS poc、Burp插件、一键利用  
  
最近做攻防演练发现了很多内网穿透的工具，其中最多的就是nps，红队老哥好像还挺喜欢这个的，真的是多，每天导出攻击IP，浅浅扫一下端口，基本都能发现这个nps。贼多 ![image.png](https://mmbiz.qpic.cn/mmbiz_png/XZByrJJ6uUweTDYunX7L5UJMVricbpnaqYYZpz4YWNkHSxjvialE8iaVEOuo0ZF1Pl1BZMEqnJQfJ6XaLYYUWBgfQ/640?wx_fmt=png&from=appmsg "")  
 NPS存在一个身份验证的缺陷，无需登录，直接进后台，后台功能点全都可以用。具体利用是伪造两个参数auth_key、timestamp。但是这俩参数的生命周期只有20秒，20秒过后就需要重新伪造，利用的时候，经常失效，还要不停的复制粘贴。非常的操蛋。 由于本人长年优雅，最看不得这种事，于是抽时间搞了一个小插件，一键解决所有问题。github连接：https://github.com/weishen250/npscrack。  
## 使用方法  
  
  
插件所有的功能集成到了Burp的右键中，首先访问nps站点，拦截请求包。 开启插件 ![image.png](https://mmbiz.qpic.cn/mmbiz_png/XZByrJJ6uUweTDYunX7L5UJMVricbpnaqObtKCJakrriaHUB03ZGQIWaVztavEic1ZxgUEUWjcRDkKTOCXBg2oHcw/640?wx_fmt=png&from=appmsg "")  
  
  
下面的这些功能为nps系统中主要的功能点。 ![image.png](https://mmbiz.qpic.cn/mmbiz_png/XZByrJJ6uUweTDYunX7L5UJMVricbpnaq2jZnf9rIbu0jkA6j3273XRz2pQVXB8GYubfab8mHf5picyT0l4KX1NQ/640?wx_fmt=png&from=appmsg "")  
 点击会修改请求包，之后直接放行数据包。其他的什么都不用管了 ![image.png](https://mmbiz.qpic.cn/mmbiz_png/XZByrJJ6uUweTDYunX7L5UJMVricbpnaq7jjFMmb4dNINebtSzf9YKgWg0RIdKlXMYHnMwSkVnKaKvEB2iaph5Qw/640?wx_fmt=png&from=appmsg "")  
 每一个请求都会自动贴上身份验证参数，非常的优雅，非常适合优雅且端庄的高级工程师。  
  
关注公众号回复“  
20260102  
”获取工具地址  
。  
  
