#  ComfyUI-Manager 远程代码执行漏洞通告  
RicterZ  腾讯玄武实验室   2026-01-06 08:59  
  
近期腾讯玄武实验室发现可视化 AI 工作流工具 ComfyUI 的官方扩展组件 ComfyUI-Manager 中存在一个高危漏洞（CVE-2025-67303）。利用该漏洞可在无需任何账号的情况下  
远程入侵安装 ComfyUI 的系统。玄武实验室在发现漏洞后向 ComfyUI 官方进行了报告，目前该漏洞已被修复。  
  
  
漏洞详情  
  
ComfyUI 是一款基于节点式工作流的 Stable Diffusion 专业图形界面，它是开源 AI 绘画领域的核心项目之一。ComfyUI-Manager 是 ComfyUI 的官方扩展管理器，负责管理自定义节点、模型和更新的安装。ComfyUI-Manager 的数据与配置  
目录  
在旧版本中未受 ComfyUI 的 Web API 访问控制充分保护。攻击者利用此漏洞可导致在服务器上实现远程代码执行，从而完全控制服务器。该漏洞已在 ComfyUI-Manager   
v  
3.38 版本中通过引入“系统用户保护API”得到修复，所有配置数据被迁移至受保护的  
目录  
下。  
  
  
漏洞发现者  
  
该漏洞由腾讯玄武实验室的  
  
R  
i  
c  
t  
e  
r  
Z  
  
发现并报告  
。  
  
  
风险等级  
  
高风险  
  
  
漏洞风险  
  
远程攻击者利用该漏洞可完全控制  
安装了  
  
ComfyUI  
  
的  
系统  
，  
包括  
盗取  
用户  
数据  
、  
私有  
  
AI  
  
模型  
等  
。  
  
  
影响版本  
  
ComfyUI-Manager 版本 <   
v  
3.38  
  
  
安全版本  
  
ComfyUI-Manager 版本 >=   
v  
3.38  
  
  
修复建议  
  
1.   
立即升级  
  
ComfyUI-Manager  
  
至  
  
v  
3.38  
  
或  
更高  
版本。  
  
a.   
您可以通过  
  
ComfyUI-Manager  
  
自身的更新功能检查并安装，或参考官方文档进行手动升级。  
  
b.   
版本更新之后需要手动进行数据迁移，  
请  
参考  
官方发布的  
《  
用户数据安全迁移指南  
》  
。  
  
2.   
检查并确保  
  
ComfyUI  
  
本身已更新至支持“系统用户保护API  
  
”的版本（例如  
  
v0.3.76  
  
或更高）  
。  
  
3.   
如非必要，避免将  
  
ComfyUI  
  
服务暴露在公共网络。  
  
  
【备注】  
 建议您在升级前做好数据备份工作，避免出现意外  
  
  
漏洞参考  
  
●  
   
《  
ComfyUI-Manager   
v  
3.38 用户数据安全迁移指南  
》  
：https://github.com/Comfy-Org/ComfyUI-Manager/blob/main/docs/en/v3.38-userdata-security-migration.md   
  
●  
   
CVE  
  
漏洞  
通告  
[https://www.cve.org/CVERecord?id=CVE-2025-67303](https://www.cve.org/CVERecord?id=CVE-2025-67303)  
  
  
  
  
  
