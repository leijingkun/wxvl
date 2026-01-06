#  Shiro漏洞利用工具 -- ShiroEXP（1月4日更新）  
Y5neKO  Web安全工具库   2026-01-06 02:11  
  
===================================  
  
**免责声明**  
  
请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。工具来自网络，  
安全性自测  
，  
大家都要把工具当做病毒对待，在虚拟机运行。  
如有侵权请联系删除。个人微信：  
ivu123ivu  
  
  
**0x01 工具介绍**  
  
Shiro漏洞利用工具，主要功能如下：  
```
爆破key及加密方式(已完成)
漏洞探测(已完成Shiro550 URLDNS探测)
探测回显链(已完成CB1+TomcatEcho、Spring、AllEcho回显链)
漏洞利用(已完成命令执行、Shell模式)
注入内存马(支持蚁剑、冰蝎、哥斯拉等filter、servlet类型)
全局代理(已完成)
```  
  
**0x02 安装与使用**  
  
**爆破key及加密方式**  
  
  
**漏洞验证**  
  
  
**爆破回显链**  
  
  
**命令执行**  
  
  
**Shell模式**  
  
  
**注入内存马**  
  
  
  
**全局代理**  
  
## 自行编译打jar包时，可能会遇到找不到或无法加载主类的问题，经排查是因为bcprov包中的签名文件，打包后删除*.DSA、*.SF文件即可   
<table><tbody><tr><td data-colwidth="287"><section><span leaf=""><img class="rich_pages wxw-img" data-aistatus="1" data-imgfileid="100034149" data-ratio="1.4015518913676042" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/8H1dCzib3UibuzZMmDJXkGjLC9jdfcib0wMKwrhmbpdz9YMaPIOWNqeHbkALhuHouhdhZGY2I2ia7TIQdTibPfBjjZg/640?wx_fmt=jpeg&amp;from=appmsg" data-w="1031" type="inline"/></span></section></td><td data-colwidth="287"><section><span leaf=""><img class="rich_pages wxw-img" data-aistatus="1" data-imgfileid="100033627" data-ratio="1.2469635627530364" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/8H1dCzib3UibsC4yYFwgTnJrN0q57DearHJhaWSE6XQllpkUviaibg5MqTYgdUQYDNt8ysfV2v6o4jsN34pmq3DAOg/640?wx_fmt=jpeg&amp;from=appmsg" data-type="jpeg" data-w="1235" type="inline"/></span></section></td></tr></tbody></table>  
  
  
  
**·****今 日 推 荐**  
**·**  
  
  
> 《格蠹新编——软件调试以战说法》本书通过 63 个真实案例，以故事形式深度聚焦软件调试这一关键技术，直面发生在真实产品中的真实故障，并介绍定位故障的调试工具和方法。案例中涉及的硬件包括经典的 x86 和新兴的 ARM；涉及的软件平台主要是 GNU/Linux 系统；涉及的上层软件包括 Chrome 浏览器、英伟达 GPU 驱动、微信、腾讯会议、阿里旺旺、银行软件等。书中涵盖常见的各类软件问题，包括应用程序崩溃、多线程死锁、驱动程序故障、系统级挂死和崩溃等。书中设计了一些动手试验，以供读者上手小试牛刀。  
  
  
  
  
