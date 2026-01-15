#  Shiro漏洞利用工具，更新V0.2！  
e安在线  e安在线   2026-01-15 02:03  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1Y08O57sHWiahTldalExhOyzXNMO6kcO7ULmiclhSZfg8zVMLHEMUGBu3lBjFbjib8vsYDZzplofMSC7epkHHWpibw/640?wx_fmt=png&from=appmsg "")  
  
  
## 工具介绍  
  
Shiro漏洞利用工具，更新ShiroEXP V0.2  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJlrfClwxTh4V4ZviaFcVYZZqy0w0llCaB5axALWWTkc7EZTuxNT7cUqw/640?wx_fmt=png&from=appmsg&watermark=1&wxfrom=13&tp=wxpic#imgIndex=1 "")  
## 工具功能  
  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJkbgrgMicFpicOzlrwziafLxG6eNMSpfz5NOJbNuH9zYfdlwh6q5XWRnPQ/640?wx_fmt=png&from=appmsg&watermark=1&wxfrom=13&tp=wxpic#imgIndex=2 "")  
## 工具使用  
```
➜  ShiroEXP_jar git:(main) ✗ java -jar ShiroEXP.jar -h   _____    __      _                    ______   _  __    ____   / ___/   / /_    (_)   _____  ____    / ____/  | |/ /   / __ \  \__ \   / __ \  / /   / ___/ / __ \  / __/     |   /   / /_/ / ___/ /  / / / / / /   / /    / /_/ / / /___    /   |   / ____/ /____/  /_/ /_/ /_/   /_/     \____/ /_____/   /_/|_|  /_/                                                             v0.2 by Y5neKO :)                                                       GitHub: https://github.com/Y5neKOusage: java ShiroEXP.jar [-be] [-bk] [-c <arg>] [--cookie <arg>] [--gadget       <arg>] [--gadget-echo <arg>] [-h] [-k <arg>] [--mem-pass <arg>]       [--mem-path <arg>] [--mem-type <arg>] [--proxy <arg>] [-rf <arg>]       [-s] [--shell] [-u <arg>] -be,--brute-echo              爆破回显链 -bk,--brute-key               爆破key -c,--cmd <arg>                执行命令    --cookie <arg>             携带Cookie    --gadget <arg>             指定利用链    --gadget-echo <arg>        指定回显链 -h,--help                     打印帮助 -k,--key <arg>                指定key    --mem-pass <arg>           内存马密码    --mem-path <arg>           内存马路径    --mem-type <arg>           打入内存马类型(输入ls查看可用类型)    --proxy <arg>              设置代理(ip:port) -rf,--rememberme-flag <arg>   自定义rememberMe字段名 -s,--scan                     扫描漏洞    --shell                    进入Shell模式 -u,--url <arg>                目标地址
```  
  
**爆破key及加密方式**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJ65FY1xNRVicYv7CneAlTLNn3jzeNftT5a4Ufj1pwHZvVaDbYYO5kpZg/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=3 "")  
  
**漏洞验证**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJlrfClwxTh4V4ZviaFcVYZZqy0w0llCaB5axALWWTkc7EZTuxNT7cUqw/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=4 "")  
  
**爆破回显链**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJwGyx3aW5UHlL2X2u4tMmoIutrbWtkRSgbbib93mk2jzPmkpk3oWbapg/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=5 "")  
  
**命令执行**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJLlGbcw2IDbibRcN5retiaNLoj4dMyCAvK7vkribb9HWLdkBWvybwWHoicw/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=6 "")  
  
**Shell模式**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJrfoGPInQqEq1VoEPvQc5icJZXpMMUELqiamm5pIERHTD4ibNYda2xt8lg/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=7 "")  
  
**注入内存马**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJMIiaiaUcvyymic4iaotRw9Zzibnpz10iarP8cmTSrZ65Pj6kpD3jovQtA9ZA/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=8 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJXHMEHPWaE1FWvakE4x2Ceicia1Eh03hciahpMgoeMgib2HHuiaXQK8V6Y3g/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=9 "")  
  
**全局代理**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2Xd6oXzFqXtcAUuP2BQGibeJA3mPPECTeI28hmj66Maoyicqr74agqvsN3WEia8eV9z9Po03JUg8h0Bw/640?wx_fmt=png&from=appmsg&watermark=1&tp=wxpic&wxfrom=5&wx_lazy=1#imgIndex=10 "")  
  
  
## 工具获取  
  
  
https://github.com/Y5neKO/ShiroEXP  
  
  
  
声明：除发布的文章无法追溯到作者并获得授权外，我们均会注明作者和文章来源。如涉及版权问题请及时联系我们，我们会在第一时间删改，谢谢！  
**文章来源：夜组安全**  
  
  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1Y08O57sHWiaM9uv5Q89hYMT8zuKQtQYuvSPy0HyyLwRShZOMcoGgoBy6qiatgDhW3UhCXGVXiaEbS8ANmZwViaMAw/640?wx_fmt=jpeg&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&randomid=tug5rwn0&tp=wxpic#imgIndex=8 "")  
  
  
