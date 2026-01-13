#  网安实验干货每日分享IIS7.5文件解析漏洞-0113  
原创 建哥聊安全  建哥聊安全   2026-01-13 03:04  
  
# IIS7.5文件解析漏洞  
## 实验目的  
  
通过本实验，掌握  
windows下IIS7.5文件解析漏洞的利用方法。  
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
windows server 2008R2下的IIS7.5  
  
·  
实验地址：  
http://ip/index.php  
## 实验原理  
  
文件上传使用白名单做限制，只能上传图片文件，导致脚本文件无法上传，上传图片马绕过白名单文件上传的验证，但是图片马又无法解析，利用  
IIS7.5文件解析漏洞的特点：任意文件名/任意文件名.php，从而解析脚本文件。  
## 实验步骤  
  
1、登录操作机，打开浏览器，访问http://ip/index.php  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJES12g6BESevibK03AqtPlwOxjVp634j6JI69unGK513qP83WpibOV5WJg/640?wx_fmt=png&from=appmsg "")  
  
2、在操作机上准备要上传的文件（任意文件都可以），比如新建info.php文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEicD1mFfACuXwngLG6CCzLGrp69QyAa7Zv9ll4rPcdz1SAbDCDwQvWiaw/640?wx_fmt=png&from=appmsg "")  
  
3、上传任意的文件，发现为白名单验证。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEybxwLgXokB9FkgUicUZm1BFazHArCvZOYgxjvrWib9R6qib01weibK70Qw/640?wx_fmt=png&from=appmsg "")  
  
4、创建后缀名为.png的文件，文件内容如下。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEbXkWb3PmFmkcRib7HUhWcZZMyHicmAwsRnbnh2sq7mlicWEp8OrITCUhA/640?wx_fmt=png&from=appmsg "")  
  
5、上传成功，F12查看文件返回地址。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEibHibNB8iaPCzcbUjWibwBPcP5koVBolgRKZ77rrJHaveTlNUzPgPqTmng/640?wx_fmt=png&from=appmsg "")  
  
6、访问文件。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEqUFU7kxTWUAqjgic9QAgnvMo2zr81ZHAhZ4iaQSV4CibJQYh4ttt5Gl6Q/640?wx_fmt=png&from=appmsg "")  
  
7.利用IIS7.5文件解析漏洞特性访问：  
  
http://ip/upload/6720210701210811.png/.php，脚本文件成功解析  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaOqqicQYKv4jcVHhWKVNFXJEGtGnl4ianOianXLgXC0ZCHIomNXmW11nK0xva6ibQoxKONrqmfAJ0oukg/640?wx_fmt=png&from=appmsg "")  
## 实验总结  
  
掌握利用  
IIS7.5文件解析漏洞，绕过白名单限制，解析脚本文件。  
  
