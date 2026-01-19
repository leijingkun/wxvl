#  知名心理健康服务平台存在SQL注入 - 0day  
原创 zyxa
                    zyxa  众亦信安   2026-01-19 03:29  
  
声明：  
文  
中涉及到的技术和  
工具，仅供学习使用，禁止从事任何非法活动，如因此造成的直接或间接损失，均由使用者自行承担责任。  
  
众亦信安，中意你啊！  
  
  
****  
**温馨提示：当前公众号推送机制调整，仅常读及星标账号可展示大图推送。建议各位将众亦信安团队设为“星标“，以便及时接收我们的最新内容与技术分享。**  
  
****  
****  
  
  
产品介绍  
  
瑞格智慧心理服务平台通过**SaaS 云部署 + 私有化部署**  
的灵活模式，实现了内外网环境的全覆盖，既满足了普通用户便捷访问的需求，又保障了敏感机构的数据安全和隐私保护要求。平台以 AI 技术为核心驱动力，结合专业心理学资源，为不同用户群体提供个性化、全周期的心理健康支持服务。  
  
  
漏洞情况  
  
瑞格智慧心理服务平台存在SQL注入漏洞，未经身份验证的远程攻击者除了可以利用 此漏洞获取数据库中的信息（例如，管理员后台密码、站点的用户个人信息）之外，甚至在高权限的情况可向服务器中写入木马，进一步获取服务器系统权限。  
  
fofa  
```
"/images/imagesSchoolLarge/login_main.png"
```  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzqbQCkuEDJLHmUaTaDJ1WOlSgJvWRHxMEjx7Eia7gXJia4N8b4Yicx0qYQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzgVDZfptWacMRjqic6ouZ7uNdR8ZicbMXibicsJt5OyQg0VJub5btrvsRGQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzteoeD0McHPLzicc9ptEdo1TOm3K0yzNmia3A0AXMTqgUBBRdsqU9lX4Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
tips：  
  
  
加入圈子后提供poc 请师傅动动发财的小手点个赞  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/rl85Sib8ZkaBrDvy8TKfP3pmENHPYQRvn4XLcUULvT3EP6RBaiaGc589eJm59m2dApwReGGrpHqyToepZLpWebqQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
圈子介绍  
  
方向：渗透、漏洞挖掘、免杀、逆向  
  
  
已  
更新  
内容：  
  
1. suo5二开，流量修改以及客户端工具  
  
2. 哥斯拉特战版二开，并有长期更新计划  
  
3. edu和erp 0day披露  
  
  
  
后续计划：  
  
1、自研webshell管理工具  
  
2、cs远控  
  
3、  
内网漏洞批量检测工具、fscan二开（web界面）  
  
4、src，edu高赏金积分报告（脱敏）  
  
5、hw这么多年来累积的实战案例报告  
  
  
目前定价129/年，  
前30名的师傅享受85折优惠。  
  
  
  
盒子  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzSYQ2ft7SpZJiaNhkMEHa4olwMqgDF86Y1Rc70kXfBTqibibAf62r3TjGA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzBhic98u0LXENj6sHQxxgyGIK5XcvjrY7JJm1hNDibicUZW2bpBXCmvJMw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uznibrby1PK9yJKnTcjc7NoTmtiaZktRiaxhTu9cjpFfwgaeX2js941Xzvg/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzFtEgunKrrEq6V8tSibzmElXMQEM3v6HbTfxOONkmPFZ3AObkKesjKHg/640?wx_fmt=png&from=appmsg "")  
  
携程  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzXU3Q2ABoTOFh1ozp1LAVMtYpsxklFcWZU6owbicIqALDqc6raYYN0Zg/640?wx_fmt=png&from=appmsg "")  
  
  
小红书总榜第二第三  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzOgpLDiamWuWLnQGIRPiczic4lNn0c3ibLN26kFtJgXSGeViarYurPSkic6QQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzsa4ia5ibRB1Eicyicibz7ibIIlNeoibl8eLibpsVhbmSj52C3xC64u2pKQojiaA/640?wx_fmt=png&from=appmsg "")  
  
  
某攻防  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzJZicRwZ0e32I8v9B2mLJicaauHZHaehefCz0pMteFE64geiaGcAeUanTQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzwId5Hg0QbVhHetWonF9j4ASrJCDYeamMxyGbooB6gZpJZib7adsXuTQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzgYvM2N7SwvfsS1cwXZJF4zicscibTibQwZpN5gxia1vct0b9zZTQM7ibyTQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaCOHZticLo6kZuTsH2czn9uzeHvdgDlh7ryS0dMliaNLp2jibFVmnuWVj9dqDLMZ3Ou5g7ibndnJ977Ww/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaBrDvy8TKfP3pmENHPYQRvnEtp1k6O7c0icMNy4sgKEynO2zp3FPnS8fBvJzDdiaWlEmIiahJgFwZqCQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaBrDvy8TKfP3pmENHPYQRvnbHN2uEmgbqrE08j5tX7OJJX2sp1TrQpvwbxb3DC0IJZ5nWQfricHclg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaBrDvy8TKfP3pmENHPYQRvnVtI3d2czN9EjTygzTykSopMnhsHcEk3bdMCCNl3vXONs7hL7JCPzzA/640?wx_fmt=png&from=appmsg "")  
  
  
  
往这里看  
  
  
  
  
点点关注不迷路，不定时持续分享各种干货。  
可关注公众号回复"进群"，也可添加管理微信拉你入群。  
  
项目交流，src/众测挖掘，重大节日保障，攻防均可联系海哥微信。  
  
入了小圈的朋友联系海哥进内部交流群。  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/rl85Sib8ZkaALhRNp0ic9JdTb3u3x0wr8NEKVpvibCaGWymICEcwUbmO3icFAJwSvxbszDKv7OXQwoDjtrmVRvN91Q/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=10 "")  
  
  
  
  
  
  
