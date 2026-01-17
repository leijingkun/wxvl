#  【安全圈】AMD Zen 1~5全系CPU曝出StackWarp漏洞  
 安全圈   2026-01-17 11:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
漏洞  
  
  
科技媒体 cyberkendra 昨日（1 月 16 日）发布博文，报道称 CISPA 亥姆霍兹中心的研究人员披露名为 StackWarp 的高危硬件漏洞，该漏洞波及 AMD 自 2017 年 Zen 1 至 2024 年 Zen 5 的全系处理器。  
  
  
StackWarp 利用了堆栈引擎（一项性能优化功能）中的同步故障，操控一个未公开的模型特定寄存器（MSR 0xC0011029）控制位，该控制位用于启用或禁用堆栈指针跟踪。  
  
  
恶意的云提供商可以通过 CPU 线程切换该控制位，引发连锁反应，影响运行加密虚拟机的同级线程。这种操作会让 CPU “冻结”堆栈指针的架构更新。  
  
  
  
  
在攻击者重新启用引擎后，累积的偏移量会瞬间“释放”，导致堆栈指针发生高达 640 字节的确定性位移。值得注意的是，这种攻击无需访问访客明文或注入中断，完全避开了传统安全监控。  
  
  
研究人员通过演示证实了 StackWarp 的破坏力。在加密操作过程中，攻击者仅需通过精准的堆栈操控破坏一个签名，即可提取出 RSA-2048 私钥。  
  
  
此外，攻击者还能利用该漏洞将堆栈移动 32 个字节，直接跳过 OpenSSH 的密码验证环节，或者通过破坏系统调用的返回值，将普通用户权限提升至 Root 级。这意味着，原本被企业视为“数据保险箱”的机密计算环境，可能已沦为攻击者的后花园。  
  
  
  
  
  
  
AMD 目前已确认该漏洞，并分配了 CVE-2025-29943 编号，同时向企业客户发布了微码补丁。然而，在硬件级修复方案随下一代处理器问世前，现有的临时解决方案代价高昂。  
  
  
为了彻底阻断攻击路径，用户需要禁用同步多线程（SMT / 超线程）技术。这一操作虽然有效，**但会直接导致可用 CPU 核心数减半，迫使企业在保障数据安全与维持计算性能之间做出痛苦的抉择。**  
  
****  
  
  
 END   
  
  
阅读推荐  
  
  
[【安全圈】腾讯FPS大作突发崩溃引热议！玩家抱怨:网费谁补偿？](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073743&idx=1&sn=d569c1593013a127f457b74734010c74&scene=21#wechat_redirect)  
  
  
  
[【安全圈】数据泄露蝴蝶效应！外卖平台惨遭黑手](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073743&idx=2&sn=2608532ec97f40796a2f1f6f05a4ad87&scene=21#wechat_redirect)  
  
  
  
[【安全圈】美国最大运营商 Verizon 服务一度中断十小时、波及数十万人，官方承诺补偿](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073743&idx=3&sn=68404eced1fbed01846f09613e81f0e2&scene=21#wechat_redirect)  
  
  
  
[【安全圈】俄语黑客伪装以色列组织，Sicarii勒索软件实为假旗行动](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073743&idx=4&sn=f07e381df80a18c3eb1b66f071e961b2&scene=21#wechat_redirect)  
  
  
  
  
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
  
  
  
  
