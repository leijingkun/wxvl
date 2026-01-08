#  jsPDF 高危漏洞可窃取服务器密钥及配置文件  
安融技术  安融技术   2026-01-08 03:36  
  
jsPDF  
是一个广泛应用于  
 Web   
及  
 Node.js   
环境的开源  
PDF   
生成库。该库由  
 Parallax   
团队开发维护，支持在浏览器端和服务器端生成  
 PDF   
文件，并提供丰富的  
 API   
用于添加文本、图像及字体等内容。  
jsPDF   
在前端开发和后端服务中被大量使用，常用于动态生成报表、发票和证书。由于该库的  
Node.js   
版本具备文件系统访问能力，可用于加载本地资源，该特性也成为了本次漏洞的攻击入口。  
  
jsPDF  
周下载量超  
350  
万次，影响范围广泛，建议所有开发者立刻排查。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCD6cYQSVMbvpocibt02fxPpwB9ZF1qxTKrYHIpQLXWCIoT5F9kPZPCnJt4yTEZwpYlJlI1YsndekqQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
漏洞概述：  
  
CVE-2025-68428   
是  
2026  
年  
1  
月披露的一个高危路径遍历漏洞，影响广受欢迎的  
JavaScript PDF  
生成库  
jsPDF  
。该漏洞  
CVSS v4.0  
评分高达  
 9.2  
，属于关键级别。  
  
漏洞类型：本地文件包含（  
LFI  
）  
+   
路径遍历（  
Path Traversal  
）  
  
披露时间：  
2026  
年  
1  
月  
3  
日  
  
影响范围：  
jsPDF  
版本  
   
< 4.0.0   
（主要为  
Node.js  
环境）  
  
漏洞原理：  
  
此漏洞源于  
 jsPDF (Node.js  
构建版  
)   
的部分方法（  
loadFile, addImage, html, addFont  
）未对用户传入的文件路径进行严格的过滤与校验  
。远程攻击者可构造包含路径遍历序列（如  
../  
）的恶意请求，绕过预期的资源目录限制，读取服务器上的敏感文件（如配置文件、密钥等）  
，并将文件内容嵌入到生成的  
 PDF   
文档中。由于该漏洞利用门槛较低，无需特殊权限或用户交互，因此具有较高的安全风险。  
  
攻击示例：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCD6cYQSVMbvpocibt02fxPpwEfepdqDdnjZSN53YREhZyxcOgKoqNffXhzOpCfeq65aG9dNmZZtQRw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
关键点：  
文件类型参数（如  
"JPEG"  
）不会阻止非图片文件的读取，库会读取任意指定文件并原样嵌入  
PDF  
。  
  
修复方案：  
  
1.   
立即升级（首选）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCD6cYQSVMbvpocibt02fxPpwgrykSWXZAKgOzibQfnyPVa5iblfjwzEtH4gvsnIn7libNWtGfHRwjn8UQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
注意：  
4.0.0  
版本默认限制文件系统访问，但不会引入其他破坏性变更。  
  
2. Node.js  
权限模式（  
Node   
≥  
 v22.13.0  
）  
  
对于无法立即升级的情况，使用  
--permission   
标志限制文件系统访问：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCD6cYQSVMbvpocibt02fxPpwoQCnT32xY9SaxiaibibQHAwGFy616Tw2NWmicWQulrzBV9DZQCytHaspmw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
3.   
输入验证（旧版本  
Node.js  
）  
  
如果无法升级  
Node.js  
版本，必须手动清洗用户输入：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/f0KlQiaibhCCD6cYQSVMbvpocibt02fxPpwUga2diaE5JPOJzcMIR0bn1tYm0hsTD3Kz4V5HJP4IkIU0pibkVMicADCA/640?wx_fmt=jpeg&from=appmsg "")  
  
