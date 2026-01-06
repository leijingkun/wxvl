#  Shiro漏洞利用工具，更新V0.2！  
Y5neKO  无影安全实验室   2026-01-06 15:05  
  
免责声明：  
本篇文章仅用于技术交流，  
请勿利用文章内的相关技术从事非法测试  
，  
由于传播、利用本公众号无影安全  
实验室所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号无影安全实验室及作者不为此承担任何责任，一旦造成后果请自行承担！  
如有侵权烦请告知，我们会立即删除并致歉。谢谢！  
所有工具安全性自测！！！  
**VX：smile62157**  
  
  
  
朋友们现在只对常读和星标的公众号才展示大图推送，建议大家把"**无影安全实验室**  
"设为星标，这样更新文章也能第一时间推送！  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3GHDOauYyUGbiaHXGx1ib5UxkKzSNtpMzY5tbbGdibG7icBSxlH783x1YTF0icAv8MWrmanB4u5qjyKfmYo1dDf7YbA/640?&wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
安全工具  
  
  
  
## 0x01 工具介绍  
  
Shiro漏洞利用工具，更新ShiroEXP V0.2。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBqQXRcc4YENEEAGiaaPvfxcoqIF2vkolgJsZUCjMSXfXxOfoUaiaNsOwQ/640?wx_fmt=png&from=appmsg "")  
  
## 0x02 工具功能  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/awCdqJkJFER16OuauX8z57pFsF6rOicHBqORVbEJKsogd1O1FJIGut4OBicekM35Fqaxu0nevPEfdibofGChZ8DwQ/640?wx_fmt=jpeg "")  
## 0x03 工具使用  
```
➜  ShiroEXP_jar git:(main) ✗ java -jar ShiroEXP.jar -h   _____    __      _                    ______   _  __    ____   / ___/   / /_    (_)   _____  ____    / ____/  | |/ /   / __ \  \__ \   / __ \  / /   / ___/ / __ \  / __/     |   /   / /_/ / ___/ /  / / / / / /   / /    / /_/ / / /___    /   |   / ____/ /____/  /_/ /_/ /_/   /_/     \____/ /_____/   /_/|_|  /_/                                                             v0.2 by Y5neKO :)                                                       GitHub: https://github.com/Y5neKOusage: java ShiroEXP.jar [-be] [-bk] [-c <arg>] [--cookie <arg>] [--gadget       <arg>] [--gadget-echo <arg>] [-h] [-k <arg>] [--mem-pass <arg>]       [--mem-path <arg>] [--mem-type <arg>] [--proxy <arg>] [-rf <arg>]       [-s] [--shell] [-u <arg>] -be,--brute-echo              爆破回显链 -bk,--brute-key               爆破key -c,--cmd <arg>                执行命令    --cookie <arg>             携带Cookie    --gadget <arg>             指定利用链    --gadget-echo <arg>        指定回显链 -h,--help                     打印帮助 -k,--key <arg>                指定key    --mem-pass <arg>           内存马密码    --mem-path <arg>           内存马路径    --mem-type <arg>           打入内存马类型(输入ls查看可用类型)    --proxy <arg>              设置代理(ip:port) -rf,--rememberme-flag <arg>   自定义rememberMe字段名 -s,--scan                     扫描漏洞    --shell                    进入Shell模式 -u,--url <arg>                目标地址
```  
  
**爆破key及加密方式**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBqQXRcc4YENEEAGiaaPvfxcoqIF2vkolgJsZUCjMSXfXxOfoUaiaNsOwQ/640?wx_fmt=png&from=appmsg "")  
  
**漏洞验证**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBVl2dbH5pcxic8WTsol0uXhFeicdBwIqc2TLgkBQiaSMke5CY3UDAJ9lgw/640?wx_fmt=png&from=appmsg "")  
  
**爆破回显链**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHB8aeEbU8PBlOhxlFaBhPbmqAOibQ3GKkfGoLWeWIhv4chqoIpAibJKQ1A/640?wx_fmt=png&from=appmsg "")  
  
**命令执行**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBjw2desm2YKFejPUJcoUMqIgancTx07UOL3HcIewl9R8hySDXOSddibA/640?wx_fmt=png&from=appmsg "")  
  
**Shell模式**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBJkqn6r8PV2IOrBS2jfadpL6tVah6bd7hDuADjUibS23wusbiaHWDKy4A/640?wx_fmt=png&from=appmsg "")  
  
**注入内存马**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBYxptBk4XILfpNZibC3Al1f2L74260zlC0icklSicjaDBuPic9yoqic4674Q/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBibR4zAw4LYn0b9gI85RRhhb1YL4fMPo8zdI8AUictqG5u01ex8P60Dew/640?wx_fmt=png&from=appmsg "")  
  
****  
**全局代理**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/awCdqJkJFER16OuauX8z57pFsF6rOicHBGxqREX5ZI1cfQNCofSjzrp5GU2AaDLmRrAWSpbMzKtgGe8icqJRYsYg/640?wx_fmt=png&from=appmsg "")  
  
## 0x04 工具下载  
  
**点****击关注**  
**下方名片****进入公众号**  
  
**回复关键字【260106****】获取**  
**下载链接**  
  
  
  
最后推荐一下内部  
小  
密圈，干货满满，物超所值，**内部圈子每增加100人，价格将上涨20元，越早进越优惠！！！**  
  
****  
