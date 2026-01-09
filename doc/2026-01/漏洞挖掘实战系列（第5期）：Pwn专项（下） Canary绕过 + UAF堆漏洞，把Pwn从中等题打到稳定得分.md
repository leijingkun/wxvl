#  漏洞挖掘实战系列（第5期）：Pwn专项（下） Canary绕过 + UAF堆漏洞，把Pwn从中等题打到稳定得分  
原创 点击关注👉  网络安全学习室   2026-01-09 02:33  
  
上一期我们把“无保护栈溢出”讲透了，但真正的CTF Pwn题，保护机制越来越多：**Canary、PIE、NX**  
 基本是标配。  
  
这一期我带你突破两个关键门槛：  
1. **Canary 绕过（泄露 + 还原）**  
  
1. **UAF 堆漏洞（最常见的堆题入门点）**  
  
只要掌握这两个，你就能稳定拿下 Pwn 中等题，不再被保护机制劝退。  
## 一、Canary 是什么？为什么能防栈溢出？  
  
Canary 是 GCC 加的一种栈保护机制：  
- 在栈上 buf 和 return address 之间插入一个随机值  
  
- 函数返回前会检查这个值是否被改变  
  
- 被改了就直接 crash（stack smashing detected）  
  
所以你不能像之前那样直接覆盖 return address。  
  
**绕过思路只有一个：先泄露 Canary，再原样写回去。**  
## 二、Canary 绕过实战：泄露 Canary + 精准覆盖  
  
下面用一个典型真题结构演示（CTF 2025 常见）：  
  
程序逻辑类似：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6QMRW9nhAw0KibUQUniarEia6Fs71icms1cvBXbs0wymxjDxGyLQGa1tZNg/640?wx_fmt=png&from=appmsg "")  
  
注意两点：  
- read  
 读太多 → 可能溢出  
  
- printf  
 直接输出 buf → 可能泄露 Canary  
  
### 1）泄露 Canary（关键步骤）  
  
Canary 的特点：  
- 最后一个字节是 **0x00**  
（用来截断字符串）  
  
- 前面 7 字节随机（64位）  
  
利用 printf 泄露：  
  
构造输入：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6vYfbmXVcDPPRiaGLAicahvt0p2BciaC6fGdIPWMsHNaX3tPGBaC2ibvLwg/640?wx_fmt=png&from=appmsg "")  
  
如果 buf 大小是 0x40，那么 printf 输出时会把 buf 后面的内容一起打出来，其中就包含 **Canary**  
。  
  
泄露后你会得到类似：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6hESYia36f2SJwDNSQIgEaIXIF5Q2rpGCL9LlwsfJRckmGYGU7F5bMOg/640?wx_fmt=png&from=appmsg "")  
  
Canary 就是那个以 **00**  
 结尾的值。  
### 2）还原 Canary，构造 payload  
  
64位栈布局一般是：  
  
buf → Canary → RBP → return address  
  
所以 payload 结构是：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6MllFJibNHK3yyg042iaiaat8jwKWV7h7oMXCbD0RaKgjAI2icdSiaMavOuA/640?wx_fmt=png&from=appmsg "")  
  
你只需要把泄露到的 Canary 原样写回去，就能绕过检测。  
### 3）完整 exp（64位 Canary 绕过模板）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6H9GRmC8JOcOFPQGSVCE0FiatKIjpDJexYVJibYb3gNf8KzjWuGw6HZKQ/640?wx_fmt=png&from=appmsg "")  
  
记住：Canary 题 90% 都是“泄露 + 还原”，套路固定。  
## 三、堆漏洞入门：UAF（Use After Free）最容易拿分  
  
堆题比栈题更灵活，但 **UAF 是最适合入门的堆漏洞**  
，因为：  
- 原理简单：释放后没清空指针，仍然被使用  
  
- 利用简单：劫持指针 → 控制程序流程  
  
- CTF 出现频率极高（几乎每场都有）  
  
### 1）UAF 原理一句话  
  
程序执行顺序如下：  
1. malloc → 分配一块堆内存  
  
1. free → 释放这块内存（但指针没置空）  
  
1. 继续使用这块指针 → 访问已经被释放的内存 → UAF  
  
你要做的就是：  
- 让这块内存被重新分配到你可控的位置  
  
- 然后通过原指针执行你布置的内容  
  
## 四、UAF 实战：劫持函数指针（最常见打法）  
  
下面是 CTF 真题中最常见的 UAF 结构：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6h3IaCf27U4iaR39etYauWBemUVIuic1xR8uwQc6lycXdsQRvwgX3E8BA/640?wx_fmt=png&from=appmsg "")  
  
漏洞点：func3()  
 调用了已经被 free 的 u->print  
。  
### 利用思路  
1. 调用 func1 → 分配 User  
  
1. 调用 func2 → free(u)  
  
1. 再次 malloc 一块同样大小的内存 → 这块内存会被分配到刚才 freed 的位置  
  
1. 你控制这块内存的内容 → 把 print 指针改成 system  
  
1. 调用 func3 → 执行 system("/bin/sh")  
  
### 完整 exp（UAF 经典模板）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/iaLzURuoralatJBL1icQTSaib9bZknZWKn6dUQw1Hf4zoptGEP2ADJbOjyqPnyO8msZL9fOAhQe1hjTd33ib47LOUg/640?wx_fmt=png&from=appmsg "")  
  
这就是 UAF 的核心套路：**free 后重新 malloc，把指针劫持成你想要的函数地址。**  
## 五、堆题新手最常踩的 4 个坑  
1. **没对齐堆块大小**  
：malloc(0x20) 和 malloc(0x18) 分配的 chunk 不一样  
  
1. **没注意 tcache**  
：新版本 glibc 会优先从 tcache 分配  
  
1. **没控制好指针**  
：UAF 必须保证指针还能被程序访问  
  
1. **没处理 libc 版本差异**  
：不同版本堆结构不同，exp 要微调  
  
## 六、福利+互动  
  
如果你觉得堆题难，其实是因为你没掌握“堆块生命周期”。  
  
**200节攻防教程，限免领！**  
  
想要的兄弟，**关注我+在后台发“学习”**  
，直接免费分享！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/iaLzURuoralabfv1111Tgjkyyujc3adEmm2OrDeGEQaKib25ERG3fddoNA8lz87MtKVHwNLliaG5cdqGyabh0ibb0w/640?wx_fmt=jpeg&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=17 "")  
  
咱学漏洞挖掘和CTF，光看文章不够，这套教程里全是**实战演示**  
——从工具配置到漏洞利用，每一步都手把手教，跟着练就能上手！  
  
（注：资源领取入口在公众号后台，关注后发“学习”自动弹链接）  
  
  
