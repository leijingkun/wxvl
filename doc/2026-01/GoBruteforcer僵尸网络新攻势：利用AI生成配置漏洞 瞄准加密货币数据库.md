#  GoBruteforcer僵尸网络新攻势：利用AI生成配置漏洞 瞄准加密货币数据库  
 嘶吼专业版   2026-01-19 06:00  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
  
GoBruteforcer（又名GoBrut）僵尸网络发起新一轮恶意攻击，目标直指暴露在公网的服务器，特别是那些疑似使用AI生成示例配置的加密货币及区块链项目数据库。  
  
该恶意软件基于Golang编写，通常针对暴露的FTP、MySQL、PostgreSQL及phpMyAdmin服务发动攻击。它往往利用已沦陷的Linux服务器扫描随机公网IP，并实施暴力破解登录。  
# 利用薄弱防御实施入侵  
  
研究人员估计，目前互联网上可能有超过5万台服务器易受GoBrut攻击。攻击者通常通过运行XAMPP的服务器上的FTP服务获取初始访问权限。这是因为除非管理员手动进行安全配置，否则默认配置往往包含极易被破解的弱口令。  
  
当攻击者利用标准账户（通常为daemon或nobody）和弱默认密码成功登录XAMPP FTP后，典型的下一步操作是将Web后门（Web Shell）上传至网站根目录。  
  
攻击者也可能通过其他途径上传Web后门，例如配置不当的MySQL服务器或phpMyAdmin面板。感染链条随后继续延伸，通过下载器获取IRC僵尸程序及暴力破解模块。  
  
恶意软件在延迟10至400秒后启动，在x86_64架构上可启动多达95个暴力破解线程，扫描随机公网IP段，但会自动跳过私有网络、AWS云服务段及美国政府网络。  
  
每个工作线程生成一个随机公网IPv4地址，探测相关服务端口，尝试提供的凭证列表，随后退出。系统会持续生成新的工作线程以维持设定的并发级别。  
  
FTP模块依赖于直接嵌入在二进制文件中的22组硬编码用户名密码对。这些凭证与XAMPP等虚拟主机套件中的默认或常见部署账户高度吻合。  
# AI生成配置与老旧套件助长攻势  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o290yVoiboKLLQeN3l8UBfAfIqLqAdJaoH0ywARty6XD8ay2cKxT4AGkOvFOoRNgGhic54OW7McKQb5w/640?wx_fmt=png&from=appmsg "")  
  
GoBruteforcer的感染链  
  
研究人员指出，在近期的攻击活动中，GoBruteforcer的活跃度之所以激增，是因为大量服务器复用了由大语言模型（LLM）生成的通用配置片段。这导致了大量弱口令和可预测的默认用户名（如appuser、myuser、operator）的泛滥。   
  
这些用户名频繁出现在AI生成的Docker和DevOps操作指南中，研究人员据此推测，这些配置已被应用到真实系统中，从而使其极易遭受密码喷洒攻击。   
  
助长该僵尸网络近期攻势的第二个原因是过时的服务器套件（如XAMPP）。这些套件在部署时仍保留默认凭证和开放的FTP服务，暴露了脆弱的网站根目录，使攻击者能够轻松植入Web后门。  
  
一起典型的攻击案例是一台受感染的主机被植入了TRON钱包扫描工具，该工具会对TRON和币安智能链（BSC）进行全网扫描。攻击者使用一个包含约2.3万个TRON地址的文件，通过自动化工具识别并掏空余额非零的钱包。   
  
为防御GoBruteforcer攻击，管理员应避免使用AI生成的部署指南，并使用非默认用户名搭配强密码。  
  
同时，及时检查FTP、phpMyAdmin、MySQL和PostgreSQL是否存在暴露的服务，并考虑将XAMPP等过时软件套件替换为更安全的替代方案。  
  
参考及来  
源：  
https://www.bleepingcomputer.com/news/security/new-gobruteforcer-attack-wave-targets-crypto-blockchain-projects/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o290yVoiboKLLQeN3l8UBfAfINnln50ZmkGktmh13mnyZ4BiaJY8icfiaOfBycfqHf81kZQqURpG8ibsogA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wpkib3J60o290yVoiboKLLQeN3l8UBfAfIhtTydvGgcE3jsvaUDvzKo9AeL6Dh8kOp1AZFc8WvuhZxzNByibtzUAA/640?wx_fmt=png&from=appmsg "")  
  
  
