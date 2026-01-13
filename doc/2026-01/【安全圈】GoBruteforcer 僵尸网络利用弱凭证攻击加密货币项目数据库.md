#  【安全圈】GoBruteforcer 僵尸网络利用弱凭证攻击加密货币项目数据库  
 安全圈   2026-01-13 11:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
暴力破解  
  
  
 新一波GoBruteforcer攻击瞄准了加密货币和区块链项目的数据库，将其纳入僵尸网络，**该网络能够对Linux服务器上的FTP、MySQL、PostgreSQL和phpMyAdmin等服务进行用户密码暴力破解**  
。  
  
“当前这一波攻击活动由**两个因素**  
驱动：大量重复使用人工智能生成的服务器部署示例，这些示例传播了常见的用户名和脆弱的默认设置；以及XAMPP等遗留Web堆栈的持续存在，这些堆栈暴露了FTP和管理界面，且安全加固程度极低。”Check Point Research在上周发布的分析报告中表示。  
  
GoBruteforcer最初由Palo Alto Networks Unit 42于2023年3月记录，其能够针对运行x86、x64和ARM架构的类Unix平台，部署一个互联网中继聊天僵尸程序和一个用于远程访问的Web Shell，同时获取暴力破解模块以扫描易受攻击的系统并扩大僵尸网络的覆盖范围。  
  
Lumen Technologies的Black Lotus Labs团队在2025年9月的一份后续报告中发现，由另一个名为SystemBC的恶意软件家族控制的大量受感染僵尸机器，也属于GoBruteforcer僵尸网络的一部分。  
  
Check Point表示，他们在2025年年中识别出一个更复杂的Golang恶意软件版本，该版本包含一个用跨平台编程语言重写的、经过深度混淆的IRC僵尸程序，改进了持久化机制、进程伪装技术和动态凭证列表。  
  
凭证列表包含了可以接受远程登录的常见用户名和密码组合（例如：myuser:Abcd@123  
 或 appeaser:admin123456  
）。选择这些名称并非偶然，因为它们曾在数据库教程和供应商文档中使用，而这些资料都被用于训练大型语言模型，导致模型生成的代码片段带有相同的默认用户名。  
  
列表中的其他一些用户名则专注于加密货币（例如：cryptouser  
、appcrypto  
、crypto_app  
 和 crypto  
）或针对phpMyAdmin面板（例如：root  
、wordpress  
 和 wpuser  
）。  
  
“攻击者在每次活动中重复使用一个稳定的小型密码池，从该池中刷新每个任务的列表，并每周数次轮换用户名和特定领域的补充内容，以追求不同的目标，”Check Point称。”与其他服务不同，FTP暴力破解使用的是硬编码在破解程序二进制文件中的一小套凭证。这套内置的凭证指向网络托管堆栈和默认服务账户。”  
  
根据Check Point观察到的活动，攻击者利用运行XAMPP的服务器上暴露在互联网的FTP服务作为初始入侵载体，上传一个PHP Web Shell，然后使用基于系统架构的Shell脚本来下载并执行更新版的IRC僵尸程序。**一旦主机被成功感染，它可以用于三种不同的用途：**  
1. 运行暴力破解组件，尝试对互联网上的FTP、MySQL、Postgres和phpMyAdmin进行密码登录。  
  
1. 托管并向其他被入侵的系统分发有效负载，或者  
  
1. 托管IRC风格的控制端点，或充当备用命令与控制（C2）服务器以增强弹性。  
  
对该攻击活动的进一步分析确定，其中一台被入侵的主机被用来部署一个模块，该模块遍历TRON区块链地址列表，并使用 tronscanapi[.]com  
 服务查询余额，以识别资金非零的账户。这表明了针对区块链项目的协同努力。  
  
“GoBruteforcer例证了一个更广泛且持久的问题：暴露的基础设施、脆弱的凭证和日益自动化的工具的组合，”Check Point表示。”虽然该僵尸网络在技术上相对简单，但其运营者受益于大量在线且配置不当的服务。”  
  
这一披露正值GreyNoise透露，威胁行为者正在系统地扫描互联网，寻找可能提供对商业LLM服务访问权限的配置不当的代理服务器。  
  
在这两个攻击活动中，有一个在2025年10月至2026年1月期间，利用了服务器端请求伪造（SSRF）漏洞，针对Ollama的模型拉取功能和Twilio SMS webhook集成。基于对ProjectDiscovery OAST基础设施的使用，推测该活动可能源于安全研究人员或漏洞赏金猎人。  
  
第二组活动始于2025年12月28日，据评估是一次大规模枚举尝试，旨在识别与阿里巴巴、Anthropic、DeepSeek、Google、Meta、Mistral、OpenAI和xAI相关的暴露或配置不当的LLM端点。扫描源自IP地址 45.88.186[.]70  
 和 204.76.203[.]125  
。  
  
“从2025年12月28日开始，两个IP地址启动了对超过73个LLM模型端点的系统性探测，”该威胁情报公司称。”在十一天内，他们生成了80,469个会话——这是对可能泄露商业API访问权限的配置不当代理服务器进行的系统性侦察。”  
  
  
 END   
  
  
阅读推荐  
  
  
[【安全圈】Everest黑客组织宣称入侵日产汽车公司](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073682&idx=1&sn=eaafe045322c2887048550f73ea679e9&scene=21#wechat_redirect)  
  
  
  
[【安全圈】FBI 警告：朝鲜黑客正将恶意 QR 码用于鱼叉式网络钓鱼](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073682&idx=2&sn=233af4cd5dfedb13752f0410332a3ec7&scene=21#wechat_redirect)  
  
  
  
[【安全圈】MuddyWater 黑客组织通过鱼叉式钓鱼向中东多部门传播 RustyWater 远程木马](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073682&idx=3&sn=3cb91003cca511deef1548fe3fc3aaac&scene=21#wechat_redirect)  
  
  
  
[【安全圈】新型网络犯罪工具 ErrTraffic 实现 ClickFix 攻击自动化 伪造网站故障诱骗用户中招](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073682&idx=4&sn=ca3fe952aae03b098b11ef4680bb8868&scene=21#wechat_redirect)  
  
  
  
  
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
  
  
  
  
