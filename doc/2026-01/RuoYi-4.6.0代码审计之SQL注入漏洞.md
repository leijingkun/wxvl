#  RuoYi-4.6.0代码审计之SQL注入漏洞  
安全艺术  安全艺术   2026-01-08 09:08  
  
# 1. 环境搭建RuoYi-4.6.0  
  
https://github.com/yangzongzhuan/RuoYi/releases  
  
修改连接数据库的相关信息  
  
RuoYi-4.6.0/ruoyi-admin/src/main/resources/application-druid.yml  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmMXRNic2TGTSia0rbWf23ia7zDLfjpF87eabjTA7y2AuQMOheZkeA5ib2Zw/640?wx_fmt=png&from=appmsg "")  
  
将⽇志路径需改为相对路径 ./logs    
  
RuoYi-4.6.0/ruoyi-admin/src/main/resources/logback.xml  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmiaRH53Rj3ogsr6hC3oL9sazNABKGKxEZVcagicSDfFwicSsvCg8egOJ9g/640?wx_fmt=png&from=appmsg "")  
  
将上传路径也修改为相对路径    
  
RuoYi-4.6.0/ruoyi-admin/src/main/resources/application.xml  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMm7icBe5oibPnLG2QcpHxsJ3sbGPJBFPuNDvhB7sibmMWgYbX5oTpBCFuPA/640?wx_fmt=png&from=appmsg "")  
  
创建数据库  
```
create database ry;
source D:\@CodeAudit\Code\RuoYi-4.6.0\sql\ry_20201214.sql
source D:\@CodeAudit\Code\RuoYi-4.6.0\sql\quartz.sql
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmJ4sFDiapRSKzic7PsXBKEyIpknBicbx8vSNd0P7JhTMIatpwyYmEFq0Iw/640?wx_fmt=png&from=appmsg "")  
  
启动  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmxlkQibdcFIWUhhX9ibTFqBuZhdJYynIw395ogcaibK7g1OaYnq9LrfoAw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmTicW5zRHicuDFUPo4hDQdqqRFTRGXym3JfP7pV3WMy5RH8vFCLicibzficQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmvibyVrNtrGZtv167ibvIukIUS6u8WagzIllbJb87MNkspbkRw7bN1eibg/640?wx_fmt=png&from=appmsg "")  
# 2. SQL注入  
## 2.1. ${params.dataScope}  
  
铲子一把梭  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmLtv49MHPJs79SkaZYUfqicAhwUD9LM8W1tmApIXIcSibE4UBlKRmsztQ/640?wx_fmt=png&from=appmsg "")  
  
前端去抓数据包发现这个参数是隐藏的。  
```
deptName=&status=
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmX7iaib70SM8TkesbAqz9uibOT9eHrnch7jpPIBUIicp42uOlLbfyWGFNGw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmic1GqEjgChTYYne9RQE6Xk5OuPzoAHcKKa9BkePnx3iar3lB4nu4B3eQ/640?wx_fmt=png&from=appmsg "")  
  
闭合下  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmDBWmlbZbxp0qrQ3TBFUKy94xkG7ppZhWOdqLjMbNLOag3BTfML1NibA/640?wx_fmt=png&from=appmsg "")  
  
筛选下，有14个sink点  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmqmlowvThM8PRds0MYXhjnNNkL4WEib6466tOJ1LYibfQibdy7nxCsA5xA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmJGOWWCpGwvRPSvMRaVGOaFaEgz6vCLV05ibp4ocXIicHmGAzag85T40A/640?wx_fmt=png&from=appmsg "")  
### 2.1.1. /system/role/list  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMm5q6ias3kJGgiaBTHbkUic1JQgbicrKftSJ4FhhMQn2aCt16iciaWiaNhQjggQ/640?wx_fmt=png&from=appmsg "")  
### 2.1.2. /system/role/export  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmicwoxsxwD8ia1yDBWOIbNklMqWTb7JIop9Zic36I5RaCqvIicXkO0OfBHw/640?wx_fmt=png&from=appmsg "")  
### 2.1.3. /system/user/list  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmGtP7W7AKicqj4ibXKwLtWibaWrQiavZPEVcGEE4PuW7bQtMGElHjsImZDw/640?wx_fmt=png&from=appmsg "")  
### 2.1.4. /system/user/export  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmsIgdicoZBancXqicUplVOLXZcDSgXqROUUdL4ibFaLl2ojzVibup76AibOQ/640?wx_fmt=png&from=appmsg "")  
### 2.1.5. /system/role/authUser/allocatedList  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmCOcDXgicD5FKibs2pIbe1ibCNC0k1VcQobobibXRknG1yib1icUIkDnId2Lg/640?wx_fmt=png&from=appmsg "")  
### 2.1.6. /system/role/authUser/unallocatedList  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmwltWUhbWbUliaqHpeiaEMlpw4gWtq43E8rHichdRKRL93kCupib5icz6Brw/640?wx_fmt=png&from=appmsg "")  
## 2.2. ${ancestors}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMm9sQ5pd0XlJhFMEy7cKxVa3nCaEJLPvzl8ysQ8xvia02t6rmeM8Zia8AA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmer3icSlibJEuWLfCzchuj8J66StSrOrjTY71k4c75UWPianPjs9q6xEtQ/640?wx_fmt=png&from=appmsg "")  
  
全局搜java文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmwwZ6SiawftnRtIBibQqG5W9YczdgyHicgI7cibUoczKQ0zK1658JJUG60w/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmvfFp9FZH80asX4eMibicMC1ThibdOaicmNWFOic8XEAUiaztau1BGYfoz6yQ/640?wx_fmt=png&from=appmsg "")  
  
ancestors有长度限制  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmWKR8n5B2mXQEpZGu1dQyviblibfcBicPOyVGC0ZN2KbdseNITibhf9YMiaQ/640?wx_fmt=png&from=appmsg "")  
  
更多内容进群获取。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OrPlho8vhCVYz7j2m9wiatMmXp31nXCtCt16B0PgW4yjnaIic2tkdrTnYS5ogEPAWamqtzWXWlJtaag/640?wx_fmt=jpeg&from=appmsg "")  
  
