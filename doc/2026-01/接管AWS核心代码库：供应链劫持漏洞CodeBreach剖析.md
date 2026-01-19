#  接管AWS核心代码库：供应链劫持漏洞CodeBreach剖析  
Dubito
                    Dubito  云原生安全指北   2026-01-19 00:35  
  
   
  
> 注：本文翻译自Wiz的文章  
《CodeBreach: Infiltrating the AWS Console Supply Chain and Hijacking AWS GitHub Repositories via CodeBuild》[1]  
，可点击文末“阅读原文”按钮查看英文原文。  
  
  
全文如下：  
## 一、引言  
  
Wiz Research 发现了一个名为 **CodeBreach**  
 的关键漏洞，该漏洞曾将 AWS 控制台的软件供应链置于风险之中。该问题允许攻击者完全接管关键的 AWS GitHub 代码仓库——其中最核心的是 **AWS JavaScript SDK**  
，它是驱动 AWS 控制台运行的核心库。**通过利用 CodeBreach，攻击者本可以注入恶意代码，发起一次影响整个平台的入侵。这不仅可能危害无数依赖该 SDK 的应用程序，更可能危及控制台本身，威胁到每一个 AWS 账户。**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDQCX9dLVFZTGRIvFW7MpEf5LlYO4h2QibAW2aOicDmcd8zzZbic7NsLT1g/640?from=appmsg "null")  
  
该漏洞的根源在于受影响的代码仓库中，其  
AWS CodeBuild[2]  
 CI 流水线处理构建触发器的机制存在一个微妙缺陷。仅仅是一个正则表达式（Regex）过滤器中缺少两个字符，就使得未经身份验证的攻击者能够渗透构建环境并窃取高权限凭据。本文将详细解析我们如何利用这个细微的配置错误实现对整个代码仓库的接管，并为 CodeBuild 用户提供关键建议，以加固其项目免受类似攻击。  
  
Wiz 已将所有发现负责任地披露给 AWS，后者迅速修复了该问题。AWS 还在 CodeBuild 服务内部实施了全局加固措施以防止类似攻击。尤为重要的是，新增的  
PR请求评论批准[3]  
构建门控（build gate）为组织提供了一条简单、安全的路径，以防止不受信任的构建。请在此处阅读 AWS 安全公告。  
  
此事件延续了近期软件供应链攻击中（如   
Nx S1ngularity[4]  
 事件）常见的模式：细微的 CI/CD 配置错误导致了影响巨大的攻击。就在去年七月，一名威胁行为者就曾滥用类似的 CodeBuild 问题，对 Amazon Q VS Code 扩展的用户  
发起了一次供应链攻击[5]  
。这种日益增长的趋势凸显了各组织加固其 CI/CD 流水线的紧迫性。  
  
**2025年1月16日更新**  
：部分早期报告错误地将其认定为 CodeBuild 服务本身的漏洞。该问题源于受影响的 AWS 代码仓库其 CodeBuild 流水线内部的一个细微配置错误，而非服务层面的缺陷。尽管如此，CodeBuild 客户仍可能在自己的项目中引入同样的错误配置，因此我们强烈建议您查看下面的“必需措施”部分，以保护您自己的 CodeBuild 流水线。  
## 二、必需措施与缓解方案  
  
虽然受影响的 AWS GitHub 代码仓库的下游消费者无需立即采取行动，但我们强烈建议所有 AWS CodeBuild 用户实施以下安全防护措施，以保护其自身项目免受类似问题影响。  
- • **阻止不受信任的拉取请求触发特权构建：**  
  
- • 启用新的  
PR请求评论批准[3]  
构建门控（build gate）。  
  
- • 或者，使用   
CodeBuild 托管的运行器[6]  
，通过 GitHub 工作流来管理构建触发器。  
  
- • 如果必须依赖   
Webhook 过滤器[7]  
，请确保其正则表达式模式是锚定的（anchored）。  
  
- • **保护 CodeBuild 与 GitHub 的连接安全：**  
  
- • 为每个 CodeBuild 项目生成一个**唯一的、细粒度的个人访问令牌（PAT）**  
。  
  
- • 严格将 PAT 的权限限制在  
此处所列[8]  
的**最低**  
必需范围内。  
  
- • 考虑为 CodeBuild 集成使用一个专用的、无特权的 GitHub 账户。  
  
## 三、使用 Wiz 查找易受攻击的 CodeBuild 项目  
  
Wiz 客户可以通过   
Wiz 威胁情报中心[9]  
中这个预构建的查询，查找那些基于不受信任的拉取请求触发构建的 CodeBuild 项目。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDORnbPzRe514O3Ge6W62ZR0qj8UZo6x0ZzbYicZmVsW38x8gPbtw8xyg/640?from=appmsg "null")  
## 四、我们为何审计 CodeBuild  
  
我们对 AWS CodeBuild 的调查，始于针对 Amazon Q VS Code 扩展的未遂  
供应链攻击[5]  
。在该事件中，攻击者利用了一个配置错误的 CodeBuild 项目，入侵了该扩展的 GitHub 代码仓库，并向主分支注入了恶意代码。这些代码随后被包含在一个发布版本中，供用户下载。尽管攻击者的最终载荷由于一个拼写错误而失效，但它确实在最终用户的机器上执行了——这清楚地表明了配置错误的 CodeBuild 流水线所带来的风险。  
  
CodeBuild 是一项托管的 CI 服务，通常连接到 GitHub 代码仓库，在新PR请求等事件上触发构建。为了与 GitHub 交互，CodeBuild 需要 **GitHub 凭据**  
，这些凭据默认存在于构建环境的内存中。这就造成了一个关键风险：如果攻击者能够攻破单个构建，他们就可能通过内存转储窃取通常对源代码仓库拥有强大权限的凭据。  
## 五、构建与否：PR请求的难题  
  
攻陷 CI 构建最常见的方式是通过PR请求。攻击者先 fork 目标代码仓库，添加恶意代码，然后向原始项目发起一个 PR。如果 CodeBuild 被配置为在 PR 事件时启动构建，它就会基于攻击者的分支触发一次构建。在绝大多数像 make  
 或 yarn  
 这样的构建系统中，控制构建过程的源代码就足以运行任意代码。这正是攻击者用来入侵 Amazon Q 扩展的确切机制。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDdszz9aUYicrkumkc9oUxhE1aic6klFtrN8hcWDbmy4cyWtw79XoWWbMw/640?wx_fmt=gif&from=appmsg "")  
  
为了防止这种攻击场景，CodeBuild 提供了 **webhook 过滤器[10]**  
——这是一组事件必须满足才能触发构建的规则。回溯到去年八月，这些过滤器是防御不受信任的PR请求的主要手段。在可用的选项中，首选方案是 **ACTOR_ID**  
 过滤器：这是一个经过批准的 GitHub 用户 ID 白名单，确保只有受信任的用户才能触发构建。  
  
这看起来是一种强大的防御，但维护一个用户 ID 列表可能很繁琐。我们想知道：组织们真的正确使用了这个过滤器吗？  
  
为了查明情况，我们决定搜索连接到 **公开 CodeBuild 项目[11]**  
 的 GitHub 代码仓库。当 CodeBuild 项目设置为公开时，它会通过一个可公开访问的仪表板暴露其设置，并在任何触发构建的提交状态中自动链接到该仪表板。从这个仪表板上，任何人都可以查看项目的构建日志和配置——包括正在使用的确切 webhook 过滤器。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzD2oI1REib9CONnnXvPicLBuX1DjAJcHSLricVCEf744ugH4SqVuDWCj3Qg/640?from=appmsg "null")  
## 六、看似死胡同  
  
我们的初步扫描结果很有希望。我们很快找到了七个拥有公开 CodeBuild 页面的 AWS 旗下代码仓库。其中，有四个处于活跃状态，并且配置了在PR请求时运行构建：  
- • AWS SDK for JavaScript (aws/aws-sdk-js-v3  
)  
  
- • AWS Libcrypto (aws/aws-lc  
)  
  
- • Amazon Corretto Crypto Provider (corretto/amazon-corretto-crypto-provider  
)  
  
- • The Registry of Open Data on AWS (awslabs/open-data-registry  
)  
  
乍一看，一切似乎都很安全。这四个项目都实现了 ACTOR_ID  
 过滤器，将构建限制在批准的维护者列表内。看起来调查走到了死胡同。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDA0UhoqCSiaucnXX4OYOHY08WmkKEeEnWYNSXPlwiaAGbhibevHT9KdmlA/640?from=appmsg "null")  
  
但如上图所示，过滤器的语法对于一个典型的 ID 列表来说很不寻常。用户 ID 之间没有用逗号或空格分隔，而是用了管道符 **|**  
。这个微小的细节是关键所在：在正则表达式中，**|**  
 字符表示“或”。查阅文档后证实了这一点：这个过滤器不是一个简单的列表，**它其实是一个正则表达式模式**  
。并且它存在一个致命的缺陷。  
## 七、未锚定：一个细微的缺陷如何导致 CI 被攻陷  
  
这个问题简单但关键：**正则表达式模式没有锚定**  
。没有起始符 ^  
 和结束符 $  
 来要求精确匹配，正则表达式引擎寻找的不是完全匹配模式的字符串，而是仅仅 **包含**  
 该模式的字符串。这意味着，任何包含批准 ID（approved ID）的字符串的 GitHub 用户 ID 都可以绕过此过滤器。  
  
这在理论上是一个强大的原语，但其成功与否取决于一个实际问题：是否可能注册一个包含现有用户 ID 的全新 GitHub 用户 ID？  
## 八、当 ID 重合时  
  
答案是肯定的，这取决于 GitHub 分配 ID 的方式。每个用户都被分配了一个唯一的、**顺序递增的数字 ID**  
。2008 年的早期账户是 5 位 ID，而近年来的账户则暴涨到 9 位 ID。随着这个数字序列不断增长，较短的、较早的 ID 作为子串出现在较长的、较新的 ID 中是不可避免的。  
  
根据我们的测试，GitHub 每天大约创建 **20 万个**  
 新 ID。按照这个速度，对于任意一个给定的 6 位维护者 ID，大约 **每五天**  
 就会出现一个包含它的、更长的、可供注册的新 ID。  
  
我们将这个反复出现的机会窗口称为 **eclipse**  
——即一个新的、更长的 ID 完美“遮盖”一个受信维护者 ID 的时刻。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzD59MlQJmQhGdIU2OrSu3ndXdqFEONzR9WW2bryqhSLahQYe2VwibRfAA/640?wx_fmt=gif&from=appmsg "")  
  
这四个 AWS 代码仓库的维护者 ID 都只有 6 或 7 位数字长，这导致了频繁的“eclipse”出现，使得它们都成为了有效的攻击目标。  
## 九、捕获eclipse：赢得目标 ID 的争夺战  
  
在确认新的 GitHub ID 可以包含维护者 ID 后，挑战变成了实际操作层面的：我们如何在特定 ID 可用的瞬间立刻抢占它？这实际上是一场与全世界的竞赛，GitHub 大约每秒新增两个用户。我们需要一种能够一次性创建大量 GitHub 用户的方法。  
  
标准的用户注册流程受 reCAPTCHA 保护，使得自动创建账户无法实现。我们需要不同的方法。  
### 9.1 尝试 #1：用于 ID 采样的组织账户  
  
我们首先想到的是使用 GitHub Enterprise API 创建组织账户，它们与用户账户共享同一个 ID 池。虽然这可以让我们抢占目标 ID，但 GitHub 组织账户无法发起PR请求，这使得它们对最终的攻击利用毫无用处。不过，这并非完全的死胡同。我们重新利用了这个 API，将其改造成一个 **ID 采样工具**  
：我们可以创建一个组织账户，检查其 ID 以查看我们离目标 ID 有多近，然后立即将其删除。  
### 9.2 突破：GitHub 应用清单流程  
  
真正的突破来自   
GitHub 应用[12]  
。创建一个应用会生成一个对应的机器人用户（例如：app-name[bot]），这个用户**可以**  
与PR请求交互。通过  
清单流程（Manifest Flow）[13]  
自动化应用创建也是可行的。虽然这需要几个步骤，但可以使其具有**原子性**  
：只有当访问最终的确认 URL 时，应用及其机器人才会被创建。  
  
这使我们能够提前准备好数百个应用创建请求，然后在精确的时刻，同时访问它们所有的确认 URL。  
  
我们的策略制定完毕后，是时候执行了。我们定期使用组织创建 API 来采样当前的 GitHub ID，这让我们能够准确预测“eclipse”发生的时刻。我们还为 200 个新的 GitHub 应用启动了清单流程，收集它们唯一的确认 URL。  
  
我们等到实时 ID 计数距离目标 ID 仅剩约 100 个时，便同时访问所有 200 个 URL，触发了一场新机器人用户注册的洪流。目标 ID 是 226755743，它包含了 aws/aws-sdk-js-v3  
 代码仓库中一个受信维护者的 ID。  
  
注册完成后，快速检查确认了我们成功了：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDPgiaDUHZPtR7qFIYZnpgXdahLGULlEPia1fceXsGvclwb8A9Rmiae6dbw/640?from=appmsg "null")  
  
我们成功捕获了一个可以绕过 ACTOR_ID  
 过滤器的用户 ID，而且这个方法足够可靠，足以对四个目标仓库中的每一个都成功重复此操作。  
## 十、从绕过到Admin：执行接管  
  
在掌握了能够绕过 ACTOR_ID 过滤器的机器人用户后，我们准备执行PoC。我们选择以 aws/aws-sdk-js-v3  
 代码仓库为目标，准备了一个修复合法问题的PR请求。PR 中隐藏着真正的载荷：一个新的 NPM 包依赖项，旨在在构建环境中执行并提取 GitHub 凭据。  
  
我们提交了   
PR[14]  
，不久后就收到了通知：**一个构建被触发了**  
。片刻之后，**我们成功获取了 aws-sdk-js-v3 CodeBuild 项目的 GitHub 凭据。**  
  
（如果你有兴趣挑战一下，可以尝试找到 PR 中触发了构建的那个提交。）  
  
我们的载荷通过转储构建环境中某个进程的内存来获取 GitHub 令牌。CodeBuild 中一项  
先前为应对 Amazon Q 事件而实施的内存转储缓解措施[15]  
恰好遗漏了这个特定进程。在我们披露之后，CodeBuild 现在也对这个进程进行了保护。虽然这是一个值得欢迎的改进，但它并非万无一失。GitHub 凭据仍然驻留在构建内存中，拥有 Linux 提权漏洞利用能力的攻击者仍可能绕过内存保护。这就是为什么最稳健的缓解措施是使用构建门控（build gate），从一开始就阻止不受信任的构建运行。  
## 十一、爆炸半径  
  
我们获取到的凭据是一个属于 aws-sdk-js-automation  
 用户的 GitHub 经典个人访问令牌。对于攻击者而言，这是最佳的攻击对象，因为它定期与代码仓库交互并向 GitHub 发布新版本：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDNRK9fiaXYB58HBsibGnOHcRppRtQmyAAWIgCItCUYmJpSJfbE63tibBtw/640?from=appmsg "null")  
  
我们很快确认 aws-sdk-js-automation  
 用户拥有对该代码仓库的**完全管理员权限**  
。不过，我们最初的访问权限受限于该令牌的权限范围：repo  
 和 admin:repo_hook  
。为了提升权限，我们滥用了令牌的 repo  
 范围，该范围可以管理代码仓库协作者，从而邀请我们自己的 GitHub 用户成为代码仓库管理员。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzD3PEZS1aGluBvPPCMU2kXiayxng7vm6LuusCBGUIt8yib0nRBPHGJMWTg/640?from=appmsg "null")  
  
成为管理员后，我们现在可以直接向主分支推送代码、批准任何PR请求，并窃取代码仓库的敏感信息。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDUfYiah1jyd4iaXSugZQKHGabGOXMcOyGMBFbW1zMewqN6UpNrZCQTUpA/640?from=appmsg "null")  
  
这种级别的控制为供应链攻击提供了一条清晰的路径。JavaScript SDK 每周都会发布到 GitHub，然后是 NPM。攻击者可以滥用这种频繁的发布周期，在版本发布前注入恶意载荷，从而污染版本。就在  
一个月前[16]  
，一名威胁行为者使用完全相同的方法，成功感染了 Amazon Q VS Code 扩展的下游用户。  
  
虽然 Amazon Q 事件很严重，但此处潜在的影响呈指数级增长。**根据我们的分析，惊人的 66% 的云环境都包含了 JavaScript SDK**  
——这意味着每三个环境中就有两个安装了该 SDK 的实例。它是一个极其突出的软件库，而其用户中或许包含了云中最关键的应用：**AWS 控制台本身**  
。此外，控制台打包了较新的 SDK 版本；下图展示了一个来自控制台的请求，其中包含了用户凭据，并且使用了仅在 18 天前发布的 SDK 版本：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDyAwjoEXl4INSGOrACwYuYQAlGE3zsJHYMC0CDypB5z2HSbEicFhdYFA/640?from=appmsg "null")  
  
除了 aws/aws-sdk-js-v3  
 仓库外，我们获取的令牌还对其他几个与 JavaScript SDK 相关的仓库拥有**完全管理员权限**  
。其中包括 **三个私有仓库**  
，似乎包含 AWS 的 JavaScript SDK 私有镜像。但在此刻，考虑到已证实的接管及其潜在影响，我们停止了进一步的研究，并立即向 AWS 报告了这些问题。  
  
关键的是，这个漏洞的影响范围不仅限于 SDK。虽然我们仅在 aws/aws-sdk-js-v3  
 仓库上执行了 CI 接管，但相同的 ACTOR_ID  
 过滤器绕过年段至少在另外三个 AWS GitHub 仓库中存在。威胁行为者本可利用这些漏洞来获取另外三个 GitHub 账户的凭据。其中两个是类似 aws-sdk-js-automation  
 的自动化账户，但有一个**是 AWS 员工的个人 GitHub 账户**  
。  
## 十二、结论  
  
这个漏洞是一个教科书般的案例，说明了为什么攻击者将目标锁定在 CI/CD 环境：一个细微的、容易被忽视的缺陷可以被利用来造成巨大的影响。我们在最近的 **Nx S1ngularity[4]**  
 和 Amazon Q 供应链攻击中看到了完全相同的模式。  
  
这种趋势并非偶然。攻击者越来越倾向于 CI/CD 系统，因为它们代表着理想的目标：  
1. 1. **复杂性高**  
：这导致它们容易出现细微的配置错误；  
  
1. 2. **处理不受信任的数据**  
：通常需要测试外部贡献者的代码；  
  
1. 3. **权限极高**  
：需要强大的凭据来访问代码、发布构件和部署到云端。  
  
复杂性、不受信任的数据和高权限凭据的组合，为无需预先访问就能造成高影响的安全事件创造了完美的条件。  
  
近期攻击的成功是一个关键的警钟。攻击者已经将重点转向了 CI/CD 流水线，而防御者已经落后了。应对这一威胁需要共同努力：组织需要降低流水线权限并实施更严格的构建门控（build gate），而 CI/CD 平台应使这些安全基线易于采用。对于安全团队来说，第一步是执行一个简单而强大的原则：**绝不能让不受信任的贡献触发特权流水线。**  
## 十三、来自 AWS 的声明  
> AWS 调查了 Wiz 研究团队在“渗透 AWS 控制台供应链：通过 CodeBuild 劫持核心 AWS GitHub 仓库”一文中强调的所有报告问题。作为回应，AWS 采取了一系列措施来缓解 Wiz 发现的所有问题，并采取了额外的步骤和缓解措施，以防范未来可能出现的类似问题。  
  
> 导致已识别仓库因未锚定的正则表达式而被绕过 Actor ID 的核心问题，在首次披露后 48 小时内得到缓解。同时实施了额外的缓解措施，包括进一步加强了对所有在内存中包含 GitHub 令牌或其他凭据的构建进程的保护。此外，AWS 审计了所有其他公共构建环境，以确保 AWS 开源项目范围内不存在此类问题。最后，AWS 审计了所有公共构建仓库的日志以及相关的 CloudTrail 日志，确认没有其他行为者利用了 Wiz 研究团队演示的未锚定正则表达式问题。AWS 确认所发现的问题未对任何客户环境或任何 AWS 服务的保密性或完整性造成影响。  
  
> 我们感谢 Wiz 研究团队在识别此问题方面所做的工作，以及他们负责任地与我们合作，确保我们的客户得到持续的保护和安全。  
  
## 十四、负责任披露时间线  
  
**2025年8月25日**  
 – Wiz Research 向 AWS 报告了 Actor ID 绕过与仓库接管问题。  
  
**2025年8月25日**  
 – AWS 与 Wiz 会面，审查发现结果并讨论缓解措施。  
  
**2025年8月27日**  
 – AWS 锚定了存在漏洞的 Actor ID 过滤器，并撤销了 aws-sdk-js-automation  
 的个人访问令牌。  
  
**2025年9月**  
 – AWS 实施额外加固措施，防止非特权构建通过内存转储访问项目凭据。  
  
**2026年1月15日**  
 – 公开披露。  
#### 引用链接  
  
[1]  
 《CodeBreach: Infiltrating the AWS Console Supply Chain and Hijacking AWS GitHub Repositories via CodeBuild》: https://www.wiz.io/blog/wiz-research-codebreach-vulnerability-aws-codebuild  
[2]  
 AWS CodeBuild: https://aws.amazon.com/codebuild/  
[3]  
 PR请求评论批准: https://docs.aws.amazon.com/codebuild/latest/userguide/pull-request-build-policy.html  
[4]  
 Nx S1ngularity: https://www.wiz.io/blog/s1ngularity-supply-chain-attack  
[5]  
 发起了一次供应链攻击: https://aws.amazon.com/security/security-bulletins/AWS-2025-015/  
[6]  
 CodeBuild 托管的运行器: https://docs.aws.amazon.com/codebuild/latest/userguide/action-runner.html  
[7]  
 Webhook 过滤器: https://docs.aws.amazon.com/codebuild/latest/userguide/webhooks.html  
[8]  
 此处所列: https://docs.aws.amazon.com/codebuild/latest/userguide/access-tokens-github.html  
[9]  
 Wiz 威胁情报中心: https://app.wiz.io/boards/threat-center/wiz-adv-2026-002  
[10]  
 webhook 过滤器: https://docs.aws.amazon.com/codebuild/latest/userguide/github-webhook.html  
[11]  
 公开 CodeBuild 项目: https://docs.aws.amazon.com/codebuild/latest/userguide/public-builds.html  
[12]  
 GitHub 应用: https://docs.github.com/en/apps/overview  
[13]  
 清单流程（Manifest Flow）: https://docs.github.com/en/apps/sharing-github-apps/registering-a-github-app-from-a-manifest  
[14]  
 PR: https://github.com/aws/aws-sdk-js-v3/pull/7280  
[15]  
 先前为应对 Amazon Q 事件而实施的内存转储缓解措施: https://aws.amazon.com/security/security-bulletins/aws-2025-016/  
[16]  
 一个月前: https://www.csoonline.com/article/4027963/hacker-inserts-destructive-code-in-amazon-q-as-update-goes-live.html  
  
   
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzD5OlztdUBOBheR50SNynuAdLZ0z2tazBjKCDibN2ibOuIcwiacyb0FwOVw/640?wx_fmt=gif&from=appmsg "")  
  
  
  
**交流群**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4Y3Su4ibkBkPLJTZXSkIiayzDCJS2Ic431MBtccRgcqmFDGJEiaAkr2QzWEwrEiazPISAoU89yGGN1n4A/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
