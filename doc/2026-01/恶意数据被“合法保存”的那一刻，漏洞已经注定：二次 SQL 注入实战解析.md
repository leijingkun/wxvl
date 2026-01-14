#  恶意数据被“合法保存”的那一刻，漏洞已经注定：二次 SQL 注入实战解析  
原创 武文学网安  武文学网安   2026-01-14 21:15  
  
大家好，我是武文。  
  
今天继续挑战学习 **sqli-labs**  
。  
  
当我来到 **第 24 关**  
 时，出现了一个让我非常不安的信号：  
**所有我熟悉的 SQL 注入手法，全部失效了。**  
  
不管是：  
- 在用户名中尝试  
admin' or 1=1 #  
  
- 在登录阶段反复测试各种闭合方式  
  
- 在修改密码时构造逻辑条件、报错函数  
  
结果都是一样的：  
- 登录有校验  
  
- payload 被识别  
  
- 页面没有任何 SQL 报错  
  
- 看起来一切都“很安全”  
  
一度让我产生了错觉：这一关是不是已经被修好了？  
  
但冷静下来后，我意识到一件事：  
  
👉 **这并不符合 sqli-labs 的套路。**  
  
既然老办法完全行不通，那问题就一定不在“payload 写得对不对”，而在 **思路本身已经需要升级**  
。  
## 一、为什么第 24 关的“即时注入”全部失败？  
  
在前面的关卡中，我们习惯的攻击模型是：  
输入 → SQL 执行 → 页面立即反馈  
  
比如典型的：  
```
SELECT * FROM users 
WHERE username='$user' AND password='$pass';
```  
  
只要能控制 $user  
 或 $pass  
，就能通过闭合、逻辑判断、报错函数等方式，  
当场看到结果。  
  
**但第 24 关不是这样。**  
  
这一关的核心特征是：你输入的数据，并不会立刻参与危险 SQL 执行。  
  
它只是被：  
- 合法校验  
  
- 正常存储  
  
- 安静地躺进数据库  
  
危险，并没有消失，只是被**延迟了**  
## 二、理论篇：什么是二次 SQL 注入（Second Order SQL Injection）？  
### 2.1 一次注入 vs 二次注入  
  
我们熟悉的，大多是**一次注入**  
：用户输入 → 拼接 SQL → 立刻执行 → 立刻出问题  
  
而 **二次注入**  
 的流程是：  
1. 用户输入恶意数据  
  
1. 程序 **认为它是“安全数据”**  
，存入数据库  
  
1. 在另一个功能中，把这段数据 **重新取出**  
  
1. **未做过滤，直接拼接进 SQL 再次执行**  
  
1. **漏洞，不在输入点，在“第二次使用”时触发**  
  
真正的注入点，不在输入时，  
  
而在 **“未来某个业务逻辑中”**  
。  
### 2.2 为什么开发者很容易踩这个坑？  
  
因为在开发视角里：  
- “这是数据库里的数据”  
  
- “不是用户刚刚输入的”  
  
- “之前已经校验过了”  
  
👉 **于是默认它是安全的。**  
  
但攻击者恰恰就是在**第一次“合法存储”**时埋雷，等着系统在未来某个功能里自己引爆。  
## 三、实操篇：sqli-labs 第 24 关的真实注入思路,从业务流程反推 SQL 执行路径  
#### 3.1 先不注入：梳理完整业务流程  
  
访问第 24 关后，可以明确看到这是一个**完整用户系统**  
：  
- 用户注册（new_user.php）  
  
- 用户登录（login.php）  
  
- 用户修改密码（pass_change.php）  
  
这一步**先不急着打 payload**  
，而是问一个更重要的问题：  
每一步，后端“可能”在执行什么 SQL？  
#### 3.2 反推三条核心 SQL 语句（关键思维）  
  
结合常见各个页面和 PHP + MySQL 写法，可以合理猜测  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAicgicNibKaRS55t2ESoC3iaMbnvGbtKv3BJWF0VsK4ITicTKUVgibcRvzoFg/640?wx_fmt=png&from=appmsg "")  
  
注册阶段：  
```
INSERT INTO users (username, password)
VALUES ('$username', '$password');
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAYnTUn2XC2Tae4U9280L9rj7ZzAuqgScL6zzc1iaVNOu3rRCPOZ75kyw/640?wx_fmt=png&from=appmsg "")  
  
登录阶段：  
```
SELECT * FROM users
WHERE username = '$username'
AND password = '$password';
```  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAekMx7sG1zcePrGXN5wmmMFL6hTQicW1cXAy7he8lIGE8ibLicp4m9Inwg/640?wx_fmt=png&from=appmsg "")  
  
修改密码阶段：  
```
UPDATE users
SET password = '$new_password'
WHERE username = '$current_user' and password='$old_password';
```  
  
到这里，一个**非常关键的差异**  
就出现了：  
  
👉 前两条 SQL 使用的是**当次用户输入**  
  
👉 第三条 SQL 使用的是**从数据库中读取出来的数据**  
  
这背后，隐藏着一个在真实业务中极其常见、也极其危险的安全假设：  
> **“只要数据已经存进数据库了，它就是安全的。”**  
  
  
而这，正是二次 SQL 注入产生的根源。  
#### 3.3 第一反应：能不能在“登录阶段”绕过？  
  
这是一个**非常自然、也非常正确的攻击尝试**  
。  
  
尝试在登录框中输入：  
```
admin' or 1=1 #
admin" or 1=1 #
admin') or 1=1 #
admin") or 1=1 #
```  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAhv4TyjXjX2XqUzXVhVS8VardJSBmAwUTaSheJkIlBkbmsTrajOF2nA/640?wx_fmt=gif&from=appmsg "")  
  
结果发现：  
- 登录失败  
  
- 页面提示：BUG OFF YOU SILLY DUNMB HACKER  
  
这说明什么？  
  
👉 **登录阶段做了输入过滤或预处理**  
  
👉 **用户输入在“第一次使用”时是安全的**  
  
这一步**不是失败，是排除法**  
。  
  
3.4 第二反应：能不能在“注册阶段”直接显错？  
  
既然前面已经学过在 INSERT  
 场景下构造 updatexml()  
，一个很自然的想法是：  
能不能在注册阶段就触发 error-based？  
  
例如构造这样的SQL语句：  
```
INSERT INTO users (username, password)
VALUES (
    'admin' AND updatexml(1,concat(0x7e,database(),0x7e),1),
    '123456'
);
```  
  
在MySQL中，这样的语法是可以反馈当前数据库信息的。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDA6gYkN2ibWdTtiaAZwV4glbiabo7jFImv1icaBXcSQP2a8j82fPjCubrSrg/640?wx_fmt=png&from=appmsg "")  
  
为了闭合可能存在的闭合符号，我们的完整SQL语句大概率是：  
```
INSERT INTO users(username, password)
VALUES (
'admin' AND updatexml(1,concat(0x7e,database(),0x7e),1) AND '1'='1',
'123456'
);
```  
  
理论上在mysql中同样能够实现XPATH显错注入。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAHiaj0GKT8RfPQgmUVG3EsKAPEZeS0eJTxbkpXnznPKeV3zDxGOZPEvg/640?wx_fmt=png&from=appmsg "")  
  
于是尝试在用户名中构造类似payload：  
```
admin' AND updatexml(1,concat(0x7e,database(),0x7e),1) and '1'='1
```  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDASRV3TFUrNl21bvcFwyUsnB74heuAtNtvOHHNZ4y8CLkQJibp5xf4gFQ/640?wx_fmt=gif&from=appmsg "")  
  
  
这一步明显是构造了必然引发数据库错误的语法。但，  
  
实际测试结果是：  
- 注册阶段**不会产生任何数据库报错**  
  
- 页面行为始终正常  
  
原因并不复杂：  
  
👉 注册阶段只是把数据**原样存入数据库**  
  
👉 并不存在「危险 SQL 逻辑」被执行的时机  
  
这一步的意义在于：确认不存在即时注入。  
  
在数据库可以看到我们刚刚注册的数据已经存入进去：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAMckv1slUwj2JQBKetu4rOCQ0JRpOHMUQPjxoiclBzhJKLrhLRPJicR4A/640?wx_fmt=png&from=appmsg "")  
#### 3.5 关键转折：不是“即时注入”，而是“延迟触发”  
  
到这里我才意识到：  
**我不是没找到注入点，而是一直在错误的时间点寻找注入点。**  
  
可以做出一个非常重要的判断：  
- 注册阶段：只是存数据  
  
- 登录阶段：有过滤  
  
- 修改密码阶段：使用的是**数据库中已有的数据**  
  
于是攻击模型发生根本变化：  
  
❌ 不是“我输入了什么，立刻发生什么”  
  
✅ 而是“我现在埋下什么，未来会被怎样使用”  
  
这，正是  
二次 SQL 注入（Second Order Injection）的典型特征。  
#### 3.6 新思路：让“未来会被使用的字段”携带恶意结构  
#### 既然真正危险的SQL是：  
```
UPDATE users
SET password = ...
WHERE username = '$current_user'
AND password = '$old_password';
```  
  
攻击目标随之明确：  
- 不再想着绕过登录  
  
- 而是让 **用户名本身变成 SQL 结构的一部分**  
  
于是 payload 的设计目标变成：  
当用户名被拼接进 UPDATE 语句时，改变 WHERE 条件的语义  
#### 3.7 构造最小、最稳定的 payload  
  
在明确攻击点后，payload 的目标已经非常清晰：  
  
👉 不是绕过登录  
  
👉 而是让用户名在 **UPDATE 语句中改变 WHERE 条件的语义**  
  
因此这里选择一个“破坏性最小、效果最稳定”的 payload：  
```
admin'#
```  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAwBVyL4fcEfNVvBMNoy1h2Ktb10cAJzrYp7H58GIG4SaudDibGWNzqiaw/640?wx_fmt=gif&from=appmsg "")  
  
原因很简单：  
- 不依赖 OR 1=1  
  
- 不引入额外 SQL 结构  
  
- 只做一件事：**截断 WHERE 条件，可以直接注释掉AND password='...'这一安全校验**  
  
这时我们可以在数据库中看到数据：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAukbhj8ictiaye6ibn53kSdPafe4VyJfpwepytTMvAtJC0895jBXQLyYkQ/640?wx_fmt=png&from=appmsg "")  
  
damin'# 用户名，密码1234已经被存入数据库中，而原有的admin账户密码为admin。  
  
当用户名被再次拼接进 SQL 时，后端实际执行的是：  
```
UPDATE users
SET password = 'newpass'
WHERE username = 'admin'
```  
  
旧密码校验，在这一刻被彻底绕过。  
#### 3.8 利用链闭环：漏洞触发点终于出现  
  
完整利用链如下：  
  
1️⃣ 注册用户名为 admin'#  
  
2️⃣ 使用该用户正常登录  
  
3️⃣ 进入修改密码页面  
  
4️⃣ 提交新密码  
  
后端执行的 SQL 变为：  
```
UPDATE users
SET password = 'newpass'
WHERE username = 'admin'#' and password='$old_password';
```  
  
注意：这里 #  
 之后的内容虽然在字符串层面看似不完整，但在 MySQL 解析阶段已经被注释，不再参与语法分析。  
  
我们直接实践，将admin'# 密码修改为123  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAaev7ZCzKrMnItjSk0tRyOpsSUUcFvBXPQ11Xk3tOyM5QicgoJz3TMwA/640?wx_fmt=gif&from=appmsg "")  
  
从前端页面可以看到admin'# 的密码成功修改。这接下来让我们来看一下mysql数据库的信息：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAKuHbVJz5SyxBmyah9fh1M3qLI0I3EW5AJBe5tibf7QalSqMaicUxBPlA/640?wx_fmt=png&from=appmsg "")  
  
可以发现，原有的admin用户密码被修改为123了，而我们注册的admin'# 用户密码仍保持不变。  
  
可以分析知道，在 MySQL 解析阶段：  
- #  
 之后的内容被直接注释  
  
- 原本用于校验旧密码的条件被裁剪  
  
- UPDATE 的作用范围被意外扩大  
  
👉 **管理员账号密码被直接覆盖**  
  
**这样就可以实现修改已经存在的用户的密码，而不需要原账户的密码。这时可以直接用修改后的密码123，直接登录admin用户。**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/VFf46TKXLVFRxgnVZXhjtUniaUIicepWDAwDoO7wTUjNvX7fIkhmKJicDutpl6H6bxmJ1y0kqEbPiag3fib4TibMVkuA/640?wx_fmt=gif&from=appmsg "")  
  
从第 24 关开始，判断 SQL 注入是否存在，已经不能再单纯从“输入框”出发，而必须从**数据在系统中是如何被存储、再被使用的这个角度出发。**  
  
四、这一关真正危险的地方在哪？  
  
第 24 关的危险性，不在 payload，而在**时序**  
：  
- 恶意数据先被“合法存储”  
  
- 漏洞在未来某个功能中触发  
  
- 安全测试很容易漏掉  
  
这正是为什么二次注入在真实业务中：  
- 更隐蔽  
  
- 生命周期更长  
  
- 危害更大  
  
## 五、结语：第 24 关真正教会我的是什么？  
  
第 24 关并没有教我新的“高级 payload”。  
  
它真正教会我的，是三件事：  
1. **数据库里的数据，也可能是攻击载体**  
  
1. **安全校验只做在“入口”，是远远不够的**  
  
1. **漏洞不一定当场爆炸，而可能“秋后算账”**  
  
从这一关开始，我明显感觉到：SQL 注入，已经不再是“技巧问题”，而是   
  
**对业务流程和数据生命周期的理解问题**  
。  
  
  
