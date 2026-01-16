#  【已复现】全程云OA QCHMS.asmx SQL Injection  
原创 蟑螂恶霸
                    蟑螂恶霸  momo安全   2026-01-16 06:58  
  
## 免责声明  
>   
> 本文仅用于技术学习和讨论。请勿使用本文所提供的内容及相关技术从事非法活动，由于传播、利用此文所提供的内容或工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果均与文章作者及本账号无关，本次测试仅供学习使用。如有内容争议或侵权，请及时私信我们！我们会立即删除并致歉。谢谢！  
  
### 一、漏洞描述  
  
全程云OA QCHMS.asmx 接口存在SQL注入漏洞。攻击者可以通过构造恶意的SOAP请求，在参数中执行任意SQL命令，从而获取数据库敏感信息。  
### 二、影响版本  
  
全程云OA  
### 三、Fofa语法  
  
body="images/yipeoplehover.png" || body="hfShowQRCode" || body="全程软件iOA" || body="Common/Charts/LoginQRCode.ashx"  
### 四、漏洞复现  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtksRIT4GU4uNvfHGE1DSbnplnM933faAeqU6VDTwrEPjX3iaq6qXtmp4XdWvnc0ZEUvxeCmyVCYj8A/640?wx_fmt=png&from=appmsg "")  
  
nuclei批量复现![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtksRIT4GU4uNvfHGE1DSbnpYmlbPLNQVJGlxDTRAYZCTy7uq3ZbJiaBicEC2aWSOMB3P3PzP6K3GefA/640?wx_fmt=png&from=appmsg "")  
  
### 五、漏洞修复  
  
目前官方已发布漏洞修复版本，建议用户升级到安全版本：https://www.eqccd.com/  
### 六、漏洞脚本下载  
  
后台回复**20260116**  
 获取nuclei  poc  
### 七、技术服务推广  
  
✅Cn*d🀄️高提交到个人账户上，CNNVD中高 漏洞及一般、重要情报 支撑单位均可协助获得  
  
✅CISP、PTE/PTS、CISP-DSG、IRE/IRS、NISP一二级、PMP、CCSK、CISSP/CCSP、CISAW各种类、CCSC、itil、软考中高级、CDSP各种类、CISA，oscp等等全网最低价。ISO27001、ITss服务项目经理报名等下证即可，可对公，可开专普票  
  
✅需要私聊联系~  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtksRIT4GU4uNvfHGE1DSbnpvIg3cFdX6Qe78p4HQhgtMYibpBC4hoVvUDZYu72wiaibsIpmWHnFEPBibQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
