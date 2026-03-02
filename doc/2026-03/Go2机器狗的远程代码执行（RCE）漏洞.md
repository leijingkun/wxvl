#  Go2机器狗的远程代码执行（RCE）漏洞  
北京昊网
                    北京昊网  北京昊网CTF题解   2026-03-02 06:04  
  
关注北京昊网公众号，与大家共话网络安全。  
  
  
在春节复工的第一周，国外安全研究员Boschko在博客上公开了一个关于宇树科技（Unitree）Go2机器狗的  
远程代码执行（RCE）漏洞。  
  
不可否认，远程代码执行是安全漏洞中的核武器级别存在。想象一下，家里有一只价值数万元的智能机器狗，它能听懂指令、能自主避障、能跟随散步。但某天，一个陌生人坐在千里之外，通过互联网向它发送一段精心构造的数据包，这只狗突然不听指令了——它可能把家里的地形、摄像头画面传给陌生人，成为攻击家庭网络其他设备的跳板，甚至做出危险动作威胁人身安全。  
  
这不是科幻电影，这就是安全漏洞的现实威胁。  
  
[【安全预警】Unitree Go2 机器狗被曝远程代码执行漏洞](https://mp.weixin.qq.com/s?__biz=Mzk0ODM3NTU5MA==&mid=2247496387&idx=1&sn=ee86b869b4880a788795a834f2da2a3d&scene=21#wechat_redirect)  
  
<table><tbody><tr><td data-colwidth="143"><section nodeleaf="" style="font-size: 17px;font-weight: 400;color: rgba(0,0,0,0.9);line-height: 1.8;margin-bottom: 24px;"><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/OZAM4bH7ZjbZmggKvvSibzQCeib527PcBIicUkZmKKEPsnEP8Zf1icibibn2m9YsNpw0RlhgJ7GNXbiaDDicV72gPwnpXA/640?wx_fmt=jpeg&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="1.7572633552014996" data-s="300,640" data-type="jpeg" data-w="1067" type="inline" data-imgfileid="100001712" data-aistatus="1"/></section></td><td data-colwidth="143"><section nodeleaf="" style="font-size: 17px;font-weight: 400;color: rgba(0,0,0,0.9);line-height: 1.8;margin-bottom: 24px;"><img class="rich_pages wxw-img" data-aistatus="1" data-imgfileid="100001710" data-ratio="1.733271375464684" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/OZAM4bH7ZjbZmggKvvSibzQCeib527PcBIwFqPxWAkyaUDia06LqOYTHBad3SSFKQQpICjsfL8VnZldoormL0sRfQ/640?wx_fmt=jpeg&amp;from=appmsg" data-type="jpeg" data-w="1076" type="inline"/></section></td><td data-colwidth="143"><section nodeleaf="" style="font-size: 17px;font-weight: 400;color: rgba(0,0,0,0.9);line-height: 1.8;margin-bottom: 24px;"><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/OZAM4bH7ZjbZmggKvvSibzQCeib527PcBI937HPbgl7Y1KQnOMxunsEnrrTcaJDKJQTD7XsecropibClnXRCSsdRQ/640?wx_fmt=jpeg&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="1.7399438727782974" data-s="300,640" data-type="jpeg" data-w="1069" type="inline" data-imgfileid="100001709" data-aistatus="1"/></section></td><td data-colwidth="143"><section style="text-align: center;font-size: 17px;font-weight: 400;color: rgba(0,0,0,0.9);line-height: 1.8;margin-bottom: 24px;" nodeleaf=""><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/OZAM4bH7ZjbZmggKvvSibzQCeib527PcBIMwQccEHZD5hAbnA8T2m4ZpZwYk3ibicWXrVtdrknICUgZaVWMpiamwgdQ/640?wx_fmt=jpeg&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="1.7848948374760993" data-s="300,640" data-type="jpeg" data-w="1046" type="inline" data-imgfileid="100001711" data-aistatus="1"/></section></td></tr></tbody></table>  
学网安，找北京昊网，就业有保障，带你冲刺10-40万年薪！  
  
很多人想入行网络安全，却困在没人带、没方向、练不会、找不到工作，  
  
自学半年、一年，依然停留在 “看视频懂，动手就废”。  
  
其实小白入行网安，最怕的不是难，而是瞎努力。  
  
咨询对接：黎歌｜18500324210（同微信）  
  
签约《就业保障服务协议》，未达成协议内就业标准，全额退费。  
  
