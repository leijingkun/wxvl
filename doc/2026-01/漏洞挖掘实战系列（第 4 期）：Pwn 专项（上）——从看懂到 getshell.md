#  漏洞挖掘实战系列（第 4 期）：Pwn 专项（上）——从看懂到 getshell  
原创 点击关注👉  网络安全学习室   2026-01-08 02:32  
  
Pwn 模块在 CTF 里占比高、难度大，但**栈溢出**  
是最稳的得分点之一：套路固定、工具成熟、真题重复率高。这一期我把“栈溢出从挖洞到 getshell”拆成你照着做就能复现的流程：**找漏洞点 → 算偏移 → 找 gadget → 写 exp**  
。全程用 2025 常见真题风格举例，脚本可直接改地址复用。  
## 一、先把环境搭对：Pwn 入门三件套  
### 1) 必备工具  
- **checksec**  
：看保护机制（NX/Canary/PIE/FORTIFY）  
  
- **IDA / Ghidra**  
：反编译找危险函数  
  
- **gdb + pwndbg / gef**  
：动态调试算偏移、看寄存器  
  
- **pwntools**  
：写 exp 脚本（远程连接、打包 payload）  
  
安装建议（Linux/WSL）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmX1yicB8Mb8ugLcqPkHzCibuNFU6sJTJ3RDvVqsP7DgNm0pdscogiayaGw/640?wx_fmt=png&from=appmsg "")  
  
## 二、栈溢出的本质：把“返回地址”改成你想执行的地址  
  
栈溢出发生在：程序用 **gets / scanf / strcpy**  
 等函数读输入时，**没有限制长度**  
，导致你输入的数据把栈上的 **ebp / return address**  
 覆盖掉。  
  
你要做的就是：  
1. 用垃圾数据填满缓冲区 + 覆盖 ebp（可选）  
  
1. 把 return address 覆盖成某个 gadget 的地址  
  
1. 让程序“返回到”你安排的代码流程（system("/bin/sh") 或读 flag）  
  
## 三、第一步：用 checksec 快速判断“能不能打”  
  
拿到二进制文件先跑：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmA1xRic0iaiaxChCWEzqp6qQ9Zap6JPFVnBHicTUEVqJDVsj57fJYJ7icflQ/640?wx_fmt=png&from=appmsg "")  
  
常见结果与策略：  
- **NX enabled**  
：栈不可执行 → 不能直接塞 shellcode → 用 ROP / ret2libc  
  
- **Canary found**  
：有栈保护 → 先想办法泄露 canary 再覆盖  
  
- **PIE enabled**  
：地址随机化 → 需要泄露基址再计算偏移  
  
- **FORTIFY enabled**  
：部分危险函数被替换 → 更难直接溢出（但仍可能有其他漏洞）  
  
新手最友好的情况：**32位 + NX on + Canary off + PIE off**  
。  
## 四、第二步：IDA 找漏洞点（重点盯这 3 个函数）  
  
打开 IDA，看 main 里的调用链，重点找：  
- gets(buf)  
  
- scanf("%s", buf)  
  
- strcpy(dst, src)  
  
只要看到类似：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmSkFPrWz3HSSBJibfodm0U7PvABJrw6qc8hictasOzDYUObctpHAFUcibw/640?wx_fmt=png&from=appmsg "")  
  
基本就可以按栈溢出做。  
## 五、第三步：gdb 算偏移（最关键也最容易错）  
### 1) 生成 pattern  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmVJSFndj0hicIYFWcOKlwYNnzW7LhrmoeFczL58FpPTwTNGwmjTbRQTw/640?wx_fmt=png&from=appmsg "")  
  
把生成的字符串作为输入跑程序，程序 crash 后看：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmMicKqHyxeqmrWa9YZvbnItOSN2v6YY5Vw1dPm2WjvRHusRv7SGmxBibw/640?wx_fmt=png&from=appmsg "")  
  
看 **eip**  
（32位）或 **rip**  
（64位）里的值，比如是 0x41384141  
。  
### 2) 找偏移  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmiaMkSlbEfeeRSVibekvsWHn0lM0ia3rRqdLia58RA2sZnUFyQzYsaSR3Dg/640?wx_fmt=png&from=appmsg "")  
  
得到 offset，比如 **offset = 44**  
。  
  
经验：32位常见偏移 28/32/44/52；64位常见偏移 40/48/56（具体看栈帧）。  
## 六、第四步：构造 payload（两种最常见打法）  
### 打法 A：ret2libc（NX on 但无 Canary/PIE）  
  
思路：让程序调用 system("/bin/sh")  
。  
  
步骤：  
1. 找 system  
 地址（libc 里）  
  
1. 找 /bin/sh  
 字符串地址（libc 里）  
  
1. 找一个 ret  
 或 pop ...; ret  
 作为跳板（对齐栈/控制流程）  
  
exp 模板（32位）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmm6kTBPgcrgWcMrJOQ5mqGA3RT5RlwTBf4YwibjCUiaVVzbQRSJ1CZs5g/640?wx_fmt=png&from=appmsg "")  
  
64位要注意：参数通过寄存器传，通常需要 pop rdi; ret  
 这种 gadget 来把 /bin/sh  
 地址放到 rdi。  
### 打法 B：直接 shellcode（NX off）  
  
如果 checksec 显示 **NX disabled**  
，可以更粗暴：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmtUgH3HxPSzCMwL8gpsoaGkLKw28GSWfhL6nNzN5UtYNL5mAnJV1gFw/640?wx_fmt=png&from=appmsg "")  
## 七、真题风格案例：一个“标准栈溢出题”的完整 exp  
  
题目特征（常见）：  
- 32位，NX on，Canary off，PIE off  
  
- 有 gets(buf)  
，buf 大小 0x20  
  
- 目标：远程 getshell 或读 /flag  
  
exp（你只需要改 offset / 地址 / 交互提示）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralabfv1111Tgjkyyujc3adEmOTiakNdibc192gPaXgckLhWiab8I2aOUHTickgz4vGwJd6VZzaTIibS6AiaQ/640?wx_fmt=png&from=appmsg "")  
## 八、Pwn 新手最常踩的 5 个坑  
1. **没开 context.arch**  
：导致 shellcode / packing 不对  
  
1. **offset 算错**  
：最常见，eip 没被完全覆盖  
  
1. **64位没处理 rdi**  
：直接填 system+binsh 会失败  
  
1. **远程和本地 libc 不一样**  
：本地能打远程打不了 → 要指定远程 libc 或泄露  
  
1. **交互提示写错**  
：sendlineafter  
 没匹配到，payload 发早了  
  
## 九、福利+互动  
  
如果你觉得 Pwn 难，其实是“工具不熟 + 调试不会”。  
  
**200节攻防教程，限免领！**  
  
想要的兄弟，**关注我+在后台发“学习”**  
，直接免费分享！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/iaLzURuoralZUcyibzTLGp4atVEPv7Sr2D42yzEXeFfia8MicHTdicPuTgf4UwFcHgBpXViay9ibjA5EmwLA0kRILKCUg/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=17 "")  
  
咱学漏洞挖掘和CTF，光看文章不够，这套教程里全是**实战演示**  
——从工具配置到漏洞利用，每一步都手把手教，跟着练就能上手！  
  
（注：资源领取入口在公众号后台，关注后发“学习”自动弹链接）  
  
  
