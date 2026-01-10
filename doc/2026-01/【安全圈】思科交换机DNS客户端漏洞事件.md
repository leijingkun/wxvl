#  【安全圈】思科交换机DNS客户端漏洞事件  
 安全圈   2026-01-10 01:03  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
  
### 事件概述  
  
近日，多款思科（Cisco）交换机因固件中DNS客户端漏洞陷入反复重启循环，导致网络服务严重中断。该漏洞在凌晨2时左右集中显现，全球多个独立网络同时受到影响。  
#### 触发原因：  
  
漏洞源于DNS客户端解析思科域名及NTP时间服务器时出错，交换机将DNS解析失败判定为“致命错误”，触发重启机制。  
  
重启周期仅持续数分钟，导致设备反复重启，网络服务陷入瘫痪。  
### 受影响型号  
  
以下思科交换机型号受此次漏洞影响：  
- CBS250  
  
- SG350  
  
- Catalyst C1200  
  
究竟是全球范围同时触发，还是某种时间相关条件所致，仍有待进一步调查。  
### 临时解决方案  
#### 管理员应急措施：  
  
👉 **禁用DNS解析**  
：通过调整设备配置，关闭DNS客户端功能，避免因解析失败触发重启。  
  
👉 **关闭SNTP服务**  
：停止网络时间协议（SNTP）服务，可有效防止时间同步异常引发的重启循环。  
  
注意，即使DNS服务器正常可用，上述措施仍可有效阻止重启循环。  
### 后续进展  
  
思科目前尚未公开漏洞的确切根源，但已向客户确认问题存在，并正在积极开发修复补丁。  
  
官方建议用户留意后续公告，并及时应用更新补丁。  
  
  
   END    
  
  
阅读推荐  
  
  
[【安全圈】全球用户大面积中招：鼠标突然就“坏了”！不少人按到“手抽筋”，重装卸载也不管用，罗技回应](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073626&idx=1&sn=1106bc067a5b3ae39fc80cee319f95a1&scene=21#wechat_redirect)  
  
  
  
[【安全圈】海航系统故障出现bug票，成都80元起可飞全国，回应：已售出机票均有效](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073626&idx=2&sn=37a13cb2d37ea476fae941c52225aa62&scene=21#wechat_redirect)  
  
  
  
[【安全圈】互联网流量趋势显示伊朗已经完全断开国际互联网 其骨干网络提供商也下线](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073626&idx=3&sn=663d944c7492afc6bca642ff4538b5d5&scene=21#wechat_redirect)  
  
  
  
[【安全圈】三星 SSD 管理软件曝高危漏洞](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073616&idx=1&sn=00ddec83cd6f42d948271422e027e84c&scene=21#wechat_redirect)  
  
  
  
[【安全圈】黑客利用0Day漏洞工具包在野攻击VMware ESXi实例](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073616&idx=2&sn=741da551e46d5f17cae1ee738d1010fe&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEDQIyPYpjfp0XDaaKjeaU6YdFae1iagIvFmFb4djeiahnUy2jBnxkMbaw/640?wx_fmt=png "")  
  
**安全圈**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
←扫码关注我们  
  
**网罗圈内热点 专注网络安全**  
  
**实时资讯一手掌握！**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
**好看你就分享 有用就点个赞**  
  
**支持「****安全圈」就点个三连吧！**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
  
  
