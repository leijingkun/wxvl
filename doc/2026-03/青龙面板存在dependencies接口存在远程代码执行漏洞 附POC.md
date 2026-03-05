#  青龙面板存在dependencies接口存在远程代码执行漏洞 附POC  
2026-3-4更新
                    2026-3-4更新  南风漏洞复现文库   2026-03-04 15:14  
  
   
  
免责声明：请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息或者工具而造成的任何直接或者间接的后果及损失，均由使用者本人负责，所产生的一切不良后果与文章作者无关。该文章仅供学习用途使用。  
## 1. 青龙面板简介  
  
微信公众号搜索：南风漏洞复现文库  
该文章 南风漏洞复现文库 公众号首发  
  
本人只有 南风漏洞复现文库 和 南风网络安全  
 这两个公众号，其他公众号有意冒充，请注意甄别，避免上当受骗。  
  
青龙面板  
## 2.漏洞描述  
  
青龙面板存在dependencies接口存在远程代码执行漏洞  
  
CVE编号:  
  
CNNVD编号:  
  
CNVD编号:  
## 3.影响版本  
  
青龙面板  
![青龙面板存在dependencies接口存在远程代码执行漏洞](https://mmbiz.qpic.cn/mmbiz_png/b9KQYsB8q6ynd6u8UldSzEVKcia3vjrOjQTyibd7eRx6voFlLqEkkQ26t4MHIAM9M81huB4mgyKBxlONpbOg1o9ibv1A27IpVcpYpPGJR3YBZI/640?wx_fmt=png&from=appmsg "null")  
  
青龙面板存在dependencies接口存在远程代码执行漏洞  
## 4.fofa查询语句  
  
icon_hash=="-254502902"  
## 5.漏洞复现  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b9KQYsB8q6zF48pKH77J9fJmh2DonJfb87Yan5UAIxU2eRAtjBcgD43uryysNqdkY8vQhWHeWDAT0icqywHuGNlTkI0ZZhiah8IicHGfEnJ7oc/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/b9KQYsB8q6y5fglpFtvDsHHTGw2JGh5Iw85FUqO44D3k6X4lEdnMrdgKoJlNEicLiaA8bBcl0ctpjqpYiaa6ae7KpXrpibKjCF98xrm0aBSTeAY/640?wx_fmt=jpeg&from=appmsg "null")  
  
## 6.POC&EXP  
  
本期漏洞及往期漏洞的批量扫描POC及POC工具箱已经上传知识星球：南风网络安全  
1: 更新poc批量扫描软件，承诺，一周更新8-14个插件吧，我会优先写使用量比较大程序漏洞。  
2: 免登录，免费fofa查询。  
3: 更新其他实用网络安全工具项目。  
4: 免费指纹识别，持续更新指纹库。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/b9KQYsB8q6zdEFictfTzrLKZ6hicUicuRJeaYQCSFTrMvptqd0cHibLRuibvJLlE29tIyCKmwmKR6nPg0nA590KWC0Y4Bib8IJB90cLZUKiaQbOHQk/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b9KQYsB8q6yD7eibllIxm6PRHt3R6Nl18u8hH6urR6wW5hw9CX1Y295F6hZtLmicV97bHY9YK73354dqo4H7Eer3MbsngtgfWTXH75pWy3zXA/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/b9KQYsB8q6wBUegzQt7a4FDVRPfESjpLmiazCIfeCt3jvuL26oV2Dd33OYpp5mhj1Xa932n8zrquibgKPx5JVAt9b3HDreE0enibXV34icKibgmI/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b9KQYsB8q6ywqEFMPkFIMD2wianOmTHQem9PLSqaibicMfN8jffdIjZMeKyGkdtvZMcF7ET2icpzQIibmBUXiaEuUBdVOhB7qeApoyBLhlVtQUlx0/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b9KQYsB8q6xlwjPBsLg41vd27ForsgAor5QsQhI9L5bfJ95NUXVDQ69YdVAJlmwrwWedHr4sytKqxOvSk4STgI9PE5leecvtbDPria6x7ciaI/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/b9KQYsB8q6wPnoNTX0sneMECCR8ibsZDmUpAryUr6b6ibMUbjyCvD0ZXKqXB7AnNkap9fI7GS8NyibndDExIlctj6hRScYRNuSj4DJic7oTwQYk/640?wx_fmt=jpeg&from=appmsg "null")  
  
## 7.整改意见  
  
打补丁  
## 8.往期回顾  
  
  
   
  
  
  
