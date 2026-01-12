#  溯源实例-从OA到某信源RCE攻击  
Alivin  神农Sec   2026-01-12 01:01  
  
扫码加圈子  
  
获内部资料  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXLicr9MthUBGib1nvDibDT4r6iaK4cQvn56iako5nUwJ9MGiaXFdhNMurGdFLqbD9Rs3QxGrHTAsWKmc1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=png&wxfrom=13&wx_lazy=1&wx_co=1&tp=wxpic "")  
  
  
#   
  
专注于SRC漏洞挖掘、红蓝对抗、渗透测试、代码审计JS逆向，CNVD和EDUSRC漏洞挖掘，以及工具分享、前沿信息分享、POC、EXP分享。不定期分享各种好玩的项目及好用的工具，欢迎关注。加内部圈子，文末有彩蛋（知识星球优惠卷）。  
#   
  
文章作者：  
Alivin  
  
文章来源：https://forum.butian.net/share/1765  
  
01  
  
0x1   
溯源实例-从OA到某信源RCE攻击  
# 0x01 序言  
  
国Hvv真实溯源过程，在流量设备告警能力弱的情况下，重人工介入分析整个过程总结，回顾当时整个溯源过程和0day的捕获过程，尝试把当时的心境和技术上的思考点梳理出来，给大家参考，批评。  
# 0x02 溯源过程  
  
事件起源于4月9日午后的一则来自EDR的webshell告警，如下：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E9dVficH2foKic0mEpsAYbjjbknH68v8ENtGicJSFuNd0T2icyZzsickv9NA/640?wx_fmt=png&from=appmsg "")  
  
  
马上展开对该服务器的排查，该服务器为某信源VRV，纯内网环境。说明攻击者已经进入内网环境，分两条线分别对攻击入口和内网影响面进行排查。  
## 2.1 向内溯源，确定影响面  
### 2.1.1 某信源VRV溯源  
#### 2.1.1.1 从日志分析  
  
因为某信源VRV的管理后台使用SSL协议，在协调厂商提供证书的同时，对access.log日志进行分析，尝试找出其中的攻击入口。  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EGyd6AjEoLtHcjNBRZpnImPFTjffj38dLRlqJXWumbCia7NdIjmW6rTA/640?wx_fmt=png&from=appmsg "")  
  
  
从日志寻找入口的思路：  
  
（1）定位webshell访问接口，确认攻击跳板的IP  
  
（2）对webshell访问前后日志进行分析，确定漏洞URL  
  
（3）将可疑URL在其他时段日志中进行搜索，找出在其他时段没出现过的URL重点分析  
  
（4）对POST请求的日志重点分析。  
  
通过对日志分析，发现10.*.*.*2在对VRV服务器尝试扫描，扫描日志符合fscan等  
  
类型内网扫描工具的流量，如下：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E7DbnSCUf4zFADuBTKW8VKfnuRzkeMldvfeJAtoXNibDje2HFkXKaS1w/640?wx_fmt=png&from=appmsg "")  
  
  
此时已经定位了攻击某信源VRV的主机为10.*.*.*2，同时安排其他同事对该IP进行溯源分析。  
  
该部分扫描无影响，然后继续分析，发现攻击队成功登陆了audit账号：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E49VHciaZuRG8OqHnhoVtiaicuZRkL1bvczsiaOIoYNcsFPZiaZGzSzxUiaYA/640?wx_fmt=png&from=appmsg "")  
  
  
结合测试，发现audit账户为弱口令123456，同时在audit审计账户的后台中发现system也被登录过，同样为弱口令。（PS：此时猜测admin用户也是弱口令，经过测试并不是。）  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EpXn5nmgHZKsPCibbFANOsJMRUyAZMctv3NBm8hNP1ibqUTxiaZHJaY2icA/640?wx_fmt=png&from=appmsg "")  
  
  
时间和IP都和攻击者路径对得上。但audit和system账户权限有限，并不会直接控制终端。  
  
此外，在审计用户的后台还发现，admin账户的密码被修改过,操作者时admin本人，登录IP为攻击队控制的跳板机，修改后密码后，admin账户成功登录。  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EDCKoKicAXXapJGSCaNvlfeTWhZA8zZa3nesuzgAcF459eIt90icetQ5w/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3ElXq4S1wH56HsNQqLBTPrwibNomwzMBzYuO5a4rndkZIYgFfweym3Siaw/640?wx_fmt=png&from=appmsg "")  
  
  
到这得到的信息，总觉得是因为admin账户密码被重置导致的整台服务器实现（如果是admin弱口令的话，就不会存在admin修改自己密码的操作了。）  
继续分析日志，发现在logo.aspx文件被访问前，还曾访问过logo.txt。  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EwOFvAicNjeACt4eRuuoGNz6mEOvRLEMibhHv7WWwK5XhLhOTB1v7aBNA/640?wx_fmt=png&from=appmsg "")  
  
  
而且logo.txt的访问中存在一次404的访问，说明马没写成功。那么比对两次logo.txt访问前被访问的接口，大概率可以定位到漏洞存在点。  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3Eia71hAKInibn7lpKIgz4nv1LmqhjUJsW9HicX7xFvL0DdBpoM88iatldgQ/640?wx_fmt=png&from=appmsg "")  
  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EcmXVDjloy0gHGb0IwMqaDJEg3Fibmh3MAmM82F2ibyHC5gjm2j6IiacpQ/640?wx_fmt=png&from=appmsg "")  
  
成功定位/VRVEIS/SystemMan/GetNavUserByNavGuid.aspx就是漏洞文件，而该文件能写入shell，且从日志看，该路径应该需要admin权限才能访问。  
  
那么问题来了，admin账号权限咋来的？带着疑问，先分析GetNavUserByNavGuid.aspx被访问的日志：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3Er6dFtXPppzrVGMfyew8eiccvlFTsLsnDDRhyxgFV7fJo6QztjIWlvug/640?wx_fmt=png&from=appmsg "")  
  
在admin用户登录前，已经出现了大量的接口调用和访问，里面奇迹般的记录了一个POST的body，把思路引向了注入（后话：最后证明pczq参数与漏洞利用无关）。  
  
恰好如果是注入的话，也解释的通admin账号的来源和写文件的操作。mssql支持堆叠注入，update操作可以改密码，xp_cmdshell可以用于写shell。  
  
而在登录admin之前，登录audit、system账号的行为，原因大概是因为admin用户的密码复杂，cmd不可解。所以先解开audit和system的密码登录的。  
#### 2.1.1.2 从流量分析  
  
从流量分析已经是几个小时以后的事情了，某信源厂家并不愿意提供证书对流量进行解密。但是在不经意间看到VRV根目录下有一个cert目录，在其中找到证书。  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EwrGTUibfj0lH3CO2wj8c3wLGfhLwEjvc5UIrX7hNWwR3xkgulGkrHcA/640?wx_fmt=png&from=appmsg "")  
  
  
证书有加密，尝试以后发现证书密码是123。  
  
此时终于有了明文流量了，直接搜索接口，验证上面从日志中溯源的结论，如下：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EibNKMbz5n4GVx5SvVBY7oibP1sMuyKGwgn38JGyiaTKJ9BFziaPkCOQFxw/640?wx_fmt=png&from=appmsg "")  
  
  
与日志分析结论一致，且在流量中有发现修改admin的密码。（时间久远，找不到数据包截图了，payload截图如下）  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3Ewp5Wy5pYNCbmhwt6WoefmTibPVNZZTFaCicic1GUGia1TxCcaj5KVuBBOg/640?wx_fmt=png&from=appmsg "")  
  
#### 2.1.1.3 杂记  
  
某信源这个漏洞是0day，后来因为客户要求，将细节给了某信源，某信源还特地发了公告：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EZVrK9nra7LbcU9KqgMxZBMhH96WUGRf3dnFj1s3yfkRCo22NS7ib69w/640?wx_fmt=png&from=appmsg "")  
  
  
具体漏洞分析过程见3.1  
### 2.1.2 某信源后台失陷的影响面  
  
shell在上传后，我们马上进行了处置，从某信源服务器进行拓展是来不及的，所以我们重点从某信源后台失陷后，攻击者都干了些什么来确定影响面。  
众所周知，某信源是终端管控系统，其最常用的攻击方法就是通过后台对管控的终端进行下发文件/执行命令等方式操作进行利用。于是在日志中找到相应的接口进行分析：  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EJ2kHA1ibSttYRcgUqyKogavEjUj3OVst4sBdj8wSXJVRSLMia1qHRURQ/640?wx_fmt=png&from=appmsg "")  
  
  
根据DeviceID可以确定出被攻击的机器具体是哪台，最终梳理出一个表，最后使用时间都在被攻击之前，如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3Egr5N5wAaEUBECNPToJsSib3eRfM2c7razv4yWl3Cd5HlAiccBastdBtw/640?wx_fmt=png&from=appmsg "")  
  
也不敢大意，在流量侧重点监控了几个IP的流量，没有明显异常行为，对PC都进行了查杀、进程、启动项、注册表分析，确认未被攻击者控制。此时已经凌晨3点多。  
  
第二天与客户沟通后发现，该VRV是用于VPN接入时进行管控的，而VPN在4月8日晚上已经关闭。  
### 2.1.3 10.*.*.*2失陷后的影响面  
  
该失陷主机在DMZ区，且开放对互联网的访问，所以大概率这就是首台被攻破的机器了。  
  
凌晨3点多，我同事还没有完整的梳理出该机器的影响面，于是我参与其中一起梳理。（PS：客户第二天一早要看到影响面，不敢怠慢）。  
  
分析思路：  
（1）以10.*.*.*2作为源IP，分析其对内网的整个访问流程中的异常流量。  
（2）分析10.*.*.*2上的木马文件、攻击者工具等文件，在分析影响面的同时也寻找被攻击的点。  
（3）关联服务器分析  
  
整个分析展开时，由于流量设备告警能力弱（可以说连SQL注入都不怎么告警），分析依靠蛮力介入的比较大。整体看下来就是扫描流量非常多，但分析是否成功实在工作量太大了。  
  
在扫描文件的时候，发现服务器上仍存在攻击队未删除的fscan扫描结果，如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EeNQCOkP9rCRAqTk4GfkNAjj7SiaZXR8adgEACGQDepdiaME9n6sYRDvw/640?wx_fmt=png&from=appmsg "")  
  
整理后，结合流量进行分析，发现攻击队共计探测到28台内网机器（含10.*.*.*2本身），其中存在漏洞的情况如下：  
- 10.*.*.*2 存在MS17-010漏洞（本机），DMZ区域做了策略优化，禁止了该区域内的445访问，所以其只能访问自己的445。  
  
- 10.*.*.*7 存在Druid未授权访问漏洞，未发现流量中有利用行为。  
  
- 10.*.*.*1 存在MySQL弱口令，未在流量中发现进一步漏洞利用。  
  
- 10.*.*.*5 存在某信源SQL注入0day  
  
- 10.*.*.*3 被成功登录了数据库  
  
其本身情况就是这样了，然后对其关联的服务器进行分析，因为该应用站库分离，用的是mssql，使用的是sa用户，IP为10.*.*.*3，在流量中也证实该机器被成功登录了mssql。那极有可能通过xp_cmdshell已经获取到了数据库服务器的权限。  
  
通过上机排查10.*.*.*3，发现xp_cmdshell已经被激活，但未从数据库日志里面找到xp_cmdshell被调用的记录，无法得知攻击队用xp_cmdshell做了什么。  
  
在流量侧对10.*.*.*3进行分析，仅发现其与10.*.*.*2（应用）有流量交互，不存在向内网扩散的行为。  
## 2.2 向外溯源，查找入口点  
  
之所以把对DMZ区域的攻击过程溯源放的比较靠后，是因为该机器出现问题后一直处于断网状态，所以不急着分析。  
### 2.2.1 寻找线索，发现端倪  
  
对于10.*.*.*2的被攻击的路径是一点线索也没有，所以上去先对进程、文件、定时任务、启动项、网络连接进行检查，状况如下：  
  
（1）定时任务、进程、启动项里面没有驻留的后门  
  
（2）网络连接只有外网访问该应用的，并没有由内向外的C2回连（因为不出网）  
  
（3）文件方面只发现了fscan，竟然没有发现webshell。  
  
此时可以说是一头雾水，又整理了一下手里的信息：  
  
（1）服务器处于DMZ区，向外提供服务  
  
（2）服务是e-mobile，当时未暴出0day（ps：溯源到的第二天细节公开了）  
  
（3）无文件落地，可能是用了内存马（ps：不排除攻击过程中有文件落地）  
  
于是，使用cop对内存马进行检测，如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EN4j89XHwDFKxNu6uvLjrGQia8D1iatibRVd04ppnQwEQrqGVuuDSjUTWQ/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E9UNbaUXO9XyPhRh0xByODIX6GJuoVrfEx5KB4icQ9vZzM8hR1nn0R2g/640?wx_fmt=png&from=appmsg "")  
  
果然发现了内存马，然后找到内存马对应的java文件，如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EJRBukKondUS1Y4Fv9vwn51e3ZYq7D2lAMjpPsrU6J9ChStg6MSibXXw/640?wx_fmt=png&from=appmsg "")  
  
根据对样本进行分析，发现该内存马的特征流量为返回包的set-cookie中包含eagleeye-traceid字段，对流量中包含该特征的流量进行检索，如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E2NwP2arO05FBLp7KEQQtnpibGfFibLt6LYGrHufhGXibSNl9rXC7lxtPA/640?wx_fmt=png&from=appmsg "")  
  
发现两个IP有过webshell连接的请求，随后对两个IP的的流量进行分析，发现两个IP的交互都很有目标,直接就连接了webshell，未发现其进行其他攻击操作。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EJ3ScV0l7uVf09zlALKus7kYia1sibjOusIbpIBERKHAZqoXuOhv4xWoA/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EcR1hhljZTUO4ibPOgjVEtNlbzDyS6gKB2SdDSEdfk1q8CNFWIAJS8NQ/640?wx_fmt=png&from=appmsg "")  
  
然后又开始了苦逼的分析。  
### 2.2.2 深入分析，找到过程  
  
通过对内存马访问前后流量的排查，未发现直接上传webshell的操作（服务没shell文件，不排除内存马植入后删除了木马，所以排查了webshell上传行为）。  
  
想到可能是直接执行代码将内存马加载到内存中的，搜索关键字loadClass，截图如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EsIvuwyGwdATeibbsK53c2rFFC7ylFqeUOpsXVTUb0nwiaNwzhrGiaKeibA/640?wx_fmt=png&from=appmsg "")  
  
从而定位到攻击IP和加载内存马的过程。根据攻击IP筛选，还原整个漏洞利用过程。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E7ynQ6ESRiabwHXVjS29mbPfXXanfT9kDxsZzhER8WqqIs1iaDUQbFpfg/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EfwIw14nuPJEQrV68R03NlpqOibuDVB0l7IILGXyibgzSziafXzhxgxyQg/640?wx_fmt=png&from=appmsg "")  
  
漏洞为SQL注入，通过创建别名的方式执行java代码，将内存马的字节码文件写入到tmp目录下的tmpD591.tmp中，字节码文件较长，所以进行了多次追加写入，然后在最后调用java.net.URLClassLoader类将tmpD591.tmp字节码文件加载到内存中。  
  
随后上机排查，在tmp目录下发现tmpD591.tmp,截图如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EVfHaLiaVRH1xKTzCgDNSslExPfVssFZxLQWrPNfwibZ6QyxUG1PtWWOw/640?wx_fmt=png&from=appmsg "")  
  
通过对字节码文件进行分析，发现其中包含了一个名为resin.class的字节码文件。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EBdLbT7RB9v8crw9TFVmUF1MlOsyXym06eOaC7ER1XavTv9Jkt8EfLw/640?wx_fmt=png&from=appmsg "")  
  
通过分析代码，证实该文件是Resion的内存马。至此，整个溯源过程结束，跨时2天。  
# 0x03 漏洞分析  
## 3.1 E-moblie注入分析  
  
当我们在为发现两枚0day而窃喜的时候，第二天E-moblie这个漏洞就被公开了。下面是分析过程，看官们直接跳转，不赘述了。  
https://forum.butian.net/share/84  
## 3.2 某信源SQL注入分析  
  
找到漏洞文件，代码如下：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EAUnCWbsxovpRBupaWH32Fibwjv28icsqacQwe7jO5nt2VbNRwdCQCL1g/640?wx_fmt=png&from=appmsg "")  
  
通过反编译VRV的dll文件，找到该方法的实现：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EsWGsfIK4qnggiacic244HHicicmsSJj4kHJsuKLKY1OqNMDsK2jGsph2Xg/640?wx_fmt=png&from=appmsg "")  
  
直接从request中拿到了navGuid参数，然后带入了GetListByNavGuid方法，跟踪该方法：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3E1mMJiciclCYUDMxQgORoWtuFjj1nn14mIozmrL2SqbQUBBmZdEwWDE4g/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3Egiao8n5Ssw9qOOBDEqxfcYxeo7U0vWmPgl0krLmiaOpra6iaVibACX5XUg/640?wx_fmt=png&from=appmsg "")  
  
直接将参数拼接到SQL语句中产生的注入。  
# 0x04 总结  
  
时隔1年多，整理手中的材料时想拿出来做分享，部分过程没有找到相对应的截图。各位看官将就一下。站在上帝视角，回顾整个过程，颇有收获：  
  
（1）DMZ区的被攻破应用的数据库就在核心区域，而且核心区域访问关系不清晰，没有做严格的分级分域，里面全部互通。如果攻击队通过10.*.*.*3进行资产扫描，我们早就退场了。这也给了我以后打红队的启发。  
  
（2）在分析过程中，对于漏洞攻击过程的追求远大于对事件影响的排查，也庆幸在甲方的督促下，我注意到了其中的重要性。  
  
（3）关于项目开发，某信源对0day的解释是某项目的定制需求，定制需求还放在标准产品中，显然是审计工作不够充分。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EMrHWVibbqriax9hMcDmj9hKC7ohl0iaTaqmdzuGr5uUjy1KJ4rQiaTuARA/640?wx_fmt=png&from=appmsg "")  
  
（4）某信源系统多个不被人关注到的账号，都是123456这个弱密码。这类问题不止体现在某信源，其他产品也是，所以作为防守方应该注意这些问题。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUiaf00iaRPklhELcuHYoGa3EkOFNOO01QyROrlibe2sFkGiaFkbnia3LZfR9RX3UvJstbwSkTibkXlDbSw/640?wx_fmt=png&from=appmsg "")  
  
02  
  
0x2 内部小圈子详情介绍  
  
我们是  
神农安全  
，点赞 + 在看  
 铁铁们点起来，最后祝大家都能心想事成、发大财、行大运。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mngWTkJEOYJDOsevNTXW8ERI6DU2dZSH3Wd1AqGpw29ibCuYsmdMhUraS4MsYwyjuoB8eIFIicvoVuazwCV79t8A/640?wx_fmt=png&tp=wxpic&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/MVPvEL7Qg0F0PmZricIVE4aZnhtO9Ap086iau0Y0jfCXicYKq3CCX9qSib3Xlb2CWzYLOn4icaWruKmYMvqSgk1I0Aw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
**内部圈子介绍**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/MVPvEL7Qg0F0PmZricIVE4aZnhtO9Ap08Z60FsVfKEBeQVmcSg1YS1uop1o9V1uibicy1tXCD6tMvzTjeGt34qr3g/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
  
  
  
**圈子专注于更新src/红蓝攻防相关：**  
  
```
1、维护更新src专项漏洞知识库，包含原理、挖掘技巧、实战案例
2、知识星球专属微信“小圈子交流群”
3、微信小群一起挖洞
4、内部团队专属EDUSRC证书站漏洞报告
5、分享src优质视频课程（企业src/EDUSRC/红蓝队攻防）
6、分享src挖掘技巧tips
7、不定期有众测、渗透测试项目（一起挣钱）
8、不定期有工作招聘内推（工作/护网内推）
9、送全国职业技能大赛环境+WP解析（比赛拿奖）
10、十个专栏会持续更新~提前续费有优惠，好用不贵很实惠
11、每日内部资料分享，内部圈子资料1000+
12、联系圈主获取：内部漏洞知识库+圈子使用手册+内部圈子交流群
13、VX：routing_love，技术交流+疑问解决
```  
  
  
  
  
**内部圈子**  
**专栏介绍**  
  
知识星球内部共享资料截屏详情如下  
  
（只要没有特殊情况，每天都保持更新）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWYcoLuuFqXztiaw8CzfxpMibRSekfPpgmzg6Pn4yH440wEZhQZaJaxJds7olZp5H8Ma4PicQFclzGbQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWYcoLuuFqXztiaw8CzfxpMibgpeLSDuggy2U7TJWF3h7Af8JibBG0jA5fIyaYNUa2ODeG1r5DoOibAXA/640?wx_fmt=png&from=appmsg "")  
  
  
**知识星球——**  
**神农安全**  
  
星球现价   
￥50元  
  
如果你觉得应该加入，就不要犹豫，价格只会上涨，不会下跌  
  
星球人数少于1400人 50元/年  
  
星球人数少于1600人 65元/年  
  
（新人优惠卷20，扫码或者私信我即可领取）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWWXXxypODTreRYn1aKwInjsYxnFkfcLeKvNE9kEj6GLp4I2r5ZUOoWaqJqXdgLj44b2Mk84u3uRbQ/640?wx_fmt=png&from=appmsg "")  
  
欢迎加入星球一起交流，券后价仅50元！！！ 即将满1400人涨价  
  
长期  
更新，更多的0day/1day漏洞POC/EXP  
  
  
  
  
**内部知识库--**  
**（持续更新中）**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFu12KTxgSfI69k7BChztff43VObUMsvvLyqsCRYoQnRKg1ibD7A0U3bQ/640?wx_fmt=png&from=appmsg "")  
  
  
**知识库部分大纲目录如下：**  
  
知识库跟  
知识星球联动，基本上每天保持  
更新，满足圈友的需求  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFhXF33IuCNWh4QOXjMyjshticibyeTV3ZmhJeGias5J14egV36UGXvwGSA/640?wx_fmt=png&from=appmsg "")  
  
  
知识库和知识星球有师傅们关注的  
EDUSRC  
和  
CNVD相关内容（内部资料）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFKDNucibvibBty5UMNwpjeq1ToHpicPxpNwvRNj3JzWlz4QT1kbFqEdnaA/640?wx_fmt=png&from=appmsg "")  
  
  
还有网上流出来的各种  
SRC/CTF等课程视频  
  
量大管饱，扫描下面的知识星球二维码加入即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUw2r3biacicUOicXUZHWj2FgFxYMxoc1ViciafayxiaK0Z26g1kfbVDybCO8R88lqYQvOiaFgQ8fjOJEjxA/640?wx_fmt=png&from=appmsg "")  
  
  
  
不会挖CNVD？不会挖EDURC？不会挖企业SRC？不会打nday和通杀漏洞？  
  
直接加入我们小圈子：  
知识星球+内部圈子交流群+知识库  
  
快来吧！！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWUMULI8zm64NrH1pNBpf6yJ5wUOL9GnsxoXibKezHTjL6Yvuw6y8nm5ibyL388DdDFvuAtGypahRevg/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWUMULI8zm64NrH1pNBpf6yJO0FHgdr6ach2iaibDRwicrB3Ct1WWhg9PA0fPw2J1icGjQgKENYDozpVJg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1 "")  
  
  
神农安全知识库内部配置很多  
内部工具和资料💾，  
玄机靶场邀请码+EDUSRC邀请码等等  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDNtEt0PfMwXQRzn9EDBdibLWNDZXVVjog7wDlAUK1h3Y7OicPQCYaw2eA/640?wx_fmt=png&from=appmsg "")  
  
  
快要护网来临，是不是需要  
护网面试题汇总  
？  
问题+答案（超级详细🔎）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDbLia1oCDxSyuY4j0ooxgqOibabZUDCibIzicM6SL2CMuAAa1Qe4UIRdq1g/640?wx_fmt=png&from=appmsg "")  
  
  
最后，师傅们也是希望找个  
好工作，那么常见的  
渗透测试/安服工程师/驻场面试题目，你值得拥有！！！  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWXjm2h60OalGLbwrsEO8gJDicYew8gfSB3nicq9RFgJIKFG1UWyC6ibgpialR2UZlicW3mOBqVib7SLyDtQ/640?wx_fmt=png&from=appmsg "")  
  
  
内部小圈子——  
圈友反馈  
（  
良心价格  
）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWW0s5638ehXF2YQEqibt8Hviaqs0Uv6F4NTNkTKDictgOV445RLkia2rFg6s6eYTSaDunVaRF41qBibY1A/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/b7iaH1LtiaKWW0s5638ehXF2YQEqibt8HviaRhLXFayW3gyfu2eQDCicyctmplJfuMicVibquicNB3Bjdt0Ukhp8ib1G5aQ/640?wx_fmt=png&from=appmsg "")  
  
  
**神农安全公开交流群**  
  
有需要的师傅们直接扫描文章二维码加入，然后要是后面群聊二维码扫描加入不了的师傅们，直接扫描文章开头的二维码加我（备注加群）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b7iaH1LtiaKWXQQxwDn7nBel5rw7fdWUCoCr3O7DicI6OKcr5k2hbfORsyBiaFWWrHxNtAq0skiaZUuuGGAEE6AXYQA/640?wx_fmt=jpeg&from=appmsg "")  
```
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/b7iaH1LtiaKWW8vxK39q53Q3oictKW3VAXz4Qht144X0wjJcOMqPwhnh3ptlbTtxDvNMF8NJA6XbDcljZBsibalsVQ/640?wx_fmt=gif "")  
  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=13&tp=wxpic "")  
  
**往期回顾**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=13&tp=wxpic "")  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[手把手js逆向断点调试&js逆向前端加密对抗&企业SRC实战分享](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247495361&idx=1&sn=48283073b325e360823da8dec27a7508&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[浅谈src漏洞挖掘中容易出洞的几种姿势](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247489731&idx=1&sn=c3a5ef01648fad496ecda36b653b6e21&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[HVV护网行动 | 分享最近攻防演练HVV漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247488672&idx=1&sn=493bb70011a02eb971ff1b74c733f1d9&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[攻防演练｜分享最近一次攻防演练RTSP奇特之旅](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492377&idx=1&sn=a94ad30e30e08bd96e888dad744e9814&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[JS漏洞挖掘｜分享使用FindSomething联动的挖掘思路](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492315&idx=1&sn=88991e98058a277e267a9a79b8518e16&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[渗透测试 ｜ 从jeecg接口泄露到任意管理员用户接管+SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493292&idx=1&sn=611fd43361089a30e5f7bcda21274b95&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[分享SRC中后台登录处站点的漏洞挖掘技巧](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247485439&idx=1&sn=3fd7e4cef57edca8e73104f8af38fc05&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[企业SRC支付漏洞&EDUSRC&众测挖掘思路技巧操作分享](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247492839&idx=1&sn=b9781f60580c1da8e2151166f0494ba5&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[渗透测试 ｜ 分享某次项目上的渗透测试漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493495&idx=1&sn=791bebc6faa651cc3c585c2f5f481d21&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[【宝典】分享云安全浪潮src漏洞挖掘技巧](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247494877&idx=1&sn=2d00c0f651fd7375e881be86638e53ce&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[实战SRC挖掘｜微信小程序渗透漏洞复盘](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247494468&idx=1&sn=f0da4b4ff7763cbb83b858fb5a8964f9&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[综合资产测绘 | 手把手带你搞定信息收集](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493749&idx=1&sn=d2e0febcdcf9dcd8aa44be0d43b51936&scene=21#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1 "")  
  
[【宝典】针对若依系统nday的常见各种姿势利用](https://mp.weixin.qq.com/s?__biz=Mzk0Mzc1MTI2Nw==&mid=2247493489&idx=1&sn=d3ef10a1ae3b8c161d7174cb42702fac&scene=21#wechat_redirect)  
  
  
  
