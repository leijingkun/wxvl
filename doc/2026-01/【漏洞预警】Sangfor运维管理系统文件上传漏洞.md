#  【漏洞预警】Sangfor运维管理系统文件上传漏洞  
by 融云安全-sm  融云攻防实验室   2026-01-13 06:24  
  
**漏洞名称：**  
  
Sangfor 运维管理系统 文件上传漏洞  
  
**组件详情：**  
  
Sangfor 运维管理系统 (OSM) 版本 3.0.8 存在一个严重的任意文件上传漏洞。该漏洞位于 /fort/trust/version/common/common.jsp 端点。该端点的请求未能强制执行身份验证或正确的文件类型验证。远程的、未认证的攻击者可以通过发送精心构造的HTTP POST请求上传恶意文件（例如.jsp web shell）。 一旦上传，文件将存储在网页根目录下（通常在/fort/trust/version/common/下），可以通过网页浏览器直接执行，从而导致以网页服务器的权限（通常是root或tomcat）执行远程命令执行（RCE）。  
  
**影响范围：**  
- 涉及 /fort/trust/version/common/common.jsp 接口的部署实例  
  
- 其他集成该版本上传功能的深信服运维安全管理系统（堡垒机）定制部署  
  
**漏洞类型：**  
  
任意文件上传（Unrestricted Upload of File with Dangerous Type）  
  
**利用条件：**  
  
1、用户认证：无需用户认证（预认证远程利用）  
  
2、前置条件：默认配置下启用版本管理或相关上传接口（common.jsp 暴露）  
  
3、触发方式：远程（通过精心构造的HTTP POST multipart/form-data 请求上传恶意文件，如 JSP Webshell）  
  
**综合评价：**  
  
<综合评定利用难度>：容易，无需授权即可通过单一精心构造的请求上传任意文件（包括 JSP、JSPX 等可执行脚本），上传后直接访问路径实现远程代码执行，已有公开 PoC 和批量扫描工具。  
  
<综合评定威胁等级>：高危，可导致服务器完全失陷、Webshell 植入、敏感运维数据泄露（账号密码、操作日志、资产信息）、进一步内网横向移动或堡垒机接管全网资产。  
  
**官方解决方案：**  
  
临时缓解措施：限制 /fort/trust/version/common/ 目录公网访问、使用 WAF 规则拦截multipart 上传请求中危险文件后缀（如 .jsp、.jspx、.jspf）、禁用不必要的版本管理功能、监控异常文件创建和 Web 访问日志  
  
强烈建议：轮换所有系统凭证、检查服务器是否存在已上传恶意文件，并进行全面安全加固与渗透测试。如有官方补丁发布，立即应用并重启服务。  
  
  
  
**复现情况**  
  
  
融云攻防实验室已复现该漏洞，如下图：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/GWXBjgPE49xuW6yGZdGxsTkcxVzUkcbtyR7lgPGsRlicsKGNZ5IQl2QoAPwt0GFDCJIM1IGI5vedq3p3ocCcDkw/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2 "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/w8NHw6tcQ5wicTzaxopgE26TXk6BicVDCkRV2P7zLEqkMiarB6Gq7gwAfxowKPPDGI0iahiaOkO55LooibEGtWxj9Biag/640?wx_fmt=gif&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=6 "")  
  
**渝融云解决方案-渝融云NTM全流量分析探针**  
  
**已添加监测规则**  
，可实时监控Sangfor运维管理系统文件上传漏洞  
  
  
