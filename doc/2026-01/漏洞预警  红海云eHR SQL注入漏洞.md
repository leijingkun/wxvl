#  漏洞预警 | 红海云eHR SQL注入漏洞  
浅安  浅安安全   2026-01-06 00:00  
  
**0x00 漏洞编号**  
- # 暂无  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
红海云eHR系统是企业进行人力资源数字化管理的工具，它覆盖了组织人事、考勤、薪资、招聘、绩效等人力资源业务全流程，提供一体化解决方案。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SX1ztK6ia3zx6icT54LLodZxZbwLAcVg2kT74RiagiaghhCcZwXcvTFRznHP1ghrUIasNgGDnwAQ9liaTA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=0 "")  
  
**0x03 漏洞详情**  
###   
  
**漏洞类型：**  
SQL注入  
  
**影响：**  
获取敏感信息  
  
**简述：**  
红海云eHR系统的  
/RedseaPlatform/submitStWasAssessDept/StWasAssessDept.mob  
接口存在SQL注入漏，未经身份验证的攻击者可以通过该漏洞获取数据库敏感信息。  
  
**0x04 影响版本**  
- 红海云eHR  
  
**0x05****POC状态**  
- 已公开  
  
**0x06****修复建议**  
  
**目前官方已发布漏洞修复版本，建议用户升级到安全版本****：**  
  
https://www.hr-soft.cn/  
  
  
  
