#  电子宠物“龙虾”（OpenClaw 🦞）偷拍，窃密漏洞复现  
 迪哥讲事   2026-03-11 01:51  
  
我不明白，为什么大家都在谈论着OpenClaw，仿佛这OpenClaw就注定能解决我们日常生活中的所有问题。一年前，DeepSeek横空出世，开启了新一轮AI竞赛，AI应用自此蓬勃发展，各种智能体争相涌现，真可谓占尽天时，那种勃勃生机、万物竞发的境界，犹在眼前。短短一年之后，AI应用竟至于沦落至此，只剩下OpenClaw这种漏洞百出的产品了么？  
  
从2023年春节后chatGPT爆火开始，这一轮AI爆发，最直接，最大受益者依旧是从事内容生成为主的自媒体+二道贩子(贩卖焦虑，倒卖免费开源产品，卖课程等等)，这类群体目前在舆论上占比是很大的，他们也利用AI批量化工具进行变相的进行舆论引导宣传。其次是科技公司，如AI大模型公司，算力公司，云厂商等。  
  
直接说OpenClaw有漏洞，甩一堆漏洞列表，非专业人士来说就很抽象，不好理解，无法直观感受到危害性。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/m3tfzlbEQPqpCDNz4S5xjx0gyAehlvjicpUY8icDE38HUsQdjCrlXPI5lyJl97B5Vvcnfap29CZVODSCQyE7nEsXjm4QhbUKGKrcDdNvGKQy8/640?wx_fmt=jpeg&from=appmsg "")  
  
对于AI来说，更大的威胁是提示词注入和数据投毒。其攻击方式通过语言文字，普通人简单的复制粘贴就能实现攻击，从而导致数据泄露，执行危险命令，服务器权限丢失等  
  
什么是提示词注入？ 我相信下面的截图，很多人应该都在群里看过下面这张图的内容  
  
![](https://mmbiz.qpic.cn/mmbiz_png/m3tfzlbEQPoUjffxEZjZdOic0FVza8OkG3aKZTEeJuseE2fcFMomaKpJrPIDBMnsN0oJ6vZWaCPJMRdU4fLQQv1PFVxESpLmWnjnQW5iaHVDE/640?wx_fmt=png&from=appmsg "")  
  
这其实就是一个提示词注入攻击，  
攻击者通过构造恶意提示词，诱导模型绕过预设的安全约束，执行非预期的操作或泄露敏感信息。  
  
很多人将OpenClaw拉入微信、QQ或者飞书群，而忽略了提示词注入的风险。两个真实案例，带大家看看其危害性。  
  
原本想自己复现的，搭环境时候看到  
复旦白泽战队发了视频，就懒得复现了，这里就直接搬运。  
  
通过提示词注入，打开"养虾人"的摄像头  
  
  
蠕虫攻击  
  
OpenClaw在浏览到恶意帖子时，也会被诱导执行敏感操作。  
  
攻击者首先在发布一篇**精心设计的恶意帖子**  
，帖子里面隐藏一段“指令式内容”，要求agent忽视之前的所有指令，转而执行恶意指令；当OpenClaw正常浏览读到这篇帖子时，会瞬间被“洗脑”，转而无条件执行攻击者植入的恶意指令。最终导致：服务器被控制、系统被入侵等严重安全问题  
  
  
AI只是工具，如果你自身专业素质不强，对所在领域专业知识深度和广度都不够的情况下，也难利用AI做出什么好东西，最后只会被AI取代。  
  
在OpenClaw爆火的情况下，再次提醒诸位，提升安全意识，提高政治站位，尤其是党政机关事业单位，不要为了一时追热度，搞宣传，盲目上OpenClaw，导致政务内外网被打穿，产生严重的网络与信息安全问题。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/m3tfzlbEQPoVg3aMXdPCMS3ibsY2NCv6LMnmicRtbglRLMWpibwMI624egHw4BGYp9NKg0rj5Iteic4BGzpAhHseYibIEt4ib7f2NMvJODW9ibbl9w/640?wx_fmt=jpeg&from=appmsg "")  
  
如果你是一个长期主义者，欢迎加入我的知识星球，本星球日日更新,包含号主大量一线实战,全网独一无二，微信识别二维码付费即可加入，如不满意，72 小时内可在 App 内无条件自助退款  
  
![](https://mmbiz.qpic.cn/mmbiz_png/YmmVSe19Qj5EMr3X76qdKBrhIIkBlVVyuiaiasseFZ9LqtibyKFk7gXvgTU2C2yEwKLaaqfX0DL3eoH6gTcNLJvDQ/640?wx_fmt=png&from=appmsg "")  
  
往期回顾  
#   
# 如何利用ai辅助挖漏洞  
#   
# 如何在移动端抓包-下  
#   
# 如何绕过签名校验  
#   
  
[一款bp神器](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247495880&idx=1&sn=65d42fbff5e198509e55072674ac5283&chksm=e8a5faabdfd273bd55df8f7db3d644d3102d7382020234741e37ca29e963eace13dd17fcabdd&scene=21#wechat_redirect)  
  
  
[挖掘有回显ssrf的隐藏payload](https://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247496898&idx=1&sn=b6088e20a8b4fc9fbd887b900d8c5247&scene=21#wechat_redirect)  
  
  
[ssrf绕过新思路](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247495841&idx=1&sn=bbf477afa30391b8072d23469645d026&chksm=e8a5fac2dfd273d42344f18c7c6f0f7a158cca94041c4c4db330c3adf2d1f77f062dcaf6c5e0&scene=21#wechat_redirect)  
  
  
[一个辅助测试ssrf的工具](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247496380&idx=1&sn=78c0c4c67821f5ecbe4f3947b567eeec&chksm=e8a5f8dfdfd271c935aeb4444ea7e928c55cb4c823c51f1067f267699d71a1aad086cf203b99&scene=21#wechat_redirect)  
  
  
[dom-xss精选文章](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247488819&idx=1&sn=5141f88f3e70b9c97e63a4b68689bf6e&chksm=e8a61f50dfd1964692f93412f122087ac160b743b4532ee0c1e42a83039de62825ebbd066a1e&scene=21#wechat_redirect)  
  
  
[年度精选文章](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247487187&idx=1&sn=622438ee6492e4c639ebd8500384ab2f&chksm=e8a604b0dfd18da6c459b4705abd520cc2259a607dd9306915d845c1965224cc117207fc6236&scene=21#wechat_redirect)  
  
  
[Nuclei权威指南-如何躺赚](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247487122&idx=1&sn=32459310408d126aa43240673b8b0846&chksm=e8a604f1dfd18de737769dd512ad4063a3da328117b8a98c4ca9bc5b48af4dcfa397c667f4e3&scene=21#wechat_redirect)  
  
  
[漏洞赏金猎人系列-如何测试设置功能IV](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486973&idx=1&sn=6ec419db11ff93d30aa2fbc04d8dbab6&chksm=e8a6079edfd18e88f6236e237837ee0d1101489d52f2abb28532162e2937ec4612f1be52a88f&scene=21#wechat_redirect)  
  
  
[漏洞赏金猎人系列-如何测试注册功能以及相关Tips](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486764&idx=1&sn=9f78d4c937675d76fb94de20effdeb78&chksm=e8a6074fdfd18e59126990bc3fcae300cdac492b374ad3962926092aa0074c3ee0945a31aa8a&scene=21#wechat_redirect)  
  
[‍](http://mp.weixin.qq.com/s?__biz=MzIzMTIzNTM0MA==&mid=2247486764&idx=1&sn=9f78d4c937675d76fb94de20effdeb78&chksm=e8a6074fdfd18e59126990bc3fcae300cdac492b374ad3962926092aa0074c3ee0945a31aa8a&scene=21#wechat_redirect)  
  
  
  
  
