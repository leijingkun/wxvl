#  D-Link老款DSL路由器被曝在野利用漏洞  
 SecHub网络安全社区   2026-01-08 01:41  
  
****  
****  
****  
**点击蓝字 关注我们**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/8icWLyUKibZZrPdaxnm18Zscp6Xcu0OiaMwuh8LP87lPQLxMwiceAsv3TurmE7zZOulOhMELnQ2OulwFIJkbmB3bRg/640?wx_fmt=png "")  
  
  
**免责声明**  
  
本文发布的工具和脚本，仅用作测试和学习研究，禁止用于商业用途，不能保证其合法性，准确性，完整性和有效性，请根据情况自行判断。  
  
如果任何单位或个人认为该项目的脚本可能涉嫌侵犯其权利，则应及时通知并提供身份证明，所有权证明，我们将在收到认证文件后删除相关内容。  
  
文中所涉及的技术、思路及工具等相关知识仅供安全为目的的学习使用，任何人不得将其应用于非法用途及盈利等目的，间接使用文章中的任何工具、思路及技术，我方对于由此引起的法律后果概不负责。  
## 🌟简介                            
  
     
  
![](https://mmbiz.qpic.cn/mmbiz_png/8icWLyUKibZZqDbHg2jecH9pPiamnyGq8zVvQWLYxntHkXeZ4BBIIIJ0fZwxaG29LEUW6dLicZLcZPfFOEL54kVVzA/640?wx_fmt=png&from=appmsg "")  
  
   
  
攻击者正在利用最近发现的一个命令注入漏洞，该漏洞影响多年前已停止支持的多个 D-Link DSL 网关路由器。  
  
该漏洞现在被跟踪为 CVE-2026-0625，由于 CGI 库中输入清理不当，影响了 dnscfg.cgi 端点。未经身份验证的攻击者可以利用此漏洞通过 DNS 配置参数执行远程命令。  
  
漏洞情报公司 VulnCheck 于 12 月 15 日向 D-Link 报告了该问题，此前 The Shadowserver Foundation 在其蜜罐之一上观察到了一次命令注入利用尝试。  
  
VulnCheck 告知 BleepingComputer，Shadowserver 捕获的技术似乎没有被公开记录。  
  
"未经身份验证的远程攻击者可以注入并执行任意 shell 命令，导致远程代码执行，" VulnCheck 在安全公告中说。  
  
影响版本  
# D-Link 确认以下设备型号和固件版本受 CVE-2026-0625 影响：DSL-526B ≤ 2.01DSL-2640B ≤ 1.07DSL-2740R < 1.17DSL-2780B ≤ 1.01.14  
  
# 详情上述设备自 2020 年起已达到生命周期结束（EoL），将不会收到针对 CVE-2026-0625 的固件更新。因此，厂商强烈建议退役并更换受影响的设备为支持的型号。D-Link 正在通过分析各种固件发布版本来尝试确定是否有其他产品受到影响。“由于固件实现和产品代数的差异，D-Link 和 VulnCheck 都面临着精确识别所有受影响型号的复杂性，”D-Link 解释道 。“当前分析显示，除了直接固件检查之外，没有可靠的型号检测方法。因此，D-Link 正在将固件构建验证作为调查的一部分，涵盖传统和支持的平台，”供应商表示。目前尚不清楚是谁在利用该漏洞以及针对什么目标。然而，VulnCheck 表示，大多数消费者路由器设置仅允许局域网访问管理通用网关接口（CGI）端点，例如 dnscfg.cgi。利用 CVE-2026-0625 意味着进行基于浏览器的攻击或目标设备配置为远程管理。使用已停产（EoL）路由器和网络设备的用户应将其更换为供应商积极支持的型号，或部署在非关键网络中，最好是在分段网络中使用最新可用的固件版本和限制性安全设置。D-Link 警告用户，已停产的设备不会收到固件更新、安全补丁或任何维护。  
  
  
  
  
欢迎关注SecHub网络安全社区，SecHub网络安全社区目前邀请式注册，邀请码获取见公众号菜单【邀请码】  
  
**#**  
  
  
**企业简介**  
  
  
**赛克艾威 - 网络安全解决方案提供商**  
  
****  
       北京赛克艾威科技有限公司（简称：赛克艾威），成立于2016年9月，提供全面的安全解决方案和专业的技术服务，帮助客户保护数字资产和网络环境的安全。  
  
  
安全评估|渗透测试|漏洞扫描|安全巡检  
  
代码审计|钓鱼演练|应急响应|安全运维  
  
重大时刻安保|企业安全培训  
  
![](https://mmbiz.qpic.cn/mmbiz_png/8icWLyUKibZZrPdaxnm18Zscp6Xcu0OiaMwuh8LP87lPQLxMwiceAsv3TurmE7zZOulOhMELnQ2OulwFIJkbmB3bRg/640?wx_fmt=png "")  
  
  
**联系方式**  
  
电话｜010-86460828   
  
官网｜https://sechub.com.cn  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/MVPvEL7Qg0FW5uwU0BZtn2lmMrLPwpibCeCVbtBFDRkbFb7n7ibhPRxg20spUo9mUIiakmRYABB88Idl81IpGuXfw/640?wx_fmt=gif "")  
  
**关注我们**  
  
![](https://mmbiz.qpic.cn/mmbiz_png/SUZ43ICubr4mWJcUARDKYbQooQjbjbmqZTerAIXqDX9CaVxXbB7pyWwnMRklrCJias9r59PhnJAxZ4e3gYjyqVQ/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/SUZ43ICubr4mWJcUARDKYbQooQjbjbmqZTerAIXqDX9CaVxXbB7pyWwnMRklrCJias9r59PhnJAxZ4e3gYjyqVQ/640?wx_fmt=png "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/8icWLyUKibZZrPdaxnm18Zscp6Xcu0OiaMwyhlWCYDVqK38BA5dbjKkH7icWmAew7SYRA7ao1bFibialrMvmQ9ib0TBvw/640?wx_fmt=jpeg "")  
  
  
**公众号：**  
sechub安全  
  
**哔哩号：**  
SecHub官方账号  
  
  
