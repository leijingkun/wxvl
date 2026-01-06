#  无法修补的漏洞：PS5 BootROM密钥遭泄露，索尼安全防线崩塌  
 FreeBuf   2026-01-06 10:30  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/qq5rfBadR38jUokdlWSNlAjmEsO1rzv3srXShFRuTKBGDwkj4gvYy34iajd6zQiaKl77Wsy9mjC0xBCRg0YgDIWg/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/qq5rfBadR39H4eicalbOEwZ1t8X3mSSZsyjE5KRbxA0pJ7bFIlCAWTsKXLZTHOdhiaib000nGYeesjxcDgKBIE6pA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
上周，一名身份不明的黑客泄露了索尼用于保护PlayStation 5游戏机信任链的关键安全密钥。这类被称为BootROM的安全密钥是索尼安全信任架构的核心组件。理论上，该密钥的曝光为未来针对游戏机的破解工作奠定了重要基础。  
  
  
**Part01**  
## BootROM密钥的核心作用  
  
  
BootROM密钥是PlayStation内部硬件的固有组成部分，存储在物理不可重写的只读存储器中。系统启动时，会立即调用该密钥验证引导加载程序的数字签名，随后才会初始化游戏机的操作系统内核。只有这些关键验证步骤全部完成后，索尼才会允许用户运行合法购买的游戏和应用程序。在此次泄露事件前，黑客和游戏机改装者主要专注于攻击PlayStation 5的操作系统内核或针对用户空间运行的基于WebKit的浏览器，但收效有限。而获取BootROM密钥后，研究人员现在能够深入探索游戏机的硬件架构和运行机制。  
  
  
**Part02**  
## 密钥来源与影响范围  
```

```  
  
经查证，泄露的密钥似乎源自游戏机破解社区的两位知名人物BrutalSam_和Shadzey1。目前业界普遍认为被曝光的BootROM密钥真实有效，不过其更广泛的应用还需要进一步深入研究。  
  
  
掌握该密钥后，开发者最终可能实现以下突破：  
  
- 创建自定义固件  
  
- 扩展游戏修改和模拟的生态系统  
  
- 最关键的是解锁部分索尼独占游戏  
  
不过值得注意的是，索尼独占游戏的数量正在持续减少，大多数现代游戏现已实现索尼、微软和PC平台同步发布。  
  
  
**Part03**  
## 索尼面临的修复困境  
```

```  
  
对索尼而言，处理这类密钥泄露事件存在重大技术挑战。作为硬件嵌入式安全密钥，BootROM无法轻易更换。但索尼可以在未来的硬件修订版本中引入新密钥，确保新生产的游戏机保持安全，同时现有设备的核心功能不会受到任何密钥轮换的影响。  
  
  
**参考来源：**  
  
The Unpatchable Leak: Sony’s PS5 Security Crumples as BootROM Keys Hit the Web  
  
https://securityonline.info/the-unpatchable-leak-sonys-ps5-security-crumples-as-bootrom-keys-hit-the-web/  
  
  
###   
###   
###   
  
**推荐阅读**  
  
[](https://mp.weixin.qq.com/s?__biz=MjM5NjA0NjgyMA==&mid=2651333596&idx=1&sn=a5f1d8decaf400a24f3b9e74a3a357e1&scene=21#wechat_redirect)  
###   
### 电台讨论  
  
****  
![](https://mmbiz.qpic.cn/mmbiz_png/qq5rfBadR3ibl7jGenyy4j8qWgan0RtibartNWPtzeeYghKJbbNPqS5rOJDpDr5YL7LGpoFibAV3davCAEvU2IESg/640?wx_fmt=png&from=appmsg "")  
  
****  
  
  
  
  
