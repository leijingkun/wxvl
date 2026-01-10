#  漏洞挖掘实战系列（第6期）：Pwn高阶实战 格式化字符串+ROP进阶，拿下CTF高分题  
原创 点击关注👉  网络安全学习室   2026-01-10 03:02  
  
经过前两期的铺垫，你已经能搞定Pwn基础题和中等题，但想冲击CTF高分，必须突破**高阶漏洞组合利用**  
——格式化字符串漏洞（任意读/写）+ ROP进阶（复杂链构造），这两类题是拉开分差的关键，也是2025 CTF Pwn模块的高频压轴题。  
  
本期全程用真题逻辑拆解，从漏洞识别到exp编写，教你“组合拳”思路，让你能应对复杂保护机制（全保护开启：Canary+PIE+NX）的场景。  
## 一、格式化字符串漏洞：不止是泄露，还能任意写  
  
格式化字符串漏洞（Format String Vulnerability）的核心是**格式化参数可控**  
，比如printf(buf)  
而非printf("%s", buf)  
。它的威力远不止泄露数据，还能实现**任意地址读/写**  
，直接绕过多种保护。  
### 1. 漏洞识别：3个快速测试方法  
  
拿到程序后，找printf  
/fprintf  
/sprintf  
等函数，输入以下Payload测试：  
- 输入%p.%p.%p.%p  
：观察输出是否有栈上的内存地址（如0x401234  
）；  
  
- 输入%x.%x.%x  
：观察是否输出栈上的十六进制数据；  
  
- 输入%n  
：若程序崩溃或异常，大概率存在漏洞（%n  
会把已输出字符数写入指定地址）。  
  
典型漏洞代码：char buf[0x20]; read(0, buf, 0x100); printf(buf);  
### 2. 核心用法1：任意读（泄露关键地址）  
  
格式化字符串最常用的功能是**泄露栈上数据**  
，比如Canary、libc基址、程序基址（PIE开启时）。  
#### 真题场景：泄露Canary（全保护程序）  
  
假设程序开启Canary+PIE+NX，栈布局中Canary在第10个格式化参数位置（通过%p  
测试确定）：  
- 构造Payload：%10$p  
（$  
指定第n个参数）；  
  
- 执行后，程序会输出Canary的十六进制值（如0x00123456789abcde  
）；  
  
- 同理，泄露libc基址：找到栈上libc函数的返回地址（如puts  
的got表地址），用%x  
读取后计算偏移。  
  
#### 实操代码片段（泄露Canary）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralYwBsrZGLoKLCfUW1Pia7HbovlttibylMR9KHDPbBRHN4bkLgojvuIKoxoCVT9YlaestlzXTO6BZWVA/640?wx_fmt=png&from=appmsg "")  
### 3. 核心用法2：任意写（篡改关键数据）  
  
利用%n  
系列格式化符（%n  
/%hn  
/%hhn  
）实现任意地址写，比如：  
- 篡改got  
表（全局偏移表）：把puts  
的got表地址改成system  
地址；  
  
- 关闭保护：篡改程序的标志位（如关闭PIE）；  
  
- 直接写flag路径：把/flag  
写入指定内存地址。  
  
#### 真题实战：篡改got表getshell（64位程序）  
  
场景：程序puts  
的got表地址为0x404018  
，想把它改成system  
地址（0x7f1234567890  
）：  
1. 拆分system  
地址（64位地址分两次写，避免溢出）：  
  
1. 低32位：0x34567890  
  
1. 高32位：0x7f12  
  
1. 构造Payload（用%hn  
写2字节，$  
指定写入地址）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralYwBsrZGLoKLCfUW1Pia7HboUAiapm1JfyXg9OelN5iafwLRyLfl2T6pUa70ubXRgKKgjWqeHDBUXyHw/640?wx_fmt=png&from=appmsg "")  
  
1. 发送Payload后，puts  
的got表被篡改，下次调用puts  
时会执行system  
，再传入/bin/sh  
即可getshell。  
  
### 4. 避坑点：  
- 地址对齐：64位程序中，写入的地址必须是8字节对齐，否则会崩溃；  
  
- 字符数计算：%n  
统计的是已输出字符数，需精准计算payload长度和填充字符数；  
  
- 小端序：x86/x64是小端序，多字节写入时要注意地址顺序。  
  
## 二、ROP进阶：复杂链构造（突破全保护）  
  
当程序开启Canary+PIE+NX时，简单的ret2libc已经不够用，需要构造**复杂ROP链**  
，实现“泄露地址→绕过保护→getshell”的完整流程。  
### 1. ROP进阶核心：找gadget组合  
  
ROP的本质是“拼接内存中的指令片段（gadget）”，实现指定功能。进阶场景下，需要组合以下gadget：  
- 寄存器操控gadget：pop rdi; ret  
（给rdi传参）、pop rsi; pop r15; ret  
（给rsi传参）；  
  
- 内存读写gadget：mov qword ptr [rdi], rsi; ret  
（把rsi的值写入rdi指向的地址）；  
  
- 系统调用gadget：syscall; ret  
（触发Linux系统调用，如execve("/bin/sh")）。  
  
#### 工具：快速找gadget  
  
用ROPgadget  
或ropper  
工具提取gadget：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralYwBsrZGLoKLCfUW1Pia7Hbo8hpDyVT4ty5rWuaQJy0W7KSWFqhOjUM7xOSKllD1Amu5q1tIiaj5SbQ/640?wx_fmt=png&from=appmsg "")  
### 2. 真题实战：全保护程序ROP链构造  
  
场景：程序开启Canary+PIE+NX，无/bin/sh  
字符串，需通过ROP链实现：  
1. 泄露libc基址（用puts输出got表地址）；  
  
1. 构造execve("/bin/sh", NULL, NULL)  
系统调用。  
  
#### 完整ROP链步骤：  
1. 泄露libc基址：  
  
1. 找puts  
的plt表地址（elf.plt["puts"]  
）；  
  
1. 找puts  
的got表地址（elf.got["puts"]  
）；  
  
1. 构造链：pop rdi; ret  
 + puts_got  
 + puts_plt  
 + main  
（泄露后回到main，继续执行）。  
  
1. 计算system和/bin/sh地址：  
  
1. system_addr = libc_base + libc.sym["system"]  
；  
  
1. binsh_addr = libc_base + next(libc.search(b"/bin/sh"))  
。  
  
1. 构造getshell链：  
  
1. pop rdi; ret  
 + binsh_addr  
 + system_addr  
。  
  
#### 完整exp（关键片段）：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralYwBsrZGLoKLCfUW1Pia7Hboz7oVZBSziaZB6ibK1rAnW8hYng0KQlzD89BLJnjr2s7sicrNia76wSE35Q/640?wx_fmt=png&from=appmsg "")  
### 3. ROP进阶技巧：  
- 利用libc  
中的gadget：当程序中gadget不足时，用ROPgadget --file ./libc.so.6 --search "pop ret"  
提取libc中的gadget；  
  
- 栈迁移（Stack Pivoting）：当栈空间不足时，把栈迁移到堆或其他可写内存区域；  
  
- 多步ROP：分阶段实现功能（如先泄露地址，再构造shell）。  
  
## 三、高阶组合拳：格式化字符串+ROP 综合利用  
  
在真实CTF题中，很少单独考一个漏洞，更多是“多个漏洞组合”，比如：  
1. 用格式化字符串漏洞泄露Canary和libc基址；  
  
1. 用栈溢出（已绕过Canary）构造ROP链；  
  
1. 调用system("/bin/sh") getshell。  
  
#### 核心逻辑：  
- 格式化字符串负责“破保护”（泄露关键地址）；  
  
- ROP负责“拿权限”（构造执行流程）；  
  
- 两者结合，可突破几乎所有保护机制。  
  
## 四、Pwn高阶避坑清单  
1. **libc版本不匹配**  
：本地和远程libc版本不同，gadget地址和函数偏移会变→必须泄露远程libc基址，或用libc-database匹配；  
  
1. **PIE地址计算错误**  
：程序开启PIE后，所有地址都要加基址→先泄露程序基址（如main函数地址）；  
  
1. **gadget地址错误**  
：找gadget时要注意程序位数（32/64位），64位gadget需匹配寄存器使用规则；  
  
1. **Payload长度溢出**  
：格式化字符串Payload过长会触发栈溢出，需控制长度或分块发送。  
  
## 五、福利+互动  
  
Pwn高阶题的核心是“思路拆解+工具熟练”，光看理论没用，必须跟着实操练。  
  
**200节攻防教程，限时领！**  
  
想要的兄弟，**关注我+在后台发“学习”**  
，直接免费分享！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/iaLzURuoralatJBL1icQTSaib9bZknZWKn6RRwkKk9gxxm1XMjJqJtAPJf7zEBAIZU8gicMxsJuPUTE0bmFK8cO05g/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=17 "")  
  
咱学漏洞挖掘和CTF，光看文章不够，这套教程里全是**实战演示**  
——从工具配置到漏洞利用，每一步都手把手教，跟着练就能上手！  
  
（注：资源领取入口在公众号后台，关注后发“学习”自动弹链接）  
  
### 系列总结  
  
从基础漏洞挖掘到Web专项，再到Pwn从入门到高阶，本系列覆盖了CTF漏洞挖掘的核心场景。记住：漏洞挖掘的本质是“理解程序逻辑+打破信任边界”，多练真题、多调试，把每个漏洞的套路内化为肌肉记忆，你就能在CTF中稳定拿分！  
  
如果想补充某类漏洞的细节（如物联网Pwn、内核漏洞），或需要更多真题exp模板，评论区留言告诉我！  
  
