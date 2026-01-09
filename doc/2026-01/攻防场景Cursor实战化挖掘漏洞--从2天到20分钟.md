#  攻防场景Cursor实战化挖掘漏洞--从2天到20分钟  
原创 Hyyrent  0xSecurity   2026-01-09 07:19  
  
## Cursor挖掘漏洞成果展示  
  
先看看AI输出的漏洞审计报告，准确率基本达到80%，如果没有企业会员可以去闲鱼购买一个  
  
![image-20260109150839854](https://mmbiz.qpic.cn/sz_mmbiz_png/umfmicibSEUbh2SjqVbWL0U7rypeEOenGKicEAwVF7qOms6ickoNKe1uTdW3JQx6qPoD06y2ZY17OrHAcpWBQGE0NA/640?wx_fmt=png&from=appmsg "")  
## 一、为什么选择 Cursor 做代码审计？  
  
传统 Java 代码审计的痛点：  
- 项目体量大，调用链复杂  
  
- 手工追踪 Controller → Service → DAO 成本极高  
  
- 容易漏掉“隐藏参数”“内部逻辑分支”  
  
- 扫描器噪声大，人工二次确认成本高  
  
**Cursor 的优势在于：**  
- 能自动拉取上下文（跨文件 / 跨层级）  
  
- 擅长“沿变量 / 参数”扩展分析  
  
- 适合灰盒场景（已有接口 / 流量 / 入口）  
  
> Cursor ≠ 自动扫描器  
  
 Cursor =   
**代码审计加速器**  
  
## 二、整体审计思路（强烈推荐）  
```
```  
  
Cursor 的正确用法是：  
  
  
**先缩小范围，再让 AI 深挖。**  
## 三、阶段一：Web 配置与接口暴露面分析  
### 3.1 web.xml / Spring 配置分析  
  
重点关注以下配置点：  
#### web.xml  
- <servlet>  
  
- <servlet-mapping>  
  
- <url-pattern>  
  
Cursor 提示词示例：  
```
```  
#### Spring MVC / Spring Boot  
  
重点注解：  
- @Controller  
  
- @RestController  
  
- @RequestMapping  
  
- @GetMapping  
 /   
@PostMapping  
  
Cursor 使用技巧：  
- 打开 Controller 文件  
  
- 让 Cursor 自动汇总路径  
  
示例提示词：  
```
```  
### 3.2 鉴权绕过配置检查  
  
重点关注：  
- Filter  
  
- Interceptor  
  
- SecurityConfig  
  
- WebSecurityConfigurerAdapter  
  
Cursor 提示词：  
```
```  
## 四、阶段二：classes / lib / jar 源码分析  
### 4.1 WEB-INF/classes 分析  
- 对比   
.class  
 与反编译   
.java  
  
- 注意是否存在未部署源码  
  
Cursor 辅助点：  
- 帮你快速理解反编译代码  
  
- 标记关键逻辑分支  
  
### 4.2 lib 目录下 jar 包审计  
  
重点关注：  
- 是否包含业务逻辑  
  
- 是否存在内部接口实现  
  
- 是否存在调试 / 管理功能  
  
Cursor 提示词：  
```
```  
## 五、阶段三：调用链路识别（Cursor 最强能力）  
### 5.1 入口函数确认  
  
常见入口：  
- request.getParameter()  
  
- @RequestParam  
  
- @RequestBody  
  
- HttpServletRequest  
  
### 5.2 前置知识输入  
  
不同的语言审计的方法和思路不一样，在让AI分析代码时候需要提供一些前置知识，这能让 AI 更精确地聚焦在“可能的风险点”，而不是泛泛地猜测  
  
像SQL注入，不同语言的sink点也不完全相同  
  
<table><thead><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;"><th style="box-sizing: border-box;padding: 6px 13px;font-weight: bold;border-width: 1px 1px 0px;border-top-style: solid;border-right-style: solid;border-left-style: solid;border-top-color: rgb(223, 226, 229);border-right-color: rgb(223, 226, 229);border-left-color: rgb(223, 226, 229);border-image: initial;border-bottom-style: initial;border-bottom-color: initial;margin: 0px;"><span cid="n629" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">语言</span></span></span></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: bold;border-width: 1px 1px 0px;border-top-style: solid;border-right-style: solid;border-left-style: solid;border-top-color: rgb(223, 226, 229);border-right-color: rgb(223, 226, 229);border-left-color: rgb(223, 226, 229);border-image: initial;border-bottom-style: initial;border-bottom-color: initial;margin: 0px;"><span cid="n630" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">方法 / 函数</span></span></span></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: bold;border-width: 1px 1px 0px;border-top-style: solid;border-right-style: solid;border-left-style: solid;border-top-color: rgb(223, 226, 229);border-right-color: rgb(223, 226, 229);border-left-color: rgb(223, 226, 229);border-image: initial;border-bottom-style: initial;border-bottom-color: initial;margin: 0px;"><span cid="n631" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">示例代码 / SQL</span></span></span></th></tr></thead><tbody><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n633" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Golang</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n634" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">(*gorm.io/gorm).Where</span></code></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n635" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">db.Where(StringData).First(&amp;data)</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;background-color: rgb(248, 248, 248);"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n637" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Golang</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n638" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">(*github.com/jmoiron/sqlx.DB).Queryx</span></code></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n639" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">db.Queryx(query, params)</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n641" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Java</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n642" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">mybatis like</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n643" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">select * from users where username like &#39;%${name}%&#39;</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;background-color: rgb(248, 248, 248);"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n645" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Java</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n646" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">mybatis order by</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n647" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">select * from users order by ${orderby}</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n649" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Java</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n650" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">mybatis-plus apply</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n651" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">wrapper.eq(&#34;id&#34;, id).apply(&#34;username=&#34; + name);</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;background-color: rgb(248, 248, 248);"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n653" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Python</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n654" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">pymysql execute</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n655" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">sql = &#34;select * from users where username = &#39;%s&#39;&#34; % (name)</span></code></span></span></td></tr><tr style="box-sizing: border-box;break-inside: avoid;break-after: auto;border: 1px solid rgb(223, 226, 229);margin: 0px;padding: 0px;"><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n657" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 69.4875px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">Node.js</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n658" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 383.888px;min-height: 10px;"><span md-inline="plain" style="box-sizing: border-box;"><span leaf="">mysql query</span></span></span></td><td style="box-sizing: border-box;padding: 6px 13px;border: 1px solid rgb(223, 226, 229);margin: 0px;min-width: 32px;"><span cid="n659" mdtype="table_cell" style="box-sizing: border-box;display: inline-block;min-width: 1ch;width: 604.625px;min-height: 10px;"><span md-inline="code" spellcheck="false" style="box-sizing: border-box;"><code style="box-sizing: border-box;font-family: var(--monospace);text-align: left;vertical-align: initial;border: 4px solid rgb(243, 244, 244);background-color: rgb(243, 244, 244);border-radius: 3px;padding: 0px 2px;font-size: 0.9em;color: rgb(223, 50, 96);"><span leaf="">sql = &#34;select * from users where username = ${name}&#34;</span></code></span></span></td></tr></tbody></table>  
在Shiro和Spring Security中，可以配置哪些API不需要进行权限校验  
- 在Shiro中，可以使用Shiro的过滤器链（Filter Chain）来配置不需要进行权限校验的API  
  
- 在Spring Security中，可通过继承WebSecurityConfigurerAdapter类并重写其中的configure()方法，配置不需要进行权限校验的API  
  
![image-20251230153740679](https://mmbiz.qpic.cn/sz_mmbiz_png/umfmicibSEUbh2SjqVbWL0U7rypeEOenGKvdu9gLNoDic312al6xzCDlbzVibSC2cGkjiak8Ws8DvPk4hESkXZMTHAA/640?wx_fmt=png&from=appmsg "")  
### 5.3 最终Cursor 提示词：  
```
```  
  
  
