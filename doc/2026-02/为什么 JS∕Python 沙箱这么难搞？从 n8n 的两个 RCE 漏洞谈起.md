#  为什么 JS/Python 沙箱这么难搞？从 n8n 的两个 RCE 漏洞谈起  
原创 Hankzheng
                    Hankzheng  技术修道场   2026-02-02 23:55  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wWBwsDOJT4ib97eCYFf5iaDVXwxF4yfUIn308chat0SCADDRHibic24ico5EneOVjyEcCnfN6YJZYPXicWjpkaMBx6pg/640?wx_fmt=png&from=appmsg "")  
  
前两天和几个做架构的朋友撸串，聊起现在大火的 **AI 工作流自动化**  
。大家都在用 n8n 来串联各种 API 和 LLM，甚至有人开玩笑说：“只要 n8n 玩得好，下班回家早”。  
  
但作为在安全圈摸爬滚打多年的挨踢人，我当时就提了个醒：**这种把所有“重要信息”（API Key、数据库权限、IAM 凭证）都交给一个平台的做法，一旦被破防，那就是全线崩溃。**  
  
果不其然，这两天安全圈炸锅了。JFrog 研究团队刚披露了 n8n 的两个“重量级”漏洞，其中一个评分高达 **9.9**  
！这意味着什么？意味着黑客只要拿到一个普通账号，就能分分钟把你的整个 n8n 实例给“端”了。  
## 1. 为什么这次漏洞这么“毒”？  
  
大家可能觉得，需要“认证后”才能触发的漏洞没那么可怕。但你要知道，n8n 在企业内部通常连接着核心业务：销售数据、LLM API、甚至是内部权限管理系统（IAM）。  
> JFrog 的副总裁打了个很形象的比喻：这就像是黑客拿到了全公司的**“万能钥匙”**  
。即便是在内部执行模式下，一旦实现 RCE（远程代码执行），你的服务器就彻底成了人家的“肉鸡”。  
  
## 2. 硬核拆解：沙箱是怎么被“玩坏”的？  
  
这次出问题的核心在于 **沙箱逃逸（Sandbox Escape）**  
。咱们重点看看这两个高危漏洞的技术逻辑：  
### ● CVE-2026-1470 (CVSS 9.9)：JS 注入的“绝命毒师”  
  
n8n 允许用户在节点中使用 JavaScript 表达式。为了安全，官方设计了一套基于 **AST（抽象语法树）**  
 分析和白名单过滤的沙箱机制。  
- **实现难点：**  
 如何在允许用户写逻辑的同时，禁止他们调用 process  
 或 child_process  
？  
  
- **漏洞成因：**  
 黑客利用了一些极度冷门、甚至是被弃用的 JS 语法特性，配合 Node.js 运行时的异常处理机制，成功绕过了 AST 静态检查。  
  
- **后果：**  
 攻击者构造了一段“看起来很萌”的 JS 代码，让 n8n 误以为安全并执行，结果直接拿到了主进程权限。  
  
### ● CVE-2026-0863 (CVSS 8.5)：Python 执行器的“金蝉脱壳”  
  
n8n 的 Python 节点使用了一个名为 python-task-executor  
 的组件。  
- **技术点：**  
 这里涉及 **Eval 注入**  
。即便官方做了多层校验，但 Python 作为动态语言实在是太灵活了。  
  
- **破防手段：**  
 研究员发现，通过特定的解释器行为和底层的特殊变量访问，可以突破受限环境，直接在宿主操作系统上跑任意 Python 代码。  
  
## 3. 深度反思：沙箱防线为何难守？  
  
作为搞了多年后端的人，我也深有感触。研究员 Nathan Nehorai 说了一句很扎心的话：**“想要完全封锁 JS 和 Python 这种高阶动态语言的沙箱，简直是难如登天。”**  
  
即便你做了：  
**1. 多层校验**  
**2. 黑名单过滤**  
**3. AST 静态分析**  
  
但只要解释器更新了、或者某些边缘语法没处理干净，黑客就能通过**“嵌套、异常抛出、运行时反射”**  
等骚操作，把原本死掉的代码“复活”。这就是动态语言的魅力，也是安全官的噩梦。  
## 4. 你的 n8n 还在“裸奔”吗？  
  
最让人头大的是，截至 1 月底，全网竟然还有 **39,000 多个**  
 n8n 实例暴露在之前的漏洞威胁之下。如果你还在用旧版本，听哥一句劝，赶紧升！  
<table><thead><tr style="background-color: rgb(242, 242, 242);"><th style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">漏洞编号</span></span></section></th><th style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">安全版本建议</span></span></section></th></tr></thead><tbody><tr><td style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">CVE-2026-1470</span></span></section></td><td style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">1.123.17 / 2.4.5 / 2.5.1+</span></span></section></td></tr><tr><td style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">CVE-2026-0863</span></span></section></td><td style="padding: 10px;border: 1px solid rgb(221, 221, 221);"><section><span leaf=""><span textstyle="" style="font-size: 16px;">1.123.14 / 2.3.5 / 2.4.2+</span></span></section></td></tr></tbody></table>  
**防御进阶：**  
在生产环境中，**强烈建议关闭“Internal”执行模式**  
。虽然它跑得快，但隔离性差。一定要切换到 **External 模式**  
，让任务运行在独立的隔离进程中。  
  
技术圈里，功能和安全永远在博弈。n8n 给了我们自由，但也可能挖下深坑。你是如何平衡开发便捷性与系统安全的？或者你在使用沙箱时遇到过哪些坑？  
  
欢迎在评论区留言交流！  
  
想了解如何配置 n8n 的 External 隔离模式？欢迎留言或私信交流。  
  
