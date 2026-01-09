#  【0day预警】ComfyUI-Manager CRLF注入导致远程代码执行漏洞  
 长亭安全应急响应中心   2026-01-09 07:34  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/FOh11C4BDicSxMzAMshPH2YKAzHBToywQ5NNsribfzrsGJ40fCKSib4TiaM83FpHEl365E1fTiacCsblm8vdrtglczQ/640?wx_fmt=png&from=appmsg "")  
  
  
ComfyUI 是一款流行的基于节点的 Stable Diffusion 图形用户界面，广泛应用于 AI 图像生成工作流的构建和执行。ComfyUI-Manager 是 ComfyUI 的扩展管理器插件，用于简化自定义节点、模型和依赖项的安装管理。  
  
  
2026年1月，长亭安全应急响应中心监测到 ComfyUI-Manager 官方在修复CVE-2025-67303漏洞之后仍然存在CRLF注入漏洞可导致配置篡改，未经身份验证的攻击者可利用该漏洞注入不安全的配置项，配合 Git URL 安装功能实现远程代码执行，利用难度较低，建议受影响的用户尽快修复。  
  
  
**漏洞描述**  
  
   
Description  
   
  
  
  
**0****1**  
  
**漏洞成因**  
  
由于 ComfyUI-Manager 默认暴露了可通过Web API修改配置项的接口，并且在处理用户输入时未对\r\n等特殊字符进行过滤，导致存在CRLF注入漏洞。攻击者可通过换行符注入恶意配置项，将security_level设置为weak，从而结合Git URL安装功能实现远程代码执行，最终控制服务器。  
  
  
## 漏洞影响  
  
攻击者可在服务器上执行任意代码，可能导致服务器被完全控制、数据泄露或业务系统沦陷。  
  
  
**处置优先级：高**  
  
漏洞类型：  
远程代码执行  
  
**漏洞危害等级：**  
高  
  
**触发方式：**  
网络远程  
  
**权限认证要求：**  
无需权限  
  
**系统配置要求：**  
默认配置  
  
**用户交互要求：**  
无需用户交互  
  
**利用成熟度：**  
POC/EXP 未公开  
  
**修复复杂度：**  
低，  
官方已提供修复方案  
  
  
  
  
  
**影响版本**  
  
   
Affects  
   
  
  
  
**02**  
```
```  
  
**解决方案**  
  
   
Solution  
   
  
  
  
**03**  
  
##   
  
## 升级修复方案  
  
  
升级 ComfyUI-Manager 至 3.39.2 或更高版本  
  
## 临时缓解方案避免使用 --listen 0.0.0.0等允许外部连接的参数启动 ComfyUI漏洞复现Reproduction                                       04产品支持 Support 05云图：默认支持该产品的指纹识别，同时支持该漏洞的PoC检测洞鉴：默认支持该产品的指纹识别，预计2026.01.09支持该漏洞的自定义PoC无锋：默认支持该产品的指纹识别，预计2026.01.09支持该漏洞的PoC检测全悉：规则库版本大于 20260108150417 默认支持检测雷池：预计 2026.01.09 发布自定义规则支持检测时间线 Timeline 062026年1月9日 长亭安全应急响应中心发布通告参考资料：[1].https://github.com/Comfy-Org/ComfyUI-Manager/commit/f4fa394e0f03b013f1068c96cff168ad10bd0410  
  
  
**长亭应急响应服务**  
  
  
  
  
全力进行产品升级  
  
及时将风险提示预案发送给客户  
  
检测业务是否受到此次漏洞影响  
  
请联系长亭应急服务团队  
  
7*24小时，守护您的安全  
  
  
第一时间找到我们：  
  
邮箱：support@chaitin.com  
  
