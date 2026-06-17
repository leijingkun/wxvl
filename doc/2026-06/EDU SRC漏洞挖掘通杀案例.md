#  EDU SRC漏洞挖掘通杀案例  
原创 zkaq -nnsae86
                    zkaq -nnsae86  掌控安全EDU   2026-06-17 04:00  
  
扫码领资料  
  
获网安教程  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/BwqHlJ29vcrpvQG1VKMy1AQ1oVvUSeZYhLRYCeiaa3KSFkibg5xRjLlkwfIe7loMVfGuINInDQTVa4BibicW0iaTsKw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=0 "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/b96CibCt70iaaJcib7FH02wTKvoHALAMw4fchVnBLMw4kTQ7B9oUy0RGfiacu34QEZgDpfia0sVmWrHcDZCV1Na5wDQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=1 "")  
  
  
# 本文由掌控安全学院 -   zkaq-nnsae86 投稿  
  
**来****Track安全社区投稿~**  
  
**千元稿费！还有保底奖励~（https://bbs.zkaq.cn）**  
  
****# 声明  
  
只做技术讲解，且所涉及漏洞已修复。切勿用于违法途径，所有渗透测试都需要获取授权，违者后果自行承担，与本文章及作者无关，请谨记守法。  
# 前言  
  
EDU后台学生教务系统，多个接口存在越权漏洞，后续分析发现为通用系统，可直接实现通杀，也是兑了几张证书玩玩。  
# SQL注入  
  
某学校，需要统一身份认证登录  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoLz8zfYzvTbawjVBOrJGdJJebV0PreF31GBduXLmJicDfAdaqB9foTANVqY6d8KrKSA8Wv78iczBd8KfNpg3UkFs0ck8e5cnWWcU/640?wx_fmt=png&from=appmsg "")  
  
这个很好搞的，EDU类型的系统，肯定是学生最多，随便弄个账号就行了，具体怎么弄不说了，反正网上这类型的文章都很多了  
  
登录进入系统后，直接是个人信息系统，其中存在很多模块，但是有些模块只有教师账号才能使用，不过这里有一个学生基本信息管理系统模块  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoJxA2HLVVg2J7uJoP2X1Sy2T19XYNPgHBayhRfSe3EAzF2qPo6FgmhE9SWxzIccuz3MmvpfqvEnDicPpxFQf1Btb9KSN4axpEPo/640?wx_fmt=png&from=appmsg "")  
  
进入后可以看到自己账号的身份证学号等敏感信息，还有一个学籍卡导出功能  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoIcrmkvVFXibgoqEPhVK73n6kxicd7Q3y8icNAoiatiaxcy0yfluicBYtnwK4duejbbBvZKdczhMw4gG7vsl6ZU0ibIibaC1mVwsYEr4Eg/640?wx_fmt=png&from=appmsg "")  
  
发现某个参数经过测试存在SQL注入  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoJgibjr0ANtwUvBBXzPFA76icaKMMaT52rrRJa8NiczcFvVv9Y0aZ12BWoRlvzhBjz1psWkDkhlEfyxhkVAhm6QAKmLno9MpPC8l4/640?wx_fmt=png&from=appmsg "")  
  
两个单引号回显不一样了  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoLib0uNMw9Bd9AguSGV0hQQpYZzg7FNo62tfX83jDXW0TAsYH9V4x4cibAuQvk6Xpp19FClfMcAFZhibZeQNY8e6cpVDz1fTqrcOY/640?wx_fmt=png&from=appmsg "")  
  
此处经过测试应该是存在布尔盲注  
  
注入用户名，第一个跑出来为U  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoLAa3crfXghngbjrpjnNtycOcCnXvskJvnwggKJVh1kp9jtGXnGvib7N2de8OrZSRVo5vW5XdNWSLiaibxjeE2BiauSvrHm3vib5rTg/640?wx_fmt=png&from=appmsg "")  
  
第二个为S  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ianpxKPnLHoLAS7nWkcVueSjSauGaYBGup0CzZEVIo9mCfyydmR7Mgn3V0LiblfmhQhCttaiaiaIxQ4cjqgkR4tBqLMuwPhWdLPnrwwVgxWUepM/640?wx_fmt=png&from=appmsg "")  
  
第三个为R，成功注入处数据库用户名，sql注入到手  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoLteuFuzw5ChJfGN45zQliaibibyz3HsO5nVcCkPVqueFiaz6ibURIgP08R58HiaRYMic2m2NYHXvAZRAB5GCnd4G92JWVrFHCfFQcB6A/640?wx_fmt=png&from=appmsg "")  
# 越权  
  
后续发现如下模块存在越权  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoLhJJeEwibHcQKTfXicIl65Az8QL2J4cxWBRRw6T8yKSccv8ltQSJcUfiaI4R4WTBVIDHzuzMzjmyQLNWxavmOhvQl3SPvSF0Biaqo/640?wx_fmt=png&from=appmsg "")  
  
点击进入，这里主要是填写申请信息啥的  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ianpxKPnLHoIvQRNia0I8BWMe5vnaq3m1SyD5tWZOGh5T3nZorH2mysNBswM54FOBFyhTMS3yPUA35TKHgaOveneXF5tIDoMmPicMt493kyvRM/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ianpxKPnLHoIHibqy6h97Uvhk8JbbKjiaDibmHjEPha17CvficILMRmdBpl1ekyNWQhZibacg0UHNicsibicr6aA8fygzCSia6BUBCWcd6cO9ya8IUkL0/640?wx_fmt=png&from=appmsg "")  
  
填写好信息，然后抓包，但是发现这个账号没有身份证号还是咋地  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoIiawuKUI9CVeC5RhfVuFtnD1F3c6tbtQwlzwAeUIZZ1LZmseqfWVPDjbeBJickhdgXmaRTBQBlfRWic61BT8NMDxY0JbtV4ziaIpc/640?wx_fmt=png&from=appmsg "")  
  
直接越权遍历，成功哪些越权  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoJiaicp5EiaqMrbmKUibwZ936qGjKFL6c7WSVsL2Dyf2jurNTTHmuLmbWniaUYHNwdI0Sq9dico7YRSpDpfCiccKzvw8yznN2D7giakWicc/640?wx_fmt=png&from=appmsg "")  
  
  
后续也是批量刷一波成功交到EDU  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ianpxKPnLHoJIeUNx4Jiby4I09QOFLTXuMtqx5RJbgB3uPic60gp6yCgfIpqEP86Qu40EvA9T5rlZccia7aUyVZnH1QZZeDw06JOdXwf0tGZmBk/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ianpxKPnLHoKscp6cRPr65ZHfu7Z6Sg09XpKia0bs5ibdEXLlAeQybiahtnRvx8hMSibJoLMaGzhxUddqSN9SSqA9oYBxJWujZgwiaraiciar8icFDkE/640?wx_fmt=png&from=appmsg "")  
  
随便换了几张证书  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ianpxKPnLHoJ4H3aQWQayfiaDM8LicBKZHYdLaGMEqHSe0WV6qFRcf4mBuHRsH0Oq5bmvHTYAFalAseY2XqT9OsjC8d7icPpYX1PLnSSxrtSN30/640?wx_fmt=png&from=appmsg "")  
```
```  
  
  
