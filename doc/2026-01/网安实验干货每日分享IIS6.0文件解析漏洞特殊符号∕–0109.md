#  网安实验干货每日分享IIS6.0文件解析漏洞特殊符号"/"–0109  
原创 建哥聊安全  建哥聊安全   2026-01-09 02:05  
  
# IIS6.0文件解析漏洞–特殊符号"/"  
## 实验目的  
  
通过本实验，掌握  
windows下IIS6.0文件解析漏洞的利用方法。  
## 实验环境  
  
·  
操作机：  
Win10  
  
用户名：  
Administrator  
  
密码：  
Sangfor!7890  
  
·  
靶机：  
windows server 2003下的IIS6.0  
  
·  
实验地址：  
http://ip/iis  
## 实验原理  
  
文件上传使用白名单做限制，只能上传图片文件，导致脚本文件无法上传，上传图片马绕过白名单文件上传的验证，但是图片马又无法解析，利用  
IIS6.0文件解析漏洞的特点，从而解析脚本文件。  
  
·  
IIS除了可以解析.asp后缀的脚本以外，还可以解析.cer和.asa后缀的文件  
  
·  
特殊符号“/”，任意文件夹名.asp目录下的任何文件都会被IIS当作asp脚本执行  
  
·  
特殊符号“;”，任意文件名.asp;.jpg，后缀是.jpg，可以绕过限制，但是IIS6.0的特殊符号“;”会将该文件当作asp脚本执行  
## 实验步骤  
#### IIS除了可以解析.asp后缀的脚本以外，还可以解析.cer和.asa后缀的文件  
  
1、登录操作机，打开浏览器，访问http://ip/iis/sangfor.asp，asp脚本解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzJkQBdp6DSLooX3ND8hvWXSR4xfWfcRVa7zCsf4eWXU8dGdRMBibuL0Q/640?wx_fmt=png&from=appmsg "")  
  
2、访问http://ip/iis/sangfor.asa，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzRxATAoxAoOlczvicDC8VIgafCm3ZK3uIgo74TFj8H0hTsE6gicOeuRCQ/640?wx_fmt=png&from=appmsg "")  
  
3、访问http://ip/iis/sangfor.cer，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzKShUsxC53Q4mZUD6ej61GVhwm87eVLUWFkL5aBUTAEmmz1pVlgywLg/640?wx_fmt=png&from=appmsg "")  
#### 特殊符号“/”  
  
4、访问http://ip/iis/iis.asp/sangfor.asp，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzCwtiaBhgyQibrGJI6kFdic5IcpQiacHzaMp59xwiaTvovv4yemtzNGfpVow/640?wx_fmt=png&from=appmsg "")  
  
5、访问http://ip/iis/iis.asp/sangfor.txt，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUznzsk9tvtqoOexY6iboPw25Q3EBsQd9DbVicfaNKKYm3F0YFCl29Bw8ew/640?wx_fmt=png&from=appmsg "")  
  
6、访问http://ip/iis/iis.asp/sangfor.jpg，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzGicb0nJrpsX9ygzVU8kEjnunRJJWFsnobO0TnEFTAH9icE0Vunt0FbVA/640?wx_fmt=png&from=appmsg "")  
  
7、访问http://ip/iis/iis.asp/sangfor.rar，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzwyXgmvkh9L0ApsdqFURuw8au3RtdlFW2Od89lqgBBjDelcbOvG9lNQ/640?wx_fmt=png&from=appmsg "")  
  
8、访问http://ip/iis/iis.asp/sangfor.xxx，文件依然解析执行，返回当前时间  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPNibicGL0btfwqyJQWL46rUzicP0n9cwaBxzkbfXn4M8AWFnGGHsdUDDl0ONnWRJW9haHzge0yc4bDw/640?wx_fmt=png&from=appmsg "")  
## 实验总结  
  
掌握利用  
IIS6.0文件解析漏洞，绕过白名单限制，解析脚本文件。  
  
