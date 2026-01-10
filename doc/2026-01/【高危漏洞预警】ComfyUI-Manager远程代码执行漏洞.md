#  【高危漏洞预警】ComfyUI-Manager远程代码执行漏洞  
cexlife  飓风网络安全   2026-01-09 15:26  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibhQpAia4xu00hGrUjKMKp1RIkJmwEkksT1uQCvM9jFFbslctpOj2Do1boKa8wMbNozkb2GxlibvMvapjptHrJenQ/640?wx_fmt=png&from=appmsg "")  
  
漏洞描述:  
  
CоmfуUI-Mаnаɡer默认暴露Wеb API接口用于在线修改配置项,且未对用户输入中的\r\n做过滤,导致攻击者可向配置文件中注入恶意换行内容通过将ѕесuritу_lеvеl等关键配置项篡改为ԝеаk,进一步结合插件自带的Git URL安装功能,可在服务器上执行任意代码   
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibhQpAia4xu00hGrUjKMKp1RIkJmwEkksTnibMYThT4IURw0Eia1Lz5tcLSNFjK3KHQnwpyDSnWc5hcHCJ9WvV58mQ/640?wx_fmt=png&from=appmsg "")  
  
攻击场景:  
  
攻击者通过向ComfyUI-Manager的Web API接口发送恶意请求利用未过滤的\r\n 字符注入伪造配置项（如 security_level）将其篡改为 weak,进而触发插件的 Git URL 自动安装功能实现任意代码执行  
  
影响产品:  
  
ComfyUI-Manager < 3.39.2   
  
修复建议:  
  
官方已发布安全补丁,请及时更新至最新版本:  
  
CоmfуUI-Mаnаɡеr >= 3.39.2  
  
同时升级CоmfуＵI >= v0.3.76,以启用系统用户保护API  
  
下载地址:  
  
https://github.com/Comfy-Org/ComfyUI-Manager   
  
修复建议:  
  
建议措施:  
  
立即升级：将 ComfyUI-Manager 升级至 ≥ 3.39.2 版本，并同步升级 ComfyUI 至 ≥ v0.3.76，以启用“系统用户保护API”  
  
网络隔离：若无法立即升级，应通过防火墙策略限制对ComfyUI-Manager Web API 的访问，仅允许可信IP访问  
  
输入过滤：在中间件或反向代理层（如Nginx、Apache）对所有传入请求中的 \r\n 进行过滤或转义防止CRLF注入  
  
禁用危险功能：在未启用安全机制前，建议禁用插件的 Git URL 安装功能，避免自动加载外部代码  
  
日志监控：加强对 Web API 接口的访问日志审计，重点关注对 security_level、config 等敏感参数的异常修改行为  
  
  
  
