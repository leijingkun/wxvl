#  网安实验干货每日分享IIS6.0文件解析漏洞特殊符号";"–0112  
原创 建哥聊安全  建哥聊安全   2026-01-12 08:17  
  
# IIS6.0文件解析漏洞–特殊符号";"  
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
http://ip/upload/up.asp  
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
  
1、登录操作机，打开浏览器，访问http://ip/upload/up.asp  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyao7wqKk9OlHv9vzVARicRwnWm321P6jHCq5a9bZqR0P3ib4ncaq3xIIUuQ/640?wx_fmt=png&from=appmsg "")  
  
2、在操作机新建任意文件名的asp脚本，比如now.asp  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyaoJkBYnbibX4e3D5nWyDhQRp2b5sUKTWfLxTeBkuVD94RgumMHIT6W0Jg/640?wx_fmt=png&from=appmsg "")  
  
3、点击“浏览”按钮，选择新建的asp脚本文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyaoxmA6ic1HTWCbnichcEJGrlb2V5JJyw2ml4KBCgqSEh0iawELibcOORsVIA/640?wx_fmt=png&from=appmsg "")  
  
4、点击“上传”按钮，上传选择的文件，文件格式不正确，上传失败  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyaoKicb0IXVdlhiaySK3OoEBCN6PoqZXW04v7T6H5EmGNHPJmnpSYID9Trw/640?wx_fmt=png&from=appmsg "")  
  
5、点击“重新上传文件”，将新建的文件重命名为now.asp;.jpg，并选择该文件  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyaoGIX8VblLjLv5PpQJqVloicfazEjl5HBNiaT7EZ1pbPF3JGiaQlAsQ48xQ/640?wx_fmt=png&from=appmsg "")  
  
6、点击“上传”按钮，上传选择的文件，上传成功  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyao4qqKxRA8x6KZ9AsyAjG0HPctNuDA8SnibjMaK6VQXeVj2yTYLuKdiblw/640?wx_fmt=png&from=appmsg "")  
  
7、访问http://ip/upload/image/now.asp;.jpg，脚本成功解析  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3CkOAr73jcAf5liabibMyaoRl6PjfocicqBYaia8UuPMPUdVRSw9g3mSOYR5yosEHVVKbrJqksht7Bw/640?wx_fmt=png&from=appmsg "")  
## 实验总结  
  
掌握利用  
IIS6.0文件解析漏洞，绕过白名单限制，解析脚本文件。  
  
