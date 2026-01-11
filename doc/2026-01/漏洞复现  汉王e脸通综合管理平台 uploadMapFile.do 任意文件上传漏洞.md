#  漏洞复现 | 汉王e脸通综合管理平台 uploadMapFile.do 任意文件上传漏洞  
 实战安全研究   2026-01-11 02:00  
  
**免责声明**  
<table><tbody><tr style="-webkit-tap-highlight-color: transparent;outline: 0px;visibility: visible;"><td valign="top" style="-webkit-tap-highlight-color: transparent;outline: 0px;word-break: break-all;hyphens: auto;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;letter-spacing: 0.544px;color: rgb(255, 0, 0);font-size: 14px;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(62, 62, 62);letter-spacing: 0.544px;orphans: 4;text-align: left;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(255, 0, 0);letter-spacing: 0.544px;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);outline: 0px;visibility: visible;">本文仅用于技术学习和安全研究，请勿使用本文所提供的内容及相关技术从事非法活动，由于传播和利用此文所提供的内容或工具而造成任何直接或间接的</span><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;letter-spacing: 0.544px;visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(62, 62, 62);letter-spacing: 0.544px;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;outline: 0px;color: rgb(255, 0, 0);letter-spacing: 0.544px;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);outline: 0px;visibility: visible;">损失</span></span></strong></strong><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);outline: 0px;visibility: visible;">后果，均由使用者本人承担，所产生一切不良后果与文章作者及本账号无关。如内容有争议或侵权，请私信我们！我们会立即删除并致歉。谢谢！</span></span></strong></strong></td></tr></tbody></table>  
1  
  
**漏洞描述**  
  
  
  
汉王e脸通综合管理平台的 uploadMapFile.do 接口存在任意文件上传漏洞。攻击者可在无需认证的情况下，通过向该接口上传恶意文件，实现任意文件上传，进而可能导致远程代码执行或服务器被控制，严重威胁系统安全。  
  
2  
  
**影响版本**  
  
  
  
汉王e脸通综合管理平台  
  
3  
  
**fofa语法**  
  
  
  
fofa语法  
```
icon_hash="1380907357"
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/zBdps5HcBF27VmLMoC1AnXER8GwASUol1unMMe0ISniaRXiaRKZiaVGcHnicrkpQCHMSaM6dNOAtpUXX0j2nT6Eicnw/640?wx_fmt=png&from=appmsg "")  
  
  
4  
  
**漏洞复现**  
  
  
  
上传文件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/zBdps5HcBF27VmLMoC1AnXER8GwASUoltoqA4c2TCzbyShSRCftcauh6jEA3kqicgIfobtCNX48BOBwLsWK2V1w/640?wx_fmt=png&from=appmsg "")  
  
访问文件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/zBdps5HcBF27VmLMoC1AnXER8GwASUolUYV7qEibtmRb7YCYFeWic4MRicPNky5do0WMwichQhLPW0ezhuicJLjlYmQ/640?wx_fmt=png&from=appmsg "")  
  
  
5  
  
**检测POC**  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/zBdps5HcBF27VmLMoC1AnXER8GwASUolxjKtPmm4cDBcXHC3nOk3JcqdWIdNskuRcltCznic8Yd8T7d3QMvIKpA/640?wx_fmt=png&from=appmsg "")  
  
  
6  
  
**漏洞修复**  
  
  
1、建议联系厂商打补丁或升级版本。  
  
2、增加Web应用防火墙防护。  
  
3、关闭互联网暴露面或接口设置访问权限。  
  
7  
  
**内部圈子**  
  
  
**现在已更新POC数量 1500+（中危以上）**  
  
  
🔥 **1day/Nday 漏洞实战圈上线**  
 🔥  
  
还在到处找公开漏洞 POC？  
  
这里专注整合全网1day/Nday漏洞复现，一站式解决你的痛点！  
  
🔍   
圈子福利  
  
✅ 整合全网 1day/Nday 漏洞POC，附带复现步骤，新手也能快速上手  
  
✅ 每周更新 10-15 个POC测试脚本，经过实测验证，到手就能用  
  
✅ 完美适配 Nuclei 主流扫描工具，脚本无需额外修改，即拿即用  
  
✅ 重磅福利：免登录免费 FOFA 查询，无需账号也能高效资产测绘  
  
✅ 专属权益：提供指纹识别库，指纹库持续更新  
  
💡   
适合对象  
  
渗透测试🔹攻防演练🔹安全运维🔹企业自查  
  
👉 不管你是企业安全自查的运维人员，或者是参加攻防演练的战队成员（红队/蓝队），还是做渗透测试的工程师等职业，这里的资源都能适合你  
  
⚠️   
重要提醒  
  
仅限授权范围内的合法安全测试，严禁用于未授权攻击行为！  
  
本服务为虚拟资源服务，一经购买概不退款，请按需谨慎购买！  
  
现在加入圈子价格是59.9元（  
交个朋友啦  
），后面将调整涨价啦。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/zBdps5HcBF030s43NvfRl8pK5qh693CUBiaFASCp9aicOY7sgFH3XSX26Bicp0kWq6nzfrlTyOSVOxoPslCwllOwA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
