#  第79天-Web安全攻防：轻松掌握越权漏洞，从入门到实战！  
原创 Сяо Яо
                    Сяо Яо  AlphaNet   2026-03-11 02:13  
  
朋友们好！👋 在我们的数字世界里，每个网站和应用背后都有一套精密的“规则”——也就是**访问控制**  
。它就像一个严格的保安，确保只有“对的人”才能做“对的事”。但如果这个“保安”打了个盹，会发生什么呢？🤔  
  
  
没错，这就是我们今天要深入探讨的核心主题——**越权漏洞 (Broken Access Control)**。这是一种非常普遍但极其危险的安全缺陷。攻击者可以利用它，像拿到万能钥匙一样，随意进出系统，查看不该看的数据，甚至执行管理员才能进行的操作。  
  
  
别担心，这篇文章将用最通俗易懂的方式，带你彻底搞懂“越权”到底是什么，为什么它如此重要，以及我们该如何发现和防范它。准备好了吗？让我们一起开始这场精彩的攻防学习之旅吧！  
  
  
### 🧐 是什么：越权漏洞的三种主要类型  
  
  
首先，我们需要明确越权漏洞的几个核心分类。理解了它们，你就掌握了分析问题的基础框架。  
  
#### 1. 水平越权 (Horizontal Privilege Escalation)  
  
>   
> 简单说，就是“你的就是我的”。  
>   
  
  
  
这发生在同级别用户之间。比如，用户 A 通过修改请求中的某个参数（如用户 ID），成功访问或操作了本应属于用户 B 的数据。  
  
  
**场景举例：**  
  
https://example.com/orders?user_id=123  
  
如果你把 123 修改成 124，结果看到了另一个用户的订单，那么水平越权就发生了。  
  
  
#### 2. 垂直越权 (Vertical Privilege Escalation)  
  
>   
> 简单说，就是“小兵想当将军”。  
>   
  
  
  
这指的是低权限用户成功执行了高权限用户（如管理员）才能进行的操作。这是最危险的越权类型之一，因为它可能导致整个系统的控制权被夺取。  
  
  
**场景举例：**  
  
https://example.com/admin/dashboard  
  
一个普通用户通过修改请求，访问了只有管理员才能进入的后台管理页面，并成功执行了删除用户、修改商品价格等高权限操作。  
  
  
#### 3. 未授权访问 (Unauthorized Access)  
  
>   
> 简单说，就是“游客变主人”。  
>   
  
  
  
这种情况是指一个未经身份验证的“游客”用户，通过直接访问特定 URL 或其他手段，绕过了登录验证，直接访问了需要登录后才能查看的页面或功能。  
  
  
**场景举例：**  
  
https://example.com/admin/secret_panel  
  
虽然这个 URL 没有公开链接，但如果攻击者猜到了它，并且系统没有对访问该 URL 的用户进行身份验证，那么任何人都可以直接进入后台。  
  
  
### 🤔 为什么：越权漏洞的成因与危害  
  
  
越权漏洞的根源在于**不充分或不正确的访问控制验证**。开发者可能只在前端界面上隐藏了某些按钮或链接，却忘记了在服务器后端对每一个敏感请求都进行严格的权限校验。  
  
  
攻击者从不按常理出牌，他们会直接向服务器发送精心构造的请求，绕过前端的限制。一旦成功，危害是巨大的：  
  
- **数据泄露**：用户的个人信息、订单、密码等敏感数据被窃取。  
- **账户劫持**：攻击者可以修改他人账户的密码、邮箱，从而完全控制该账户。  
- **系统瘫痪**：如果获得管理员权限，攻击者可以删除数据、修改系统配置，甚至让整个网站下线。  
- **财产损失**：在金融或电商应用中，越权可能导致资金被非法转移。  
### 🛠️ 怎么做：挖掘、测试与自动化检测  
  
  
理论知识已经掌握，现在进入激动人心的实战环节！我们该如何像一个专业的安全研究员一样，去发现这些潜在的漏洞呢？  
  
#### 核心思路  
  
>   
> **关注所有与用户身份和权限相关的参数！**  
>   
  
  
  
无论是 URL 中的查询参数、请求体中的表单数据，还是隐藏在 Cookie 和 JWT (JSON Web Token) 中的信息，都可能是突破口。  
  
  
### 实战演练与测试方法  
  
  
为了更好地练习，强烈推荐 **PortSwigger 的 Web Security Academy**，它提供了大量免费的在线实验环境。  
  
#### 1. 针对未授权访问的测试  
  
  
方法：尝试直接访问已知的管理后台路径。  
  
/admin/manage/console  
  
删除所有 Cookie 和认证信息，再尝试访问需要登录的个人中心页面。  
  
  
实验推荐：  
  
https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionalityhttps://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter  
  
#### 2. 针对水平越权的测试  
  
  
方法：  
  
  
准备两个普通用户账号 A 和 B。登录账号 A，操作一个功能（如查看个人资料），抓取请求包。  
  
user_idaccount_id  
  
将请求包中所有与用户 A 相关的 ID 修改为用户 B 的 ID，然后重放请求。  
  
  
实验推荐：  
  
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameterhttps://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids  
  
#### 3. 针对垂直越权的测试  
  
  
尝试在请求中加入权限参数：  
  
role=adminisAdmin=true  
  
实验推荐：  
  
https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile  
  
### 自动化检测工具  
  
  
在真实渗透测试中，我们常使用自动化工具辅助发现越权漏洞。  
  
AutorizeProhttps://github.com/WuliRuler/AutorizePro  
PrivHunterAIhttps://github.com/Ed1s0nZ/PrivHunterAI  
xia_Yuehttps://github.com/smxiazi/xia_Yue  
  
### 📜 总结：核心要点回顾  
  
1. 越权分为 **水平越权、垂直越权、未授权访问** 三种类型。  
1. 本质原因是 **服务器没有进行严格权限校验**。  
1. 挖掘方法核心是 **修改用户身份参数并重放请求**。  
今天的分享就到这里啦！希望这篇文章能帮助你对越权漏洞有一个更清晰、更深入的理解。  
  
  
最后，留给大家一个思考题：  
  
  
**在你日常使用的网站或 App 中，你觉得哪些功能点最有可能存在越权漏洞呢？**  
  
  
欢迎在评论区留下你的看法和思考，我们一起交流进步！  
  
