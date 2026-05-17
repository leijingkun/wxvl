#  【安全圈】黑客利用 Burst Statistics WordPress 插件认证绕过漏洞发动攻击  
 安全圈   2026-05-17 11:00  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
供应链攻击  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/sbq02iadgfyFlymRRb9HycricBceS7a9TmB4xSnuicAoNPYvfHwiaZBpNnPX4omKn8FDuT0fibrI5ZYTbZCBYPiaPEo22ytAxLHEQx5WZJPAmfRg4/640?wx_fmt=png&from=appmsg "")  
  
OpenAI 表示，在近期影响数百个 npm 和 PyPI 软件包的 TanStack 供应链攻击中，两名员工的设备遭到入侵。作为预防措施，该公司已轮换其应用程序的代码签名证书。  
  
在今日发布的安全公告中，OpenAI 称此次事件未影响客户数据、生产系统、知识产权或已部署的软件。  
  
该公司表示，此次漏洞与 TeamPCP 勒索团伙近期发起的 “Mini Shai - Hulud” 供应链攻击活动有关，该活动通过在受信任的热门软件包中植入恶意更新，将开发者作为攻击目标。  
  
OpenAI 解释称：“我们观察到的活动与该恶意软件公开描述的行为一致，包括对两名受影响员工有权访问的部分内部源代码存储库进行未经授权的访问和以窃取凭证为目的的数据渗出活动。”  
  
OpenAI 表示，在此次攻击中，仅从存储库中窃取了有限的凭证，且没有证据表明这些凭证被用于其他攻击。  
  
OpenAI 称已隔离受影响的系统和账户、撤销会话、轮换受影响存储库中的凭证，并暂时限制部署工作流程。该公司还在第三方事件响应公司的帮助下进行了取证调查。  
  
用于 OpenAI macOS、Windows、iOS 和 Android 产品的代码签名证书在此次事件中也被泄露。尽管 OpenAI 尚未检测到这些证书被滥用于签署恶意软件，但作为预防措施，公司正在轮换这些证书。  
  
此次轮换意味着 macOS 用户需在 2026 年 6 月 12 日前更新 OpenAI 桌面应用程序，因为由于苹果的公证流程，使用旧证书签名的应用程序可能无法启动或接收更新。  
  
Windows 和 iOS 用户不受影响，无需采取任何行动。  
### TanStack 供应链攻击事件  
  
OpenAI 的此次漏洞是 “Mini Shai - Hulud” 大规模软件供应链攻击活动的一部分，本周早些时候，该活动致使数百个 npm 和 PyPI 软件包遭到入侵。  
  
此次攻击最初针对 TanStack 和 Mistral AI 的软件包，随后通过窃取的 CI/CD 凭证和合法工作流程蔓延至其他项目，包括 UiPath、Guardrails AI 和 OpenSearch。  
  
Socket 和 Aikido 的研究人员最终追踪到通过合法软件包存储库分发的数百个受感染软件包。  
  
根据 TanStack 的事后分析，攻击者利用该项目 GitHub Actions 工作流程和 CI/CD 配置中的弱点执行恶意代码、从内存中提取令牌，并通过 TanStack 的正常发布管道发布恶意软件包。  
  
这使得攻击者能够通过合法发布直接发布恶意软件包版本，这些软件包看起来是合法的。  
  
在此次活动中传播的 “Mini Shai - Hulud” 恶意软件旨在窃取开发者和云凭证，包括 GitHub 令牌、npm 发布令牌、AWS 凭证、Kubernetes 机密信息、SSH 密钥和.env 文件。  
  
安全研究人员表示，该恶意软件还通过修改 Claude Code 钩子和 VS Code 自动运行任务，在开发者系统上实现持久化，即便软件包被移除，它仍能留存。  
  
该恶意软件利用窃取的 GitHub 和 npm 凭证入侵维护者账户，将恶意有效载荷注入软件包压缩文件，并将新的植入木马的软件包版本发布到存储库，从而传播到其他项目。  
  
微软威胁情报部门还报告称，该恶意软件启动了一个针对运行俄语软件系统的 Linux 信息窃取工具。该恶意软件还包含一个破坏性的破坏组件，会在一些以色列或伊朗系统上随机执行递归擦除命令。  
  
OpenAI 表示，此次事件反映了攻击者越来越倾向于针对软件供应链而非直接攻击个别公司，以造成更广泛的影响。  
  
该公司总结道：“现代软件构建于一个开源库、软件包管理器以及持续集成和持续部署基础设施紧密相连的生态系统之上，这意味着上游引入的漏洞能够在各个组织中迅速广泛传播。”  
  
  
  END    
  
  
阅读推荐  
  
  
[【安全圈】Linux内核漏洞"ssh-keysign-pwn"允许攻击者窃取SSH密钥与影子密码文件](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652076524&idx=1&sn=85724f6aa9c493a3823cc2988e73c7ec&scene=21#wechat_redirect)  
  
  
  
[【安全圈】微软 Edge 148 浏览器将增强安全，密码不再明文进入内存](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652076524&idx=2&sn=4bce93113ae71c271aa59e2f723b5d8f&scene=21#wechat_redirect)  
  
  
  
[【安全圈】微软Exchange Server高危漏洞正遭攻击者积极利用](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652076524&idx=3&sn=8adfa54adbe431abba673a75db6ad228&scene=21#wechat_redirect)  
  
  
  
[【安全圈】新型远程控制木马被披露，黑客伪造苹果与雅虎 CDN 域名攻击](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652076488&idx=1&sn=ffae0916c178fdfdfc63e5f205db23f8&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEDQIyPYpjfp0XDaaKjeaU6YdFae1iagIvFmFb4djeiahnUy2jBnxkMbaw/640?wx_fmt=png "")  
  
**安全圈**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
←扫码关注我们  
  
**网罗圈内热点 专注网络安全**  
  
**实时资讯一手掌握！**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
**好看你就分享 有用就点个赞**  
  
**支持「****安全圈」就点个三连吧！**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
  
