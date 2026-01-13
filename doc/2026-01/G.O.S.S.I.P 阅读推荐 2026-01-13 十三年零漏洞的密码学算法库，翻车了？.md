#  G.O.S.S.I.P 阅读推荐 2026-01-13 十三年零漏洞的密码学算法库，翻车了？  
何同学  安全研究GoSSIP   2026-01-13 12:09  
  
今天是1月13日，我们来说一个和13相关的新闻：密码学社区最近有  
一款  
 13 年来从来没获得安全漏洞编号（也就是CVE编号）的密码算法库 libsodium，竟然被它的作者自己打破了这个“不败金身”  
：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicdfzKrO21Hn6QFbnHEXticO2MpJ3c05g2okyj3wqcxF03iaO7eo4JgCvJX9Ws5FfdX0ZZ1v8vyduIXd6kWAPK6Q/640?wx_fmt=png&from=appmsg "")  
  
libsodium 的作者 Frank Denis 当年开发这款密码算法库的目标，本质上是在追随 Dan Bernstein（没错，就是大名鼎鼎的DJB，也是 Salsa20 和 Ed25519 的设计者）的思想 —— 简单、易用和可靠。libsodium 从一开始就继承了另一款知名密码算法库 NaCl（也是DJB开发）的设计哲学，希望开发者只需要调用少量高层 API，就能安全地完成加密。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicdfzKrO21Hn6QFbnHEXticO2MpJ3c05gddjY44pwvib2l5Cw6AhhtP0JOblxhiabOhLhInIJxudFIicq8nA4bJohA/640?wx_fmt=png&from=appmsg "")  
  
Frank 还提出过一个非常有意思、也很现实的开发观：**一个好的 API 未必是设计上最完美的，但一定是不轻易改变的**  
。因为对开发者来说，最友好的 API 不是“优雅”，而是“多年之后升级依然不用改代码”。  
  
但是近几年画风突变，越来越多的人开始**直接使用 libsodium 的底层 API**  
，把它当成一个密码学基础原语工具箱。对此，作者本人也在公开文章中表达过一种颇为心碎的无奈：“That made me sad”。  
  
这次的问题也正是出现在一个很底层的函数crypto_core_ed25519_is_valid_point()  
 里。起因是 Frank 最近准备增加批量签名的功能，结果发现 Zig 的实现和 libsodium 的结果居然对不上。最终发现是自己在 13 年前**少写了一个检查条件**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicdfzKrO21Hn6QFbnHEXticO2MpJ3c05g5bLia0GMGPkRrNB75f8Pj34MqQ70ae0zzia2ibh5lNJX7S85CuUF4qFHg/640?wx_fmt=png&from=appmsg "")  
  
具体来说，crypto_core_ed25519_is_valid_point()  
 这个函数的职责，是判断一个 Ed25519 椭圆曲线点是否属于正确的主子群。它的基本思路是：将该点与主子群的阶 L 相乘，如果结果是无穷远点（也就是群的单位元），那么这个点才是合法的。  
  
在投影坐标表示中，无穷远点需要同时满足两个条件：X == 0  
 且 Y == Z  
。理论上，这两个检查缺一不可。  
  
但在实际实现中，代码只检查了 X == 0  
，却遗漏了对 Y == Z  
 的验证。结果就是：只要某个点与 L 相乘后恰好满足 X == 0  
，哪怕它并不是真正的无穷远点，也会被错误地判定为属于主子群。  
  
这里作者举了一个很简单的例子，任意一个主子群的点 Q，只要加上一个点 (0, -1) 就不再是主子群的点（等价于把原来的点横纵坐标都取反），但是会被上述存在漏洞的判断认为在主子群上。至于为什么这个点不在椭圆曲线上，且投影坐标 X == 0，读者可以参考以下两个 Hint 自己思考一下：  
  
1、（0，-1）是Ed25519上阶为 2 的小子群上的点。  
  
2、一个点 P 在主子群 等价于 [L]P 为无穷远点（零点）  
  
听到这里很多人可能已经开始紧张了，但好消息是：**绝大多数人完全不用慌**  
。常用的签名接口不受影响，正常生成的密钥也都是安全的。只有使用 Low-level API 的“高阶玩家”（？），才可能踩到这个坑。  
  
当然，作为 libsodium 之父，Frank Denis 也喜提 libsodium 的第一个 CVE：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicdfzKrO21Hn6QFbnHEXticO2MpJ3c05gO0edqO72miaxb5L4okWWHxpicSDFp4U7O0YrQ73VM9UqwsDeZhIYUicGw/640?wx_fmt=png&from=appmsg "")  
  
显然，最后的patch也非常简单，**就是把忘掉的检查补上**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicdfzKrO21Hn6QFbnHEXticO2MpJ3c05gWNrAkwQO4zEL2bMadOxZdcmmzGhFHMhQPA9TKiceWibpDCtSHPwIBH2w/640?wx_fmt=png&from=appmsg "")  
  
最后作者顺手又安利了一波：**听说你还在用 Ed25519？Ristretto255 了解一下！更快更安全**  
。如果还想要了解作者更多的心路历程的话，大家可以去读一下他的博客原文：https://00f.net/2025/12/30/libsodium-vulnerability/ 了解更多细节！  
  
  
