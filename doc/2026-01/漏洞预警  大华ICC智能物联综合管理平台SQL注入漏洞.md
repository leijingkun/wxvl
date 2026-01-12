#  漏洞预警 | 大华ICC智能物联综合管理平台SQL注入漏洞  
浅安  浅安安全   2026-01-12 00:01  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
大华ICC智能物联综合管理平台是一个基于智能物联的综合业务管理平台软件，具备强大的后台服务能力。该平台配套了B/S管理员端、C/S客户端、移动APP终端、小程序等不同应用端，以满足用户各种使用需求。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SXb5GRW8HuWBMbuGelOHiao3f0h0PGhLIw93EUKg03C4TNI6kdmhBJNvoWmrsypzyiaXQPObDibsLegA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=0 "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
****  
  
**简述：**  
大华ICC智能物联综合管理平台的/evo-apigw/evo-arsm/1.0.0/ars/list接口存在SQL注入漏洞，未经身份验证的攻击者可以通过该漏洞执行任意SQL语句，从而获取数据库敏感信息。  
  
**0x04 影响版本**  
- 大华ICC智能物联综合管理平台  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.dahuatech.com/  
  
  
  
