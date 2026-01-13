#  SpringBoot-Scan：针对Spring Boot的漏洞扫描和渗透工具  
原创 网安武器库  网安武器库   2026-01-13 16:24  
  
**更多干货  点击蓝字 关注我们**  
  
  
  
**注：本文仅供学习，坚决反对一切危害网络安全的行为。造成法律后果自行负责！**  
  
**往期回顾**  
  
  
  
  
  
  
·[ANDRAX：一款强大的Android智能手机渗透测试平台](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485870&idx=1&sn=43339f93a01271dc201eee9e174be4ad&scene=21#wechat_redirect)  
  
  
  
  
  
  
·  
[WLAN爆破神器aircrack-ng：WIFI快速密码爆破工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485855&idx=1&sn=60708f498fed71a1cea55d36bbb8bed5&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Burpsuite插件：autorize实现抓包实时越权漏洞检测](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485847&idx=1&sn=727a77f7e2c484c9abdcb773b263b438&scene=21#wechat_redirect)  
  
  
  
  
  
  
·  
[Tor浏览器：匿名浏览器实现网络IP隐身](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485793&idx=1&sn=fde90afa005a931568a23c5d36e29882&scene=21#wechat_redirect)  
  
  
  
  
  
  
·  
[本地电脑没有PIN密码也可以实现文件窃取? Windows11WinRE权限控制绕过漏洞演示](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485773&idx=1&sn=b923c1ccf9852c8dab35b514a9e93c7e&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Apt_t00ls：Java 开发的OA设备高危漏洞集成利用工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485760&idx=1&sn=8ff7bd1519db2edf019ed5125997e6e6&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**前言介绍**  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQXwtepnvHD9ibcdZrKOibHV4VJtGoYRxzicPwPASlY0xDCtu2DibSnaxWcQ/640?wx_fmt=png&from=appmsg "")  
  
      
SpringBoot-Scan是一款专门针对Spring Boot应用的综合性漏洞检测与安全评估工具。它不是Spring官方产品，而是由国内安全研究人员AabyssZG开发并维护的开源安全工具，属于第三方安全审计工具范畴。  
  
      
该工具体现了 "以攻击者视角防御" 的安全理念，通过模拟真实攻击者的技术手法，主动发现Spring Boot应用中的安全弱点，实现"先于攻击者发现问题"的防御目标。  
  
      
SpringBoot-Scan  
具有以下核心价值，  
专门针对Spring Boot技术栈进行深度检测，相比通用扫描器具有更高的检测精度；能  
将复杂的手工安全测试流程自动化，大幅提升安全测试效率；同时  
内置Spring Boot相关历史漏洞检测规则，持续更新安全威胁情报。  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**安装介绍**  
  
  
  
    在安装目标文件夹内打开终端，并输入以下  
内容，  
从GitHub克隆（下载）项目：  
```
# 使用 git clone 命令克隆仓库到本地
git clone https://github.com/AabyssZG/SpringBoot-Scan.git
https://github.com/AabyssZG/SpringBoot-Scan
```  
  
    然后进入到项目内部：  
```
# 进入克隆下来的项目目录
cd SpringBoot-Scan
```  
  
    如果在使用git克隆时出现问题  
（如速度慢或失败），可以尝试在GitHub页面上手动下载ZIP压缩包并解压，然后进入解压后的目录。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQFr59pyicNeH9OxuVrByKiboXlZQQ9EufSupfG7l30q3RJKC024eib2yoQ/640?wx_fmt=png&from=appmsg "")  
  
    接着安装python依赖库，  
此类项目通常有一个名为 requirements.txt 的文件，它列出了所有必需的Python库。你需要用 pip 命令来安装它们：  
```
pip install -r requirements.txt
```  
  
    
    
如果pip安装速度慢，可以采用国内源进行安装：  
```
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/
```  
  
    安装好python依赖库后便可以正常使用了，可以通过help查看帮助，详细了解如何使用：  
```
python springboot-scan.py --help
```  
  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**功能与使用介绍**  
  
  
  
    运行项目中的SpringBoot-Scan.py，即可看到以下内容，证明可以正常使用：  
```
python3 SpringBoot-Scan.py
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQDAVJnnjylMicx7599XNDClI0KjHickSONrxcpC75ACABxajQndXAnbHA/640?wx_fmt=png&from=appmsg "")  
  
    可以看到，在这个项目中详细介  
绍了项目用法，SpringBoot应用信息泄露扫描  
、漏洞扫描  
、敏感文件下载  
三大模  
块。下面详细展示了所有用法：  
```
python3 SpringBoot-Scan.py -u http://example.com/ # 对单一URL进行信息泄露扫描
python3 SpringBoot-Scan.py -uf url.txt            #读取目标TXT进行批量信息泄露扫描
python3 SpringBoot-Scan.py -v http://example.com/ # 对单一URL进行漏洞扫描
python3 SpringBoot-Scan.py -vf url.txt            # 读取目标TXT进行批量漏洞扫描
python3 SpringBoot-Scan.py -d http://example.com/ # 扫描并下载SpringBoot敏感文件
python3 SpringBoot-Scan.py -df url.txt            # 读取目标TXT进行批量敏感文件扫描
python3 SpringBoot-Scan.py -p <代理IP:端口>        # 使用HTTP代理并自动进行连通性测试
python3 SpringBoot-Scan.py -t header.txt          # 从TXT文件中导入自定义HTTP头部
python3 SpringBoot-Scan.py -z <ZoomEye的API-KEY>  # 通过ZoomEye密钥进行API下载数据
python3 SpringBoot-Scan.py -f <Fofa的API-KEY>     # 通过Fofa密钥进行API下载数据
python3 SpringBoot-Scan.py -y <Hunter的API-KEY>   # 通过Hunter密钥进行API下载数据
```  
  
    下面挑选几个功能进行讲解。  
  
  
1、  
对单一URL进行敏感端点爆破  
  
     
   
探测SpringBoot应用暴露的敏感信息，如监控端点(Actuator)、配置信息、日志等。我们在终端中输入：  
```
python3 SpringBoot-Scan.py -u # http://example.com/
```  
  
    选择目标URL后运行，输入扫描延时后进入扫描。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQMyqzShia25m3kNb7wyleenqLdqyrmAKibbcbVY9iaTBMyHAezrQIPtzjQ/640?wx_fmt=png&from=appmsg "")  
  
  
2、  
读取目标TXT进行批量信息泄露扫描  
  
    项目支持批量信息扫描，  
从txt文件读取目标列表，并进行扫描，我们在终端中输入：  
```
python3 SpringBoot-Scan.py -uf url.txt
```  
  
    这里的url.txt在  
项目文件夹中存在，打开url.txt可进行手动修改，然后放入项目中进行扫描。扫描结束后，会把成功的结果导出为同目录下的 **output.txt**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQZsbp87ibn0rIFeqrhsNv7fqGSuEQn9KWKk794IibO6QxlK0VBa8gPd7A/640?wx_fmt=png&from=appmsg "")  
  
  
3、  
对单一URL进行漏洞利用  
  
   
   用于  
检测SpringBoot历史高危漏洞。我们在终端中输入：  
```
python3 SpringBoot-Scan.py -v # example.com
```  
  
    这里以一个本地靶场地址作为例子，进行对此URL的漏洞扫描，我们可以在目前已有的漏洞库中选择要检测的漏洞，项目会自动检测此网站是否存在此类漏洞并告知。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQnVzZDB1x1zrqPiaJz7iagWEcvf9hAiajVb5zF4l7LZUW9lxw96mZpAa5w/640?wx_fmt=png&from=appmsg "")  
  
  
4、  
读取目标TXT进行批量漏洞扫描  
  
    同样，漏洞扫描也支持批量扫描。  
```
python3 SpringBoot-Scan.py -vf url.txt
```  
  
      
扫描结束后将导出成功的内容至 vulout.txt 内。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQc667JpjdN1jMvwuxibL0cruqkNhH56MuDZibx6gQXY5goCjB6Pvkb1FQ/640?wx_fmt=png&from=appmsg "")  
  
  
5、  
扫描并下载SpringBoot敏感文件  
  
      
用于  
发现并下载SpringBoot应用的敏感文件。我们在终端中输入：  
```
python3 SpringBoot-Scan.py -d # example.com
```  
  
    这里依旧时使用一个本地靶场作为例子，我们看到在输入靶场地址后，项目扫描到了靶场中的一个漏洞并进行了下载。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQJ6VlqMgMfmIicGa8Wl76MnaMuM98LPHhEFE6JVaDrtTZPNQcgBgKAibw/640?wx_fmt=png&from=appmsg "")  
  
  
6、  
读取目标TXT进行批量敏感文件扫描  
  
    同样，敏感文件也可批量扫描。  
```
python3 SpringBoot-Scan.py -df url.txt
```  
  
     
扫描结束后将导出成功的内容至 dumpout.txt  
 内。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugH0ILMiaIs9WXBiaHTAB1iaPQ6U3Ne9TUHFmaJvaY0r64JgteLyibOgRvwJs0bPujffzVYiaz9s1VTZjA/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eRUtCzBCFbaMYy1c7utlweibCFXWsicmm9ebyvInBtdsD0QRlUDTdLib1g/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
