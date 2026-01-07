#  漏洞预警 | LangChain序列化注入漏洞  
浅安  浅安安全   2026-01-07 00:00  
  
**0x00 漏洞编号**  
- # CVE-2025-68664  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
LangChain是一个用于开发由大语言模型驱动的应用程序的框架，它提供了一套工具、组件和接口，可以简化创建和管理与语言模型的交互、将多个组件链接在一起、并集成额外的资源的过程。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SVjR0a11MK0DTHc9DqsMz388iclicFr4u1k0pbDFswIDkoqNJLicdtTn5TVcgRTYfakX6qjxkm0UBuHA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-68664**  
  
**漏洞类型：**  
序列化注入  
  
**影响：**  
获取敏感信息  
  
**简述：**  
L  
angChain存在序列化注入漏洞，由于其dumps()与dumpd()函数在处理自由字典时未正确转义包含“lc”关键字的用户可控数据，导致其在load()或loads()反序列化过程中被误识别为合法的LangChain对象结构。攻击者可通过在LLM响应、metadata、additional_kwargs等可控字段中注入特制序列化结构，实现敏感环境变量泄露，或在受信命名空间内实例化具有副作用的类。  
  
**0x04 影响版本**  
- 1.0.0 <= langchain < 1.2.5  
  
- langchain < 0.3.81  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://github.com/langchain-ai/langchain  
  
  
  
