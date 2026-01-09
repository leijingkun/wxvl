#  本地电脑没有PIN密码也可以实现文件窃取? Windows11WinRE权限控制绕过漏洞演示  
原创 网安武器库  网安武器库   2026-01-08 16:16  
  
**更多干货  点击蓝字 关注我们**  
  
  
  
**注：本文仅供学习，坚决反对一切危害网络安全的行为。造成法律后果自行负责！**  
  
**往期回顾**  
  
  
  
  
  
  
·[Hashcat密码工具：快速识别百种哈希密码类型+GPU加速密码爆破](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485727&idx=1&sn=da20b5443572fb7e1550eb1e42b28dbc&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Nikto：一款Web安全漏洞扫描器](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485724&idx=1&sn=8daf51f5fb3946907463096de6309731&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[HTTrack爬虫：网站递归式资源抓取工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485715&idx=1&sn=819198a15960118702253dffd868f127&scene=21#wechat_redirect)  
  
  
  
  
  
  
·["手机版的kali"黑客工具：Tool-X使用和安装指南](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485663&idx=1&sn=95cbaadb12dcf74077ec6e0bf8448c11&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[pentmenu:黑客必备实战流量攻击工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485644&idx=1&sn=ef9b011a20e30cea78592357736ca177&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[钓鱼工具分享：I-SEE-YOU获取受害者地理位置，实现物理”开盒“](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485643&idx=1&sn=2b298e43ac23c9b7ded4c9cddd5c1d0b&scene=21#wechat_redirect)  
  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**介绍**  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmUuwPGibDTkx7WBGsaUQyXHhNVkRysibdtFUp0Ac1UoqoA50VkhNetQ1g/640?wx_fmt=png&from=appmsg "")  
  
  
      Windows 11 的WinRE 权限控制绕过漏洞—— 其核心是 Win11 恢复环境的身份认证逻辑存在设计缺陷：在 WinRE 启动流程中，当跳过 “系统修复（Repair）” 节点直接调用命令提示符时，系统未触发本地安全认证子系统（LSASS）的 PIN 验证流程，导致低权限的 WinRE Shell 被无身份凭证地激活。  
  
      WinRE 作为 Windows 预启动执行环境（WinPE）的子集，其默认权限上下文为本地服务（Local Service）令牌，虽受限于文件系统访问控制列表（ACL），但可通过bcdedit、dir、type等原生命令行工具遍历已挂载卷的未加密文件资源 —— 而 Windows 10 的 WinRE 则强制在 Shell 启动前完成用户主体（Principal）身份绑定，因此该差异可被定义为 Win11 的认证边界失效漏洞，属于本地权限提升（LPE）类漏洞的前置攻击面。  
  
       本演示使用的是windows11 x64 24h2 ,证明可用。  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**初步演示**  
  
  
  
下面先演示正常情况下如何进入命令行界面测试功能  
  
（不使用密码的方式见下一部分）  
  
找到一台win11电脑，进入设置--系统--恢复  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmUxkxlpCDXJNPFKOHXEQGITCYbmtnqYZ9vMtk9KufpCRGIgbFrYjGtQ/640?wx_fmt=png&from=appmsg "")  
  
然后点击高级启动--立即重新启动  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmAgtJaTDwLicNtCgoSABbR2B3BAOiciar4N89ltPNiaaeVAdiaJS1O1oW8fQ/640?wx_fmt=png&from=appmsg "")  
  
然后会出现下面的界面，点击“疑难解答”  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmGjYJ8niaWhUhuwfzmuJx7gAuGtLpP5aoaCx26tOSOdCCaQb7uFuQbkg/640?wx_fmt=png&from=appmsg "")  
  
高级选项  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmdDFImiaFOypzXMoo68kTvz8eVrLHSAHTIMcRf28ZbYSnWXhkwiaV9Zvw/640?wx_fmt=png&from=appmsg "")  
  
之后进入下面的界面  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmo2ojObHibsGhPdPMic9Q2RBGgNQjf8dmSFpt7vK8dfficPeLJd3pOHCHA/640?wx_fmt=png&from=appmsg "")  
  
这里点击上面的红框就会开始启动修复，但是我们当然不是来修电脑的，点击一下下面的命令提示符  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmkphiaAtlmian9gep1rOjAtoczBTnEejCYSWpHssFQfxia9ATUjcDOR2lA/640?wx_fmt=png&from=appmsg "")  
  
输入一下指令查看文件会怎么样呢  
```
C:\users\你的用户名
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmUG74RjerLmCyqztf1LLYCuIPp1Yfo4WqJUbcjN7YxbFJaaXYMaibY1w/640?wx_fmt=png&from=appmsg "")  
  
直接输入  
```
C:
```  
  
也可以进入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmV8w8FoEvBOMWsqmKu806DQBJF3YAPI0iam8AiaHM4dTQbdY77S3KbNOQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**没有密码也可以为所欲为**  
  
  
  
刚刚只是演示了可以查看用户目录下文件，下面演示一下可以拷贝文件从而进行文件盗窃--  
即使不知道你的密码  
  
关机状态下，不进入桌面，可以通过下面的方式进入上述界面  
```
1.启动电脑。
2.一旦出现Windows，立即按下电源按钮以中断启动顺序。
3.重复步骤1和2两次。
```  
  
  
然后根据前面的步骤找到命令行界面、  
  
依旧输入  
```
C：
```  
  
  
进入系统盘目录，cd到你想看的界面  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmUjs7icyFN5Y5y4tH1zyibPdOaZFCiaxFvUwn2Tdx3LHRHDksmBxzhicgGA/640?wx_fmt=png&from=appmsg "")  
  
现在假设我想得到这个电脑上的文件 2.txt  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmFzsnYkBgAjrDzOicjchKoHG2sfuWxia5761tgdstgLlUvwKhnkTdgSBA/640?wx_fmt=png&from=appmsg "")  
  
只需要输入  
```
copy 想要的文件的位置 目标地址 #有空格的话需要打双引号
```  
  
比如这里我输入的是  
```
copy C:\users\KIVEN\desktop\2.txt C:\users\KIVEN\desktop\3
```  
  
后面改成U盘地址就可以拷贝到U盘了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQm6Gfv9EQ390thZk93v1hvkoEyicAjiaNpYSOLrEjzCdxm001EXITvml7Q/640?wx_fmt=png&from=appmsg "")  
  
也可以直接输入  
```
notepad
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmN18prglMHv8icFgWbhwd5yfP5gBn5SP3XcNf0GLmJZ9vGtXEIWGiaYuQ/640?wx_fmt=png&from=appmsg "")  
  
出现了文本文档读取界面  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmHQOpv6BYXN9QrgXLJ42ib9o1tqyZxoZZBtsCzbLQx3C8Mx7o2iciaX4Vw/640?wx_fmt=png&from=appmsg "")  
  
选择  文档--打开--就能看到想看的文本文档了  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
**需要更高权限？**  
  
  
  
执行命令环境，比如c依赖等，前面的方法可能权限不足，就可以使用下面的方法  
  
还是目前的界面，输入  
```
cd C:\Windows\System32
copy cmd.exe utilman.exe
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmSTAFZ1OM6fLadI6dDGcaDRFicUwb8Cnhcg881eGTtMhAHUhIODrhwCA/640?wx_fmt=png&from=appmsg "")  
  
然后重启  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmHmIgxxqM2iaGicibWHQTNcpnBrMb4YmHawS1lAWqaUyVXKvRNG1YKSGvQ/640?wx_fmt=png&from=appmsg "")  
  
我们假设不知道密码，所以这里当然没法进入，但是点击下面的图标会跳出终端  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQmv2twVibQ5LMykqQnam4dJJknoPYYzYeaYjLj6xxHYQXXfNjzh3RUlUA/640?wx_fmt=png&from=appmsg "")  
  
用法与前面一样，但是权限更高，比如这里再复制一份2.txt到文件夹4  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQm5kdu4SYBvBlibPtPv6rf1CrhmphOZ9AOKZOl5dicpD0ia0Ply1Na9DQrw/640?wx_fmt=png&from=appmsg "")  
  
可以看到非常成功  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ujwEpJ8e5eiapzo5oEkMWTQm2gwkucappmYBziaBMicpR2YWeZ8nHian9zHk8x9Tmg1iadm0XZmDib26htw/640?wx_fmt=png&from=appmsg "")  
  
输入notepad查看文件也可  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eRUtCzBCFbaMYy1c7utlweibCFXWsicmm9ebyvInBtdsD0QRlUDTdLib1g/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
