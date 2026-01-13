#  漏洞预警 | 天锐绿盾审批系统fastjson反序列化漏洞  
浅安  浅安安全   2026-01-13 00:02  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
天锐绿盾审批系统是一款专注于企业数据安全与合规管理的智能审批平台，深度融合文档加密、权限管控与流程自动化，为企业提供从文件创建、流转到归档的全生命周期安全管控。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SVSDYjhzHn2I3VQBVaiaQtxflEcrHnBDozkxg0W0HemglOhvaNDof6QUIBh5ib0cV6WDAkgumhTdTSg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=0 "")  
  
**0x03 漏洞详情**  
  
漏洞类型：  
Fastjson反序列化  
  
**影响：**  
执行任意代码****  
  
**简述：**  
天锐绿盾审批系统的  
/trwfe/login.jsp/.%2e  
/ext/mail/update/{mailId}  
接口存在Fastjson反序列化漏洞，  
未经授权的  
攻击者可以通过构造恶意的JSON数据包，利用Fastjson库在处理数据时存在的反序列化缺陷，在服务器端执行任意代码。  
  
**0x04 影响版本**  
- 天锐绿盾审批系统 V3.53.240913  
  
- 天锐绿盾审批系统 V7.05.240904  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.tipray.com/  
  
  
  
