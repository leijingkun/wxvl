#  CTF Pwn模块系列分享（三）：溢出基础与ret2text漏洞利用  
原创 龙哥网络安全  龙哥网络安全   2026-01-06 02:12  
  
今天咱们就如约进入Pwn实战的核心第一站——**栈溢出基础与ret2text漏洞利用**  
。  
  
这是新手第一次真正意义上构造payload攻击程序，也是Pwn入门的里程碑。今天的内容全程围绕实战展开，从什么是栈溢出到亲手构造payload拿到shell，每一步都写得明明白白，跟着操作就能成功！记住：栈溢出是Pwn最基础、最常考的漏洞，搞定它，你就跨进了Pwn的大门。  
## 一、先回顾+拆解：什么是栈溢出？  
  
结合上一期讲的栈帧结构，我们用通俗例子+核心原理再拆一遍栈溢出，确保所有人都懂：  
### 1. 通俗例子：栈溢出就像“杯子装太多水”  
  
程序给局部变量分配的栈空间，就像一个固定容量的杯子；用户输入的数据，就像往杯子里倒水。正常情况下，水（数据）不会超过杯子容量（栈空间）；但如果程序没检查输入长度，你倒了太多水，水就会溢出来，流到杯子外面（覆盖栈里的其他数据）——这就是“栈溢出”。  
### 2. 核心原理：覆盖“返回地址”，控制程序执行流  
  
结合栈帧结构，栈溢出的核心逻辑是：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT5TIcJSgVeSULQaJXRgxny4gGGiawBg0pVNtOZwjIVicib05hlmvY24kCxCLdtjBlJmwhDcT6RfO3vgA/640?wx_fmt=png&from=appmsg "")  
  
今天我们学的“ret2text”，就是“return to text（返回文本段）”——把返回地址改成程序自带的、位于代码段（.text）的“后门函数地址”（比如执行/bin/sh的函数），让程序执行后门拿到shell。  
  
小提醒：ret2text是最基础的栈溢出利用方式，程序通常是“无保护”的（关闭栈保护、关闭PIE等），适合新手入门。  
  
## 二、实战准备：环境&工具&测试程序  
  
我们用“自定义简单漏洞程序”实战（和CTF比赛中的入门栈溢题逻辑完全一致），先准备好这些：  
### 1. 环境：延续上一期的Ubuntu 20.04+GDB+pwntools+IDA  
  
如果之前装了pwndbg（GDB插件），用它调试更友好；没装也没关系，用原生GDB也能完成。  
### 2. 编写漏洞程序（保存为pwn1.c）  
###   
  
![](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT5TIcJSgVeSULQaJXRgxny4icz4toapHFiaUcwNdWwZNx365cQp0HSHnldtMmwIHGPtyG5PLG7XXDVQ/640?wx_fmt=png&from=appmsg "")  
  
关键说明：   
  
- gets(buf)是“危险函数”，不检查输入长度，这是栈溢出漏洞的根源；   
  
- backdoor()是后门函数，执行system("/bin/sh")，调用后就能拿到shell；   
  
- 我们的目标：通过栈溢出，让程序执行backdoor()而不是正常返回main。  
### 3. 编译程序（关闭所有保护，方便新手调试）  
  
  
终端执行命令（复制直接用）：  
  
gcc -g -fno-stack-protector -z execstack -no-pie -o pwn1 pwn1.c  
  
参数解释：   
  
- -fno-stack-protector：关闭栈保护（避免canary检测，新手先避开）；   
  
- -z execstack：允许栈执行（后续进阶会讲，现在关闭减少干扰）；   
  
- -no-pie：关闭PIE（程序地址随机化，关闭后函数地址固定，方便我们找）；   
  
- -g：添加调试信息，方便GDB调试。  
## 三、实战步骤：手把手教你构造payload，拿到shell  
  
整个解题流程分4步：**找漏洞函数→找后门地址→算溢出偏移→构造payload攻击**  
，一步都不能少！  
### 第一步：用IDA分析程序，找漏洞函数和后门地址  
  
IDA是反汇编工具，帮我们快速定位漏洞和关键函数地址：  
1. 打开IDA，把编译好的pwn1拖进去，选择“64-bit”（x86_64架构），点击“OK”。  
  
1. 等待IDA分析完成，点击左侧“Functions”窗口，找到三个函数：main、vulnerable_function、backdoor。  
  
1. 确认漏洞：双击vulnerable_function，查看反汇编代码，能看到调用了gets函数（红色标注，危险函数），确认存在栈溢出。  
  
1. 找backdoor地址：双击backdoor函数，顶部会显示函数的虚拟地址（比如0x401146，每个人的地址可能一样，以自己IDA显示的为准），记下来！这是我们要跳转的目标地址。  
  
  
小技巧：在IDA中按“F5”可以查看伪代码（更直观），确认backdoor函数调用了system("/bin/sh")。  
### 第二步：用GDB找“溢出偏移”（核心！确定要填多少垃圾数据）  
  
“偏移”就是：从buf的起始地址，到“返回地址”的字节数——我们需要先填这么多“垃圾数据”（比如A），才能覆盖到返回地址。找偏移用“cyclic模式”最方便：  
1. 启动GDB调试程序：gdb ./pwn1  
  
1. 在vulnerable_function的gets函数处下断点（确保输入后程序暂停）：   
  
① 先找gets的调用地址：输入b vulnerable_function  
，然后r  
，程序停在vulnerable_function开头。  
  
② 输入ni  
（单步执行），直到看到“call gets@plt”（调用gets函数），再输入b *$rip  
（在当前指令下断点）。  
  
③ 输入c  
（继续执行），程序等待输入。  
  
1. 生成cyclic pattern（特殊的重复字符串，用来定位溢出位置）： 在GDB中输入cyclic 100  
，会生成100个字符的pattern（比如aaaaabaaaaac...），复制这个字符串。  
  
1. 粘贴pattern作为输入，程序会崩溃（因为栈溢出）。  
  
1. 查看rsp寄存器的值（栈顶，此时栈顶是被覆盖的返回地址）： 输入x/wx $rsp  
，会显示一个值（比如0x62616167，这是pattern对应的十六进制）。  
  
1. 计算偏移：输入cyclic -l 0x62616167  
（把刚才的十六进制值填进去），GDB会输出一个数字（比如24）——这就是偏移！意味着我们需要先填24个垃圾数据，才能覆盖到返回地址。  
  
  
小提醒：如果用pwndbg，步骤更简单：直接cyclic 100  
生成pattern，输入后程序崩溃，pwndbg会自动显示“cyclic shift: 24”，直接拿到偏移。  
### 第三步：构造payload  
  
payload的结构很简单（x86_64架构，每个地址占8字节）：**payload = 垃圾数据（偏移字节数） + 后门函数地址（8字节）**  
  
结合我们的实战数据（假设偏移24，backdoor地址0x401146），用Python构造：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT5TIcJSgVeSULQaJXRgxny4GtmU0SxdbQ7rtReM3v8gco56icXraxF0pKjcb2drJlh3TZgLaGh0pUQ/640?wx_fmt=png&from=appmsg "")  
  
  
关键说明：   
  
- b'A'：把字符串转成字节流（pwntools要求）；   
  
- p64()：把十六进制地址转成64位的小端字节序（x86_64架构默认小端，必须用p64，不能直接写地址字符串）。  
### 第四步：发送payload，拿到shell！  
  
有两种方式发送payload，新手先学第一种（简单直观）：  
#### 方式1：GDB中测试（确认漏洞利用成功）  
1. 重新启动GDB：gdb ./pwn1  
  
1. 输入r < payload  
（把payload文件作为输入发送给程序）；  
  
1. 程序执行后，会出现“$”提示符——这就是shell！成功了！  
  
1. 输入ls  
查看文件，输入cat flag  
（如果有flag文件）就能拿到Flag。  
  
  
#### 方式2：pwntools脚本自动化攻击（比赛常用）  
  
编写完整脚本（保存为exp.py）：  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT5TIcJSgVeSULQaJXRgxny4M8nLZWZBiamDVdCzFZfLP4vn0bryBTF8y5lTIqwzTZqDAGhibr4ib0Y6Q/640?wx_fmt=png&from=appmsg "")  
  
终端执行脚本：python3 exp.py  
  
执行后直接拿到shell，输入cat flag  
即可获取Flag——这就是CTF比赛中Pwn题的标准解题流程！  
## 四、新手避坑：这5个问题最容易卡壳！  
- 偏移算错：如果发送payload后程序没拿到shell，大概率是偏移错了——重新用cyclic找，确保断点下在gets调用后，输入pattern后程序崩溃。  
  
- 地址用错：一定要用IDA里看到的backdoor地址，不能抄我的（虽然大概率一样，但要确认）。  
  
- 没加p64：直接把地址写成字符串（比如'0x401146'），程序肯定执行失败——必须用p64转成64位字节流。  
  
- 程序有保护：如果编译时没加“-fno-stack-protector -no-pie”，程序会有保护，导致溢出失败——新手先关闭所有保护练手。  
  
- GDB调试卡壳：如果不会下断点，直接用pwndbg的b gets  
（在gets函数处下断点），然后r  
，输入pattern即可。  
  
  
## 五、下期预告  
  
今天我们完成了第一次Pwn实战，亲手用栈溢出+ret2text拿到了shell——这是Pwn入门的关键一步！下期我们将进阶：**栈溢出进阶之ROP链构造**  
，解决“程序没有后门函数”的问题，学习如何拼接代码段中的指令，构造ROP链执行system("/bin/sh")。  
  
如果今天的内容对你有帮助，别忘了点赞、在看，转发给一起学CTF的小伙伴～   
  
  
全套CTF学习资源，也可以在下面蓝色链接拿!  
  
[CTF学习资源，限时免费领取](https://mp.weixin.qq.com/s?__biz=MzU3MjczNzA1Ng==&mid=2247499981&idx=2&sn=d893464c375b2d1707df74946d0de8fa&scene=21#wechat_redirect)  
  
  
想要的兄弟，关注我发送**CTF入门**  
，直接免费分享！前提是你得沉下心练，别拿了资料就吃灰，咱学技术，贵在坚持！  
  
给大家准备了2套关于CTF的教程，一套是涵盖多个知识点的专题视频教程：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT7xsvZnb2HBQYSQMZklxT1wPF9YAVWAiaEJlYzA5dBZoicHH8VdVic5KLB6TCicSA4mic99h1lbIQEezQw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=0 "")  
  
另一套是大佬们多年征战CTF赛事的实战经验，也是视频教程：  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/7O8nPRxfRT7xsvZnb2HBQYSQMZklxT1w5nE7Z7G7m1Ig8EoWz5fG0Xd9KTGHkuMiclIqohj5MKf6ibjUXaPGdlow/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=1 "")  
  
可以截图或者长按识别、扫码添加找我拿  
  
**龙哥网络安全**  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_jpg/7O8nPRxfRT6lk3oXDrx8qaZiaMDS5XHTAUZJyk54VZ9YhKV0EQEpCETOEEibrhIBGbJgGS8o1ZqGweicPrIcMh5LQ/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=5 "")  
  
  
**扫码添加领取**  
  
  
**点击蓝字**  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/ibxqJibmt337FAiaWRcQtUgyiak5dz81n37puYvXjff5AofqGAkjClzzyg4jcUgDucuKloOlGmF8ibYqYQeNHecpezA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=6 "")  
  
**关注我**  
  
[#计算机]()  
[#计算机网安]()  
[#网络安全]()  
[#渗透测试]()  
[#CTF]()  
[#CTF比赛]()  
[#赛事]()  
[#计算机专业大学规划]()  
[#网安零基础怎么入学]()  
[#大学生]()  
  
  
