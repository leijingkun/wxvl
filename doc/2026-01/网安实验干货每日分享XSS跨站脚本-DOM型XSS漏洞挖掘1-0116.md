#  网安实验干货每日分享XSS跨站脚本-DOM型XSS漏洞挖掘1-0116  
原创 建哥聊安全
                        建哥聊安全  建哥聊安全   2026-01-16 06:39  
  
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
http://ip/xss/06/dom1.php  
## 实验原理  
  
在页面  
URL的位置，传入"name"参数，从而构造Payload使其弹框，掌握DOM型XSS的特点。  
## 实验步骤  
  
1、登录"Attack"操作机，打开浏览器，访问http://ip/xss/06/dom1.php  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3gZdLdqEKzYxUJSkyLOUKMHp1busWbk1Eia4TXAgRO4bIt1voRtnH5ads45VOGRsKo5FYiaibhKQ1Q/640?wx_fmt=png&from=appmsg "")  
  
2、页面没有任何有用信息，在URL中传入name参数并赋值，访问http://ip/xss/06/dom1.php?name=sangfor，页面显示name参数的值  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3gZdLdqEKzYxUJSkyLOUKAIUiaDZWv5EG09NvBoBlVVOr6hoqibLz1Rvnwur81hicvZXiaPxlZdUn6w/640?wx_fmt=png&from=appmsg "")  
  
3、构造XSS验证语句，传入name参数，成功弹框  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Yxh0GAibwTaM3gZdLdqEKzYxUJSkyLOUKufOhUryMRl5tXwS3tYOGV8maqjdnwSFINHkK9TITMQNMKhHcHW37icg/640?wx_fmt=png&from=appmsg "")  
## 实验总结  
  
通过本实验，掌握挖掘  
XSS漏洞的方法以及根据XSS漏洞不同分类的特点，构造XSS验证语句，使页面弹框；判断挖掘出来的XSS漏洞是非持久性的，用户每次访问该页面都需要构造一次payload才会触发弹框，属于DOM型XSS。  
  
