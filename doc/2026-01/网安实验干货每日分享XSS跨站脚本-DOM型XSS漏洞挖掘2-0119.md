#  网安实验干货每日分享XSS跨站脚本-DOM型XSS漏洞挖掘2-0119  
原创 建哥聊安全
                        建哥聊安全  建哥聊安全   2026-01-19 01:45  
  
# DOM型XSS漏洞挖掘  
## 实验目的  
  
通过本实验，掌握挖掘  
XSS漏洞的方法以及根据XSS漏洞不同分类的特点，判断挖掘出来的XSS漏洞属于什么类型的。  
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
Apache + PHP  
  
·  
实验地址：  
http://ip/xss/06/dom2.php  
## 实验原理  
  
在页面文本框的位置，传入提交的数据，从而构造  
Payload使其弹框，掌握DOM型XSS的特点。  
## 实验步骤  
  
1、登录"Attack"操作机，打开浏览器，访问http://ip/xss/06/dom2.php  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPErtO5cBkbwIHDhDDqLicmRQudVbr8AV8RnYibSRs4HqibRZsm8TQh0iaBc4zxrsMzDxapibI0xnNv7EQ/640?wx_fmt=png&from=appmsg "")  
  
2、在文本框中输入任意字符串，比如“xss”，然后点击“替换”，页面字符串发生替换  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPErtO5cBkbwIHDhDDqLicmRDnCY1kMEAJYTUwmnsYYViciaINlxdzM6WvpeDMXvicMyqCNCMquD7GbGg/640?wx_fmt=png&from=appmsg "")  
  
3、构造XSS验证语句  
  
<img src=1 _onerror=alert(1)>  
  
输入文本框，成功弹框  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaPErtO5cBkbwIHDhDDqLicmRsHFfEfIwssnAia32PxWIDVJ6vdUMpQ9mG3kSQ6Dm1LgoobYn9anB5tA/640?wx_fmt=png&from=appmsg "")  
  
  
## 实验总结  
  
通过本实验，掌握挖掘  
XSS漏洞的方法以及根据XSS漏洞不同分类的特点，构造XSS验证语句，使页面弹框；判断挖掘出来的XSS漏洞是非持久性的，用户每次访问该页面都需要构造一次payload才会触发弹框，属于DOM型XSS。  
  
