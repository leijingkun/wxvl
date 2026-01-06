#  Shiro漏洞利用工具，更新V0.2！  
Y5neKO  夜组安全   2026-01-06 00:00  
  
免责声明  
  
由于传播、利用本公众号夜组安全所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号夜组安全及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
**所有工具安全性自测！！！VX：**  
**baobeiaini_ya**  
  
朋友们现在只对常读和星标的公众号才展示大图推送，建议大家把  
**夜组安全**  
“**设为星标**  
”，  
否则可能就看不到了啦！  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2WrOMH4AFgkSfEFMOvvFuVKmDYdQjwJ9ekMm4jiasmWhBicHJngFY1USGOZfd3Xg4k3iamUOT5DcodvA/640?wx_fmt=png&from=appmsg "")  
  
## 工具介绍  
  
Shiro漏洞利用工具，更新ShiroEXP V0.2  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJgOADpeIqGGYNdZtstfju3QZo1LPKGSu96ycxJpIQXia3j9hpd6GBicYg/640?wx_fmt=png&from=appmsg "")  
## 工具功能  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJ0KLhLPKCIDs7dUxzzaGOuKzrLCf4IoJdGSlyqVzKDy2qWu7xV3DURQ/640?wx_fmt=png&from=appmsg "")  
## 工具使用  
```
➜  ShiroEXP_jar git:(main) ✗ java -jar ShiroEXP.jar -h   _____    __      _                    ______   _  __    ____   / ___/   / /_    (_)   _____  ____    / ____/  | |/ /   / __ \  \__ \   / __ \  / /   / ___/ / __ \  / __/     |   /   / /_/ / ___/ /  / / / / / /   / /    / /_/ / / /___    /   |   / ____/ /____/  /_/ /_/ /_/   /_/     \____/ /_____/   /_/|_|  /_/                                                             v0.2 by Y5neKO :)                                                       GitHub: https://github.com/Y5neKOusage: java ShiroEXP.jar [-be] [-bk] [-c <arg>] [--cookie <arg>] [--gadget       <arg>] [--gadget-echo <arg>] [-h] [-k <arg>] [--mem-pass <arg>]       [--mem-path <arg>] [--mem-type <arg>] [--proxy <arg>] [-rf <arg>]       [-s] [--shell] [-u <arg>] -be,--brute-echo              爆破回显链 -bk,--brute-key               爆破key -c,--cmd <arg>                执行命令    --cookie <arg>             携带Cookie    --gadget <arg>             指定利用链    --gadget-echo <arg>        指定回显链 -h,--help                     打印帮助 -k,--key <arg>                指定key    --mem-pass <arg>           内存马密码    --mem-path <arg>           内存马路径    --mem-type <arg>           打入内存马类型(输入ls查看可用类型)    --proxy <arg>              设置代理(ip:port) -rf,--rememberme-flag <arg>   自定义rememberMe字段名 -s,--scan                     扫描漏洞    --shell                    进入Shell模式 -u,--url <arg>                目标地址
```  
  
**爆破key及加密方式**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJejiccEJKq06tKzXiaYv1POKn51OKv1IU19m0ZeprSNUFftuDY3WcswEQ/640?wx_fmt=png&from=appmsg "")  
  
**漏洞验证**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJgOADpeIqGGYNdZtstfju3QZo1LPKGSu96ycxJpIQXia3j9hpd6GBicYg/640?wx_fmt=png&from=appmsg "")  
  
**爆破回显链**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJPRhsTyKvzhvj2prWMYNP9TLhL83xssunwu9hST5x8G0HjFC6oLYibeg/640?wx_fmt=png&from=appmsg "")  
  
**命令执行**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJppIBZYSuUib4aH5zelHTMiblfAhlU1f12n0fGme2QdH2dyeYXxmueKpw/640?wx_fmt=png&from=appmsg "")  
  
**Shell模式**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJibn0xm2CXz66KuIFBwd0HRialjiaFkYPPCAicc8EztYgLicLy1y47HL9JibQ/640?wx_fmt=png&from=appmsg "")  
  
**注入内存马**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJIjUufA4vqiakKibr0Ecic5jdzeB1Y88odTP3kbMjcnuUzCnRqI6GWDKlA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJonQQTtEO8Sbv1OnGuGiaOeia2QFer1KlRBdxbv6qyXVibOu30tibt3VNtw/640?wx_fmt=png&from=appmsg "")  
  
**全局代理**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJFHRSqUULib9f3vjBic0xGVNys5ibfLqHQ85Pq8kCqZARbLQHvkZ7NZvCA/640?wx_fmt=png&from=appmsg "")  
  
  
## 工具获取  
  
  
  
点击关注下方名片  
进入公众号  
  
回复关键字【  
260106  
】获取  
下载链接  
  
  
## 往期精彩  
  
  
[Burp Suite 插件 | 利用AI为复杂的 HTTP 请求自动生成 Fuzz 字典2026-01-05](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496046&idx=1&sn=290789889a0d658d97d2e1e7d4ea0a23&scene=21#wechat_redirect)  
[一款轻量级的CTF/渗透测试Fuzz工具 | 多编码方式、多线程、代理转发2026-01-04](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496045&idx=1&sn=040873c2f104d627857d7fa4666645c0&scene=21#wechat_redirect)  
[25年结束了 黑客们有啥收获？2025-12-31](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496036&idx=1&sn=ef3a0c4684435cc9fed64c8a49542298&scene=21#wechat_redirect)  
[一个功能强大的 Docker 远程 API 漏洞利用工具2025-12-30](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247496019&idx=1&sn=ab3beb8bc4246f83556fb702856c9fa8&scene=21#wechat_redirect)  
[DNSLOG、HTTPLOG无回显漏洞测试辅助平台 | 辅助渗透测试过程中无回显漏洞及SSRF等漏洞的验证和利用2025-12-29](https://mp.weixin.qq.com/s?__biz=Mzk0ODM0NDIxNQ==&mid=2247495986&idx=1&sn=103764658f5011233a32103adddb1aa4&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/OAmMqjhMehrtxRQaYnbrvafmXHe0AwWLr2mdZxcg9wia7gVTfBbpfT6kR2xkjzsZ6bTTu5YCbytuoshPcddfsNg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&random=0.8399406679299557&tp=webp "")  
  
