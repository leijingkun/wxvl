#  【已复现】深信服运维安全管理系统 protocol_session 远程代码执行  
 momo安全   2026-01-13 03:56  
  
## 免责声明  
>   
> 本文仅用于技术学习和讨论。请勿使用本文所提供的内容及相关技术从事非法活动，由于传播、利用此文所提供的内容或工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果均与文章作者及本账号无关，本次测试仅供学习使用。如有内容争议或侵权，请及时私信我们！我们会立即删除并致歉。谢谢！  
  
### 一、漏洞描述  
  
深信服运维安全管理系统（OSM）是深信服科技推出的一款企业级运维安全管理平台，主要用于提供堡垒主机功能。深信服运维安全管理系统中存在的远程命令执行漏洞（CVE-2025-12916）。该漏洞源于系统中portal_login、/protocol/session等接口对请求参数校验不严格，存在命令注入缺陷。攻击者可以通过构造恶意的JSON请求，在loginUrl参数中注入系统命令，利用反引号（`）执行任意系统命令。成功利用后可在目标服务器上执行任意系统命令，最终获取服务器的完全控制权限。  
### 二、影响版本  
  
深信服运维安全管理系统（OSM）< 3.0.12 20241106  
### 三、Fofa语法  
>   
> (body="/fort/login" && header="FORTSESSIONID" ) || body="fort/trust/version/sxf/trust/images/login/QRCode.jpg" || (body="/fort/login" && body="https://"+window.location.host")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtnw5UnsZfjOC1vXcG1wU4IxvM0TbF7Axgz0ia8h6J18HzCQd99ezC2fmK0DOsfuQY4vBDhbhXnPB5g/640?wx_fmt=png&from=appmsg "")  
### 四、漏洞复现  
  
nuclei -t xxx.yaml -l urls.txt  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtnw5UnsZfjOC1vXcG1wU4IxQQCRFVEpgLEePqqeMLL4QyUmmGvZ3aks2s9Gle7Dmb45gjJhGgMwTg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtnw5UnsZfjOC1vXcG1wU4IxPIWzDkFsPFXKmBIAUrJibibGBehibEpEEpDxIfpY4kTY4I49QqicdmBnJA/640?wx_fmt=png&from=appmsg "")  
### 五、漏洞修复  
  
立即升级到OSM 3.0.12 20241106及以上版本，建议直接升级到最新版本。  
  
公告链接: https://www.sangfor.com.cn/sec_center/details/5802e177c6a84e1eb7389809936a13fc  
  
官方已发布修复版本,请及时联系并获取补丁  
### 六、漏洞脚本下载  
  
后台回复**260113**  
 获取poc  
### 七、技术服务推广  
  
✅ Cn*d🀄️高提交到个人账户上，CNNVD中高 漏洞及一般、重要情报 支撑单位均可协助获得  
  
✅ CISP、PTE/PTS、CISP-DSG、IRE/IRS、NISP一二级、PMP、CCSK、CISSP/CCSP、CISAW各种类、CCSC、itil、软考中高级、CDSP各种类、CISA，oscp等等全网最低价。ISO27001、ITss服务项目经理报名等下证即可，可对公，可开专普票  
  
✅ 需要私聊联系~  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KcvoibVKmdtnw5UnsZfjOC1vXcG1wU4IxXVTO1KnhfaiazQDVjonhodhDgkZfialvutfxucVB7CyZHVmFauCaciaLg/640?wx_fmt=png&from=appmsg "")  
  
  
  
