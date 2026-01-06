#  《不用漏洞也能拿下 Root？DC-2 靶机完整攻击链实录》  
原创 蜡笔小qian  黑客映像馆   2026-01-05 11:39  
  
DC-2的靶机下载地址为：  
```
https://www.vulnhub.com/entry/dc-2,311/
```  
## 一、信息收集：从“活跃主机”开始  
  
第一步依旧是老朋友 —— **Nmap**  
。  
  
通过扫描内网活跃主机，很快发现目标：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaL23aiaYDiaicp6aI2Imp5guV4QL0Wf6AA7uwDibH0IqnquicYcn1Ozr2yzQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
紧接着对目标主机进行端口扫描，结果非常“干净”：  
- **80**  
 → HTTP 服务  
  
- **7744**  
 → SSH 服务（非默认端口）  
  
这里已经可以判断：  
  
👉 **Web 是突破口，SSH 可能是后期通道**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaFkGSODiba8YibPKtoxracD6MLASuEmffc1iaLmCuuicZkmyOBSx14ao1rQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1 "")  
```
Did not follow redirect to http://dc-2/
未遵循重定向到http://dc-2/
```  
## 二、Web 初探：一个“访问不了”的网站  
  
浏览器直接访问 IP，却发现页面提示：  
  
这说明网站做了 **域名级重定向**  
，如果不配置本地 DNS，是无法正常访问的。  
### 解决方案：修改 hosts 文件  
  
在 /etc/hosts  
 中添加一条解析：  
```
192.168.186.134   dc-2
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICadxfiaxoJPW532PGgEAooVestgkMeo8ITddeicZjkOgfu1ibEJtjicgs5mQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=2 "")  
  
配置完成后，再访问网站，一切正常。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICapghjU2KJOSvHG09E3YBGlFEoPybzmCSplkJtmIgsaT465sv5Eq6LqA/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=3 "")  
## 三、Flag1 的提示：这次，CMS 漏洞不管用  
  
网站首页可以看到 **flag 入口**  
，找到 **flag1**  
 后，内容非常关键：  
  
这一步直接“点名”了攻击思路：  
- ❌ 不走漏洞利用  
  
- ✅ **走弱口令 + 定制字典**  
  
这在真实环境中非常常见。  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaEoFFkFBOoOcSnIRFWJ5f2GMzjjfiaBOSaacaxEPRsiaA4dw8YxlsvDvw/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=4 "")  
  
这儿提示我们使用cewl工具生成字典，这关不能像常规一样利用cms漏洞攻击了  
  
## 四、目录扫描：后台入口出现了  
  
使用 dirsearch  
 对站点目录进行扫描，很快发现了一个熟悉的路径：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICarpaZmRR8qhCZXTOKumPBuDmo5rJnvhHLH5bt2OE9yiaPVFXW496tIpg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=5 "")  
  
访问后确认：  
  
👉 **WordPress 后台登录界面**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICanLq9j7HfAtxicbicdvHJEABNeiaoAAHCTeuGlCdqV42P94LLcU28ibDfmQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=6 "")  
## 五、CeWL：为目标网站“量身定制”字典  
  
既然 flag1 已经提示了，就直接上 **CeWL**  
。  
### 爬取网站关键词生成密码字典  
```
cewl http://dc-2/ -w /home/kali/dict.txt
```  
  
这个字典不是万能的，但**非常“贴合目标”**  
，这就是它的价值。![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICayJmVVxAMUYPibMKSfPHayJIa3QBE4jjlm7ic2ctFiaic8c4q1HAe0Ct8ibQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=7 "")  
  
## 六、WPScan：枚举用户 + 爆破登录  
### 1️⃣ 枚举 WordPress 用户  
```
wpscan --url dc-2 -eu
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICazsl7v9B5MUQ5R0c95zyEE16WgyovBwyljAa6ick4RN8IZueicXZMCFzA/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=8 "")  
  
扫描出来了三个用户名，接着我们在桌面创建一个username.txt文件，放着这些用户名  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaR7KM9ar4G6ptvJPk2aAKo9Hicfs6DQ4lBggmZSVR7eU8RzqKsHFDtyg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=9 "")  
  
2️⃣ 使用 CeWL 字典进行爆破  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaoR0kiclV4nTdpRB4aK9vZQ7LicZQSxk0icG43ibHJK5gnPaYicwGuEciaITg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=10 "")  
  
  
🎯 **成功爆破出两个账号：**  
- jerry  
  
- tom  
  
后台登录后，在 **jerry 的后台页面中拿到 flag2**  
。  
  
同时再次被提醒：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaEwDxN7A0d6cuUBkWJDmNQ6M6u8exhiaiaNAyFkSDKN2fWeb7CoJqmFNg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=11 "")  
  
七、SSH 尝试：非默认端口 + 用户验证  
  
端口扫描时我们已经知道：  
```
ssh jerry@192.168.186.134 -p 7744
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICa5nfdRZmcaLt4ORQ1dfEl4Magb7oiafbNQSruZsjhcDf4yibvMSyg1RQA/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=12 "")  
  
1️⃣ 尝试 jerry 登录❌ 无论密码是否正确，始终失败  
  
👉 说明 **系统中根本不存在 jerry 用户**  
  
2️⃣ 尝试 tom 登录  
```
ssh tom@192.168.186.134 -p 7744
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICa6g1EDljQxwgkHpziae4uMyqWib7DXKUW6UCibkjrlHuc5dsPtMKMENc7g/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=13 "")  
  
登进去了，看一下权限  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaAGPBiazlz1RTdL1DU8Gz8DfBRsNc1tReTV59UReic7kRsywxE3NKu0AQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=14 "")  
## 八、rbash 限制：低权限用户的真实写照  
  
登录后第一反应：**环境非常不对劲**  
  
**查看 PATH：**  
```
echo $PATH
#查看上面得到path路径的所有文件
#运行结果 /home/tom/usr/bin
echo /home/tom/usr/bin/*
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaThCHickicSctejjc5fhBkKiaJIiaD5ibP5REaSfBQs8kPAtCSod7DcuufCg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=15 "")  
  
只剩下寥寥几个命令，其中：  
  
👉 **唯一有价值的：vi**  
## 九、绕过 rbash：三条命令破局  
  
rbash 并不是“绝对安全”的，**它只限制命令调用方式**  
。  
  
利用 BASH_CMDS  
 + 环境变量即可绕过。  
```
BASH_CMDS[a]=/bin/sh;a
export PATH=$PATH:/bin/
export PATH=$PATH:/usr/bin
```  
  
🎉 **成功获得完整 shell 环境**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICayDibMLQfaxBpK1GqbEmDW4QntLnGzfb9NvJfvtoP0XfZoFXGdYO4C1w/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=16 "")  
  
提示我们得su一下jerry，su是Linux切换用户的命  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICarJrQU5WLFGlYZX1icRZ8kWxJxm4oqztOic42ueOItGyfYXsEKDOUN5Hg/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=17 "")  
  
再输入一下之前拿到的密码即可 登入进去，  
然后用命令找一下flag文件  
```
find / -name *flag*
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaNyiclnlicKMrTGwORc1jib3XlkKExWpicHVLOdQW4ov9YSVibpJzicAibcF5g/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=18 "")  
  
好吧看来除了flag4.txt，其他文件的权限都没有，我们先看看flag4.txt  
```
cat /home/jerry/flag4.txt
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaLX08BMMOUFvVs5Z85oHIudOtHppcB6mqut5JOEe6hq44icShLNGzFPw/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=19 "")  
  
提示了我们  
git  
，还有root看来是要提权的操作  
  
使用 sudo -l 看一下，git 命令可以以 root 权限执行，而且不需要密码  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICaPKJ3qzTCs1Sr6doE1B7DFQDwD3OedLpoMtrwZia9Phr1xBfDhym1OWQ/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=20 "")  
```
sudo git help config
```  
  
回车输入  
```
!/bin/bash
```  
  
 提权成功，然后找一下flag文件  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICakt4oEWvZwZ98TgssuticicT6feTnoJVfMG4dibsG0icKFTnvWDyLxB8cyA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=21 "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICa40X9SI0Q5ImI8O5trldD6dF67ITl4szYzQgIurn6hHvbYz4x06JY6w/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=22 "")  
  
然后打开一下  
```
cat /root/final*
```  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/ibKuGDHxHhvsRM2p0Scic7IXR9aF3icLICavyu6yzZ7kgUibniaAz0kibLRM1CpEicVuYjic2OD31bLSXW7icLyJk06Qy5w/640?wx_fmt=png&from=appmsg&watermark=1&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=23 "")  
  
芜湖！拿到最终flag   
  
总结  
## DC-2 给我们的 5 个真实世界启示  
  
✅ **不是所有靶机都靠漏洞**  
  
✅ **定制字典比万能字典更有效**  
  
✅ **SSH 非默认端口 ≠ 安全**  
  
✅ **rbash ≠ 不能提权**  
  
✅ **sudo 配置错误是 Linux 提权重灾区**  
### 📌 写在最后  
  
DC-2 是一个**非常“真实”的靶机**  
，它不像很多靶场那样一路 RCE，而是：  
> **模拟了真实企业中最常见的失误组合**  
  
  
如果你正在准备 **OSCP / 渗透面试 / 实战能力提升**  
，  
  
这台靶机 **一定要亲手打一次**  
。  
  
  
  
  
  
  
  
  
