#  漏洞复现 | 深信服运维安全管理系统 change_net 远程命令执行漏洞  
 实战安全研究   2026-03-03 01:01  
  
**免责声明**  
<table><tbody><tr style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><td valign="top" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;visibility: visible;"><span style="color: rgb(255, 0, 0);letter-spacing: 0.544px;-webkit-tap-highlight-color: rgba(0, 0, 0, 0);outline: 0px;visibility: visible;font-size: 14px;"><span leaf="">本文仅用于技术学习和安全研究，请勿使用本文所提供的内容及相关技术从事非法活动，由于传播和利用此文所提供的内容或工具而造成任何直接或间接的损失后果，均由使用者本人承担，所产生一切不良后果与文章作者及本账号无关。如内容有争议或侵权，请私信我们！我们会立即删除并致歉。谢谢！</span></span></td></tr></tbody></table>  
1  
  
**漏洞描述**  
  
  
  
深信服运维安全管理系统 change_net 接口存在远程命令执行漏洞。攻击者可通过构造恶意的请求，利用该漏洞在目标服务器上执行任意命令，从而可能导致服务器被完全控制、敏感数据泄露等严重后果。影响范围包括所有运行存在该漏洞版本的深信服运维安全管理系统的服务器。  
  
2  
  
**影响版本**  
  
  
  
深信服运维安全管理系统  
  
3  
  
**fofa语法**  
  
  
  
fofa语法  
```
body="/fort/login" || header="FORTSESSIONID"
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/zBdps5HcBF0jh7ibfMcx8rBxdSicicjXy6dBmxxgw2LxK6FNvV8T8DjVmGRwUzREfROXmcdsjvu7RlibDlavJicHnow/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
  
4  
  
**漏洞复现**  
  
  
  
延迟4秒  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yIciaKAicYtopntdPiaN2e3w6MGRDzkv0GoOsX08FibVGavmmzCJ95ibbokFGTFFnIvtabTAiaMfbjqbAAel5PoIyicULiaiapZ5O8Ib0JDcznU61e5k/640?wx_fmt=png&from=appmsg "")  
  
反弹shell  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yIciaKAicYtopRV67takYIR1necON3wM5BEHYJ8L5Ayzibg9e3NtDt1eLGwkRc0O7QiazV13H1qbkREuR6n1zrnf5ibqjNRUsp54OWUsgPq1wozs/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/yIciaKAicYtorwYtWsueFM0jwGpxstUXl5LDibbeXib3PLs1LRFOn4lCM8gmGw8eA2ldA63tjajW9Npiavn5MIMiaa8Jkr5RjL6Zia7L7mFzNLRZZQ/640?wx_fmt=png&from=appmsg "")  
  
  
5  
  
**检测POC**  
  
  
  
nuclei  
  
![](https://mmbiz.qpic.cn/mmbiz_png/yIciaKAicYtoqutXgUAeuFuJiclzKS49jxib2jWYONicjjCa3zSVRHU5kbXrd9SFVcUu4EiccMBHUB7KWdJ8fxCfSeEPJHW6tlIZEN1OHyOQ5R4RE/640?wx_fmt=png&from=appmsg "")  
  
afrog  
  
![](https://mmbiz.qpic.cn/mmbiz_png/yIciaKAicYtop6sbgE1Mibp4ceWDd6lqX5X5cm8iaVGaPicJwxSiaHABibd8yx0usc6Agdtpw1VcoxTKzZAlz0d2ic5Ce9kAfNCNiaVJsQoxPm2YuBZg/640?wx_fmt=png&from=appmsg "")  
  
  
6  
  
**漏洞修复**  
  
  
1、建议联系厂商打补丁或升级版本。  
  
2、增加Web应用防火墙防护。  
  
3、关闭互联网暴露面或接口设置访问权限。  
  
7  
  
**内部圈子**  
  
  
**现在已更新POC数量 2000+（中危以上）**  
  
  
🔥 **1day/Nday 漏洞实战圈上线**  
 🔥  
  
还在到处找公开漏洞 POC？  
  
这里专注整合全网1day/Nday漏洞POC和复现，一站式解决你的痛点！  
  
🔍   
圈子福利  
  
✅ 整合全网 1day/Nday 漏洞POC，附带复现步骤，新手也能快速上手  
  
✅ 每周更新 10-15 个POC测试脚本，经过实测验证，到手就能用  
  
✅ 完美适配 Nuclei/Afrog 扫描工具，脚本无需额外修改，即拿即用  
  
✅ 重磅福利：免登录免费 FOFA 查询，无需账号也能高效资产测绘  
  
✅ 专属权益：提供指纹识别库，指纹库持续更新  
  
💡   
适合对象  
  
渗透测试🔹攻防演练🔹安全运维🔹企业自查  
🔹SRC漏洞挖掘  
  
⚠️   
重要提醒  
  
仅限授权范围内的合法安全测试，严禁用于未授权攻击行为！  
  
本服务为虚拟资源服务，一经购买概不退款，请按需谨慎购买！  
  
现在加入圈子价格是59.9元（  
交个朋友啦  
），后面将调整涨价啦。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/yIciaKAicYtoo5Ngj2YTMj32tr0ZzDauJxcJSLXYLPjic2bHGQ28hJBswkjerTCs8YjYNsyf2qsRjkOW5ygT9lib8DElib7pkqSN1yvZTaG2RfcY/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
