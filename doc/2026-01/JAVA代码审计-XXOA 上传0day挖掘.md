#  JAVA代码审计-XXOA 上传0day挖掘  
原创 白昼  白昼安全团队   2026-01-06 06:48  
  
### 系统技术栈  
```
├── Web容器: Apache Tomcat├── Web框架: Struts2 + Spring MVC├── 持久层: Hibernate├── 前端: JSP + jQuery└── 文件上传: Commons FileUpload + 自定义实现
```  
### 请求处理流程  
```
HTTP请求    ↓过滤器链（Filter Chain）    ├── SpringSecurityFilterChain    ├── CharacterEncodingFilter（编码过滤）    ├── LoginUserInfoFilter（认证过滤）    ├── OpenSessionInViewFilter（Hibernate）    └── StrutsPrepareAndExecuteFilter（Struts2）    ↓目标Servlet或Action    ↓业务处理    ↓响应返回
```  
### 过滤器配置分析  
  
LoginUserInfoFilter拦截所有Action、jsp、html后缀，且存在/Servlet/*  
路由，所有路径不经过认证过滤器  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7nHG19BXpuvgIF9bG23S78m1V1ib2LciaRFcibyoiaEUXMicLNhhHqdBPSQg/640?wx_fmt=png&from=appmsg "null")  
  
  
分析LoginUserInfoFilter doFilter()  
  
判断isDefaultPass()和isUserInfoPass()白名单或者存在用户信息放行  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7ibDkwonsYF0A2SibpwVh8nftpicKo6ayLpe7GtCKZdLvRyJoKObiakG2Bw/640?wx_fmt=png&from=appmsg "")  
  
跟入isDefaultPass()，存在passUrlList白名单列表  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP710xII8DNAjDiauIgg4qI9Lx5npzeIlTVK2nQ6yj07k2AKT0SzmV1VMg/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7pKFY7UIr0azG10XZE0YU6AxDTxxxs9HKlydrTlNRZaWH06T20v2GNw/640?wx_fmt=png&from=appmsg "null")  
  
  
回到上方isDefaultPass() 遍历所有白名单条目，使用contains进行子串匹配,经过LoginUserInfoFilter的路由存在uplod即可绕过过滤器。  
### 第一处漏洞  
  
扫描发现XXAction.java的upload方法，存在很明显的任意文件上传漏洞  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7yDPmyZ2Wo8o0u4zZWjts73roqjP7cWiaT6OgFTRxyWWAwfpAicAkCibSA/640?wx_fmt=png&from=appmsg "null")  
  
### 路由分析  
  
struts.xml 里启用 convention-default  
由 **Convention 自动扫描类 + 注解/命名规则**  
生成  
  
Convention 规则：  
- • 类名：HeImplementAction  
  
- • 去掉后缀 Action  
 → HeImplement  
  
- • 再做驼峰转连字符小写  
HeImplement  
 → he-implement  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7hD3TC52gUBkXoelsEtG7McnmQrDDIkGXTWicrOFPXvibMXlYJcgSFlsg/640?wx_fmt=png&from=appmsg "null")  
  
路由规则为/{contextPath}/Namespace/Convention 约定映射!方法名.action  
### 第二处漏洞分析  
  
发现web.xml中有处上传的Servlet的直接映射，前面分析，所有/servlet/*  
路由，不经过认证过滤器，相当于未授权，跳转分析对应的Servlet  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7slibU6rgGt4eYR00ZeuSfQRMkricaZCu1xIiawyUYSVacKlib29szgwp1Q/640?wx_fmt=png&from=appmsg "null")  
  
  
processUpload()接收mtpId参数，存在分支，有mtpId →processUpload()、无mtpId → doEbenUpload()。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7aPoFP5ibruxXqsbc0VHCkthJ0oYgSiaetjgcxXwiaUjzty1z8IgekXqSw/640?wx_fmt=png&from=appmsg "null")  
  
#### mtpId 分支  
  
mtpId=（空字符串）也会进入该分支（并且被替换成固定默认ID），只有完全不传mtpId参数才会走doEbenUpload。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7R9cZJdt2OxxHzfliauq1mvPGlWAvBNd8jib5NbL7nVhicicfKdM9MJ5ib4g/640?wx_fmt=png&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7EAb5DuWksQaQGDia4iaSgBA0Tcm0hibK99bazwKANqxUynXibud38ZoETQ/640?wx_fmt=png&from=appmsg "null")  
  
  
从数据库读取 TblMetadataTackProperty  
（元数据附件策略），通过mtpFileUploadType（local/ftp/local2ftp）保存到  
mtpFileSavePath最终通过 property2dto() 把策略转换成  
DefaultUploadDto  
  
感觉利用不了，跳过  
#### 无 mtpId 分支  
  
请求不带 mtpId  
 参数，就会进入doEbenUpload()  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7TpfHnkTr9WzbPrSlOXQCbf8ktghmYb8RAXDOB3VW27eU844V1aa6VQ/640?wx_fmt=png&from=appmsg "null")  
  
接收tempAppId和filename参数  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP75ic0XKibHM3ylNFFXHygRNHdAZIbVooEL2jw2j3ycDr5yOOp7GevSQEA/640?wx_fmt=png&from=appmsg "null")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7Dnk2SpdOMWa8XInQT75f09wiaowbJ2lKCNSjaNPlPXOba36PFVGibuDg/640?wx_fmt=png&from=appmsg "null")  
  
上传文件写入固定目录：`/xxFile，对上传文件名查版本表 + 重命名pdf规则  
  
粗看可能觉得重命名后缀就放弃了，后来细看：  
  
以（gz-  
）前缀作为信任依据，代码只在 “非gz 且无版本” 时强制改 .pdf  
，但“保持原名”分支不会限制扩展名  
```
if ((versions == null || versions.size() <= 0) && !tempName.startsWith("gz-")) {    filename = tempName.replace(".pdf","").replace(".PDF","") + "_0.pdf";} else {    // filename 保持为 tempName（即原始文件名）}File file = new File(path + "/" + tempName.replace(".doc", "").replace(".pdf", "").replaceAll("_0_0", "_0") + ".txt");
```  
  
后续无处理，直接上传gz-*.jsp即可落地shell  
  
![](https://mmbiz.qpic.cn/mmbiz_png/eawzN3dKTiauVfGGwAmsyVSsUqNdVcmP7UQo9wwib0exEkqlyzE0YdrmWP7Siahf5MBxXokVgjzD8qsLlrJhBAC6w/640?wx_fmt=png&from=appmsg "null")  
  
