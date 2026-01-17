#  【工具推荐】通达OA漏洞综合利用工具  
 隐雾安全   2026-01-17 01:01  
  
隐雾安全  
  
**致力于为大家提供最实用的安全技能**  
  
  
  
**项目介绍**  
  
  
通达OA系统作为一款广泛应用于企业管理的办公自动化平台，提供了丰富的功能，包括文档管理、工作流审批、项目管理等。这些功能不仅提升了工作效率，也使得系统成为潜在的攻击目标。由于通达OA系统在设计和实现上的一些缺陷，黑客可以利用这些漏洞进行非法访问、数据窃取或系统控制等恶意行为  
  
本工具旨在帮助安全专家和网络管理员识别和利用通达OA系统中的安全漏洞，进行有效的漏洞评估和修复。通过对通达OA系统漏洞的深入分析  
  
注：本工具仅用于学习参考，任何事情与作者无关  
  
每次看到这个工具就让我想起我HW拿到的第一个shell，好不好用试试就知道。  
  
  
**漏洞检测**  
  
  
目前工具支持对以下漏洞的检测：  
  
集成常见通达OA漏洞，总计42个漏洞身份认证绕过：4个xss跨站脚本攻击：1个信息泄露：2个任意文件下载：2个前台SQL注入：13个后台SQL注入：4个前台文件上传：9个后台文件上传：6个  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ELQKhUzr34wV98XLnPEhmOicPGeocgD5DaGfBft2nYRrKZC0IEE3RKPqglf7IHH4ibuicAuWT4ars79fSY1fdQ7icw/640?wx_fmt=png&from=appmsg "")  
  
备注：通达OA v11.6前台任意文件删除+任意文件上传漏洞会删除auth.inc.php，这可能会损坏OA系统，请谨慎操作，切勿非法利用和测试  
  
  
  
**shell连接**  
  
  
本工具webshell采用蚁剑连接，密码均为x  
  
  
**免责声明**  
  
  
仅限用于技术研究和获得正式授权的攻防项目，请使用者遵守《中华人民共和国网络安全法》，切勿用于任何非法活动，若将工具做其他用途，由使用者承担全部法律及连带责任，作者及发布者不承担任何法律连带责任  
  
  
**下载地址**  
  
  
https://github.com/xiaokp7/TongdaOATool  
  
  
**网安交流群**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ELQKhUzr34wV98XLnPEhmOicPGeocgD5Dz9h3WNSyPU80oLupd4rANkynTpsVkIr17vhR90NKQTZRtibXpFPRO6w/640?wx_fmt=jpeg&from=appmsg "")  
  
**微信号**  
丨  
Hiddenfog001  
  
**往期内容**  
  
[通用0day挖掘思路](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247492296&idx=1&sn=0350ca481c64313904a1fc06da9c4f60&scene=21#wechat_redirect)  
  
  
[某大厂勒索病毒处置流程外泄](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247492246&idx=1&sn=469d15c6ac130d611b48f9098b832120&scene=21#wechat_redirect)  
  
  
[今年大一，不小心黑进学校的迎新系统怎么办](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247485696&idx=1&sn=b4e54d33ae5b6fae788c4c8089ead1df&scene=21#wechat_redirect)  
  
  
[英雄联盟租号平台getshell](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247496399&idx=1&sn=4d3b3d4a63539fcdab4ccfe24a5f115e&scene=21#wechat_redirect)  
  
  
[记一次色情APP的渗透过程](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247486334&idx=1&sn=c2be4746c5e59ebc6e21fe68fd00b85e&scene=21#wechat_redirect)  
  
  
**课程推荐**  
  
[隐雾SRC第八期全面升级](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247498743&idx=1&sn=7ac290a5ec7d7c9b6ab01256bee48a2a&scene=21#wechat_redirect)  
  
  
[零基础就业班-三包模式](https://mp.weixin.qq.com/s?__biz=MzkyNzM2MjM0OQ==&mid=2247498939&idx=1&sn=8a1ea75429e8f1258b8887b14602b051&scene=21#wechat_redirect)  
  
  
