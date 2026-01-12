#  nOAuth风险追踪：漏洞或致Microsoft 365沦陷  
Dubito  云原生安全指北   2026-01-12 01:12  
  
   
  
> 注：本文翻译自 Semperis - Eric Woodruff 的文章  
《nOAuth Abuse Update: Potential Pivot into Microsoft 365》[1]  
，可点击文末“阅读原文”按钮查看英文原文。  
  
  
全文如下：  
## 一、关键发现  
- • 在自最初研究以来额外测试的 38 个应用程序中，Semperis 发现其中 2 个易受 nOAuth 滥用攻击。我们已向相关软件供应商报告了这两个应用的发现。  
  
- • 其中一个易受攻击的应用程序所具有的权限，可能使攻击者能够借此跳转回 Microsoft 365 环境。这再次印证了 nOAuth 不仅会危及 SaaS 应用程序内的数据，还可能威胁到组织的整个 Microsoft 365 资产。  
  
- • 在我们最近与 Microsoft 安全响应中心（MSRC）的案例中，MSRC 将此漏洞评级为“中危”。然而，Semperis 坚持我们 **“高危”**  
 的评级。对于易受攻击应用程序的客户而言，nOAuth 仍然难以检测，且客户自身无法防御。其滥用方法已被公开披露，一旦发现易受攻击的应用程序，实施攻击的复杂度较低。  
  
本文是对  
我们关于 nOAuth 的原始研究[2]  
（译文见同期发布的另一篇文章）的后续更新。尽管感觉像是昨天我们才在  
TROOPERS25[3]  
和 fwd:cloudsec 2025 上发布了这些发现，但我们的研究实际上始于 2024 年秋季。  
  
在最初演讲六个月后，以及最初研究一年后，我们进行了额外的 nOAuth 研究，发现 nOAuth 滥用的风险依然存在，并且许多组织仍未意识到此漏洞。  
  
在 2025 年 10 月和 11 月期间，我们额外测试了 38 个应用程序是否存在 nOAuth 滥用，发现其中 2 个易受攻击。在我们之前的研究中，我们测试了 104 个应用程序，发现 9 个易受攻击。这意味着，从累计百分比来看，大约有 8% 的可用应用程序易受 nOAuth 攻击。  
## 二、了解 SaaS 应用如何与 Microsoft 365 集成  
  
Microsoft Graph API 是用于访问所有 Microsoft 365 服务（如 Exchange Online、SharePoint、OneDrive 及大多数其他 Microsoft 365 服务）的统一 API。希望与这些服务集成的第三方 SaaS 应用程序会请求访问 Microsoft Graph，以便代表用户与其数据或服务进行交互。  
  
以 Calendly 为例，它会请求用户日历的权限，以便管理其在 Office 365 中的会议预约（见图 1）。**需要明确的是，Calendly 并_不_易受 nOAuth 攻击。**  
 我们只是将其作为一个参考应用，帮助您理解 SaaS 应用的常见行为。  
  
![图 1. Calendly 请求日历读写权限](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKolliceeZ0qujD1Xn2fvaT3574ulszZlnsLRyYuoUVzNGUjy8oibXuwlIW3A/640?from=appmsg "null")  
  
图 1. Calendly 请求日历读写权限  
  
当用户访问该应用程序时，只要用户已同意相关权限，SaaS 应用就会从 Entra ID 收到一个 OAuth 2.0 访问令牌（access token）。随后，该应用将使用此访问令牌通过 Microsoft Graph API 访问服务。在此示例中，提供给 Calendly 的访问令牌拥有 **Calendar.ReadWrite**  
 和 **Calendar.ReadWrite.Shared**  
 权限。  
  
在许多场景下，SaaS 应用还会收到一个刷新令牌（refresh token）（见图 2）。此令牌使得 SaaS 应用即使用户没有主动使用该应用，也能维持对用户数据的访问。  
  
![图 2. Calendly 请求使用刷新令牌的权限](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollxFa8qZqib6CvPuJGJZHfE8pib04E3TOibsEvaOJ0sIxThEAiaejnPaic2Eg/640?from=appmsg "null")  
  
图 2. Calendly 请求使用刷新令牌的权限  
  
SaaS 应用为何需要这类访问权限？在我们的示例中，这种权限使得 Calendly 能够在您没有主动使用该应用时，在您的日历上预订客户约会。这样，SaaS 应用就可以在后台持续地对您的数据进行各种“操作”。  
  
这对于提升工作效率非常有益，而且所请求的权限本身并无不妥。但是，当这些权限与易受 nOAuth 攻击的漏洞相结合时，就为攻击者滥用该应用（并可能借此跳转回 Microsoft 365）创造了条件。  
## 三、身份验证（Authentication）与授权（Authorization）的区别  
  
要完全理解 nOAuth 如何能够为跳转回 Microsoft 365 提供便利，你需要知道：  
- • **OpenID Connect (OIDC)**  
 通过 ID 令牌处理 **身份验证**  
。  
  
- • **OAuth 2.0**  
 通过访问令牌处理 **授权**  
。  
  
当访问令牌附带有刷新令牌时，SaaS 应用无论用户是否通过身份验证，都可以获取新的访问令牌。  
  
抛开身份领域术语的细微差别，可以这样理解：  
- • OIDC 处理的是用户登录应用程序。  
  
- • OAuth 2.0 处理的是应用程序对用户数据的访问权限。  
  
- • 刷新令牌使得应用程序即使用户未登录，也能访问用户数据（见图 3）。  
  
![图 3. 展示身份验证与授权区别的示意图](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollHS2KzooNFYicxMKkzZbc0uMe12d1PcNsVby1gYnvx2jmJARUhmO1drw/640?from=appmsg "null")  
  
图 3. 展示身份验证与授权区别的示意图  
  
当用户首次访问一个应用程序时，可能会同时发生身份验证和授权：应用程序可能会收到用户的 ID 令牌 **和**  
 一个访问令牌，以便该应用能够访问 Microsoft 365 资源。对于最终用户来说，这个过程通常是透明的，除非是首次使用该应用且需要授权同意时。  
  
虽然存在许多用于管理令牌的框架和库，但最终 SaaS 应用程序的开发者决定了令牌的管理方式。  
  
当用户后续再次进行身份验证时，如果现有的令牌仍然有效，应用程序可能（也可能不）会尝试请求新的访问令牌和刷新令牌。由于身份验证和授权在本质上被视为不同的功能，这两种流程之间的脱节使得攻击者能够实施跳转攻击。  
## 四、滥用 nOAuth 以跳转至 Microsoft 365  
  
在我们最初的研究中，我们发现了一个易受攻击的应用程序，它集成了 Microsoft 365 并拥有 **Calendars.ReadWrite.Shared**  
 权限。根据微软文档，此集成：  
> 允许该应用在组织用户有权访问的所有日历中创建、读取、更新和删除事件。这包括委托日历和共享日历。  
  
  
这令人担忧，但尚不足以实施例如商业电子邮件入侵（BEC）这类攻击。  
  
而这一次，我们发现了一个范围更广、更令人担忧的权限集合（见图 4）：  
- • **Calendars.ReadWrite**  
  
- • **Mail.Send**  
  
- • **Contacts.ReadWrite**  
  
- • **Mail.ReadWrite**  
  
- • **offline_access**  
  
![图 4. 易受攻击应用程序上的权限](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollGp8o2VZdK6g663hicrCJPH6N89euH0KUdg1ibavGicRq5WYwlEz0PACnQ/640?from=appmsg "null")  
  
图 4. 易受攻击应用程序上的权限  
  
如前所述，这些权限之所以令人担忧，是因为它们不仅能让攻击者入侵用户在 SaaS 应用内的账户，还能利用该应用的接口跳转回 Microsoft 365。  
  
请记住，身份验证（authentication）和授权（authorization）是使用不同令牌的不同功能。攻击者本质上是通过控制在 SaaS 应用程序内部管理的访问令牌和刷新令牌，获得了用户在 Microsoft 365 中的访问权限控制权（见图 5）。  
  
![图 5. 攻击者获取合法用户账户访问权限的可视化示意图](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollRDAwYTiamSwEjH6Z2KNeBOzqKNPTgcZEy7kiawpK4AV3JF3Wkvemx63w/640?from=appmsg "null")  
  
图 5. 攻击者获取合法用户账户访问权限的可视化示意图  
  
在这个易受攻击的 SaaS 应用程序中，此行为表现为攻击者能够以被入侵用户的身份发送电子邮件（见图 6）。  
  
![图 6. 从 SaaS 应用程序以被入侵用户身份发送的电子邮件](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollR9ibILB8zL3zEneicCqoE4XgHsLpfXKes7zwkgXHRiahuQwMKQ9ic3swicQ/640?from=appmsg "null")  
  
图 6. 从 SaaS 应用程序以被入侵用户身份发送的电子邮件  
  
由于电子邮件来自租户内的合法用户，攻击者现在拥有了一种有效的方式，可以在 Exchange Online 中以被入侵用户的身份进行操作（见图 7）。  
  
![图 7. 来自我们攻击模拟的接收到的电子邮件](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollia4NZmlzA41RnUakrsgW7osD7zRyYzibXMoNRNee9PxspiaLVliaM01R1g/640?from=appmsg "null")  
  
图 7. 来自我们攻击模拟的接收到的电子邮件  
  
攻击者并不能直接访问访问令牌（access token），因此他们与 Microsoft 365 资源的交互方式由 SaaS 应用程序的用户界面决定。尽管如此，这些发现再次印证了我们长期以来对 nOAuth 的担忧，以及它可能引发的数据访问和横向移动路径问题。  
## 五、研究 nOAuth 如同试图证明一个否命题  
  
当我们在 2025 年 6 月发布研究结果时，收到最多的问题就是：总共有多少应用程序可能易受攻击。这可以理解，因为不确定的数字难以成为有力的标题。  
  
然而，无论当时还是现在，都不可能量化有多少应用程序可能易受攻击。搜索现有的 SaaS 应用数量，得到的数字从 70,000 到超过 300,000 不等。Microsoft 365 Defender 的云应用目录列出了 36,932 个 SaaS 应用程序，但并非所有应用都集成了 Entra ID 进行身份验证。  
  
无论你如何计算，这都将是猜测。按最低估算，如果所有应用中有 50% 使用 Entra ID 进行身份验证，那么我们测试了 142 个（总数约 35,000）应用，仅占测试总量的 0.4%。按最高估算（总数 150,000 个应用），测试覆盖率仅为 0.098%。  
  
在 Troopers 会议上，我曾表示我们发现了 9 个易受攻击的应用程序，并且我怀疑这并非全部易受攻击应用的 90%。现在，结合我们的新发现，我同样怀疑 11 个也不是所有易受攻击应用的总数。这根本不可能确定。  
  
微软方面或许知道有多少应用程序将 **removeUnverifiedEmailClaim**  
 设置为 **false**  
，以及有多少应用因历史遗留原因仍在使用此声明。但这并不能表明应用程序是否易受攻击。  
  
在权限这个话题上，虽然我们只发现了具有 Exchange Online 集成的应用被滥用，但 SharePoint 和 OneDrive 的集成同样普遍。认为只有特定比例具有此类集成的应用易受 nOAuth 攻击，这种想法是幼稚的。Microsoft Graph 还有数百个其他权限，其中许多权限如果落入攻击者之手，将非常令人担忧。事实上，获取这些权限正是另一种非常常见的攻击——  
非法授权许可[4]  
——的主要目标。  
## 六、时间线与 MSRC 的回应  
- • **2025 年 11 月 18 日：**  
 基于针对 Microsoft 365 的新发现以及这种能够跳转回该环境的能力，我们向 MSRC 提交了一个案例。我们提供了完整的发现细节，包括文字说明和演示针对我们测试用户攻击的视频。我们还要求 MSRC 终止发送未验证电子邮件声明的能力。正如我们在  
原始文章[2]  
（译文见同期发布的另一篇文章）中讨论的，还存在其他获取电子邮件声明的方式。  
  
- • **2025 年 12 月 3 日：**  
 MSRC 回复称，此案例被视为中等风险并结案。在其回复中，MSRC 陈述如下：  
  
> 经过仔细调查，我们评估此案例为中等严重性漏洞。已就此通知我们相关的产品/服务团队，他们可能会在未来的版本中根据需要，增加进一步的纵深防御机制。  
> 因此，我们将关闭此案例。  
  
## 七、为何 nOAuth 依然重要  
  
我们将继续研究易受 nOAuth 攻击的应用程序，并推动微软做出改变，原因如下：  
- • nOAuth 滥用已被公开披露。  
  
- • 研究已证明，攻击者能够访问诸如人力资源管理系统（HRMS）等平台中的敏感信息。  
  
- • 研究已证明，存在潜在的横向移动路径可以跳转回 Microsoft 365。  
  
- • 易受攻击应用的客户对 nOAuth 没有防御手段。  
  
- • 安全研究人员已证明，确定 SaaS 应用的客户相对容易。  
  
- • 利用 LinkedIn 等平台锁定 SaaS 应用的特权用户并推断其电子邮件地址也同样简单。  
  
## 八、Semperis 要点简述  
  
虽然企业组织对 nOAuth 没有强有力的防御手段，但了解其相对于您自身数据的风险至关重要。在引入新的 SaaS 应用程序之前，请询问供应商是否知晓 nOAuth 相关研究，并引导供应商参考我们的博客文章。  
  
对于供应商和开发者而言，遵循微软的 OpenID Connect 规范对于确保您的应用程序不易受 nOAuth 攻击至关重要。更多详细信息，请参阅我们之前的研究。  
## 九、相关资源  
  
•   
nOAuth 滥用警报：Entra 跨租户 SaaS 应用程序的完全账户接管[2]  
（译文见同期发布的另一篇文章）  
  
•   
nOAuth 点播会话（TROOPERS25）[3]  
#### 引用链接  
  
[1]  
 《nOAuth Abuse Update: Potential Pivot into Microsoft 365》: https://www.semperis.com/blog/noauth-abuse-update-pivot-into-microsoft-365/  
[2]  
 我们关于 nOAuth 的原始研究: https://www.semperis.com/blog/noauth-abuse-alert-full-account-takeover/  
[3]  
 TROOPERS25: https://www.youtube.com/watch?v=xfcESxDh3O8&utm_source=cloudseclist.com&utm_medium=referral&utm_campaign=CloudSecList-issue-320  
[4]  
 非法授权许可: https://learn.microsoft.com/en-us/defender-office-365/detect-and-remediate-illicit-consent-grants  
  
   
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/d7OsfYudM4ayA6XdXTPxicO59mfEJKollAso1SIyiaMdUnKogX9JAtvOOBIT9Mr1CfQMzlL71oBJVbmxWjzhSzaQ/640?wx_fmt=gif&from=appmsg "")  
  
  
  
**交流群**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/d7OsfYudM4ayA6XdXTPxicO59mfEJKollS8FkphnRhWUPJGtuRevW3oYQ5sKsrUz2OWtDrwmiae5FN7pgvryQkRg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
