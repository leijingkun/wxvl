#  Linux电池实用程序漏洞允许绕过身份验证和篡改系统  
原创 网络安全9527  安全圈的那点事儿   2026-01-09 00:01  
  
Linux 笔记本电脑用户被敦促进行更新，因为一款流行的电池优化工具被发现存在漏洞，该漏洞允许 绕过身份验证 和篡改系统。    
  
该漏洞影响版本 1.9.0 中引入的 TLP 电源配置文件守护程序，该守护程序公开了一个 D-Bus API， 用于以 root 权限管理电源配置文件。   
  
TLP 的配置文件守护程序以 root 用户身份运行，并使用 Polkit 来决定是否允许客户端更改电源配置文件或日志记录设置。   
  
在 1.9.0 版本中，守护进程错误地依赖于 Polkit 已弃用的“unix-process”主题，仅传递调用者的 PID 进行授权。   
  
这会造成一种竞争条件，即当 Polkit 评估 PID 时，该 PID 可能已经被权限更高的进程回收利用，从而允许本地用户绕过身份验证并获得对 TLP 设置的更高控制权。   
  
 该问题已被分配 CVE-2025-67859，允许任何本地用户在不提供管理员凭据的情况下  任意更改活动电源配置文件和 守护程序日志配置。  
  
TLP 1.9.1 用更安全的 D-Bus“系统总线名称”主题取代了这种脆弱的机制，它将授权与实际的客户端连接绑定，而不是与容易发生竞争的 PID 绑定。   
  
研究人员还发现了一些相关的弱点，虽然这些弱点的影响通常较小，但仍然扩大了攻击面。   
- HoldProfile/ReleaseProfile API 使用可预测的递增整数“cookie”值，允许其他用户或进程猜测 cookie 并释放他们没有创建的配置文件保留。   
- 将非整数 cookie 传递给 ReleaseProfile 会触发未处理的 Python 异常，虽然不会使守护进程崩溃，但会降低其健壮性。   
- 该守护进程允许无限数量的配置文件保留，允许本地用户将任意字符串推送到内部字典中，并可能通过资源耗尽导致拒绝服务。   
上游通过切换到随机、不可预测的 cookie、加强类型处理以及将同时保持的配置文件数量限制为 16 来解决这些问题。   
  
<table><tbody><tr style="box-sizing: border-box;background-color: rgb(240, 240, 240);"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong style="box-sizing: border-box;font-weight: bold;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">CVE ID</span></font></font></strong><section><span leaf=""> </span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong style="box-sizing: border-box;font-weight: bold;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">受影响的组件</span></font></font></strong><section><span leaf=""> </span></section></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><strong style="box-sizing: border-box;font-weight: bold;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">严重程度/备注</span></font></font></strong><section><span leaf=""> </span></section></td></tr><tr style="box-sizing: border-box;"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">CVE-2025-67859 </span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">TLP 1.9.0 配置文件守护进程 </span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">高危——绕过身份验证，本地 root 控制的守护进程。 </span></font></font></td></tr></tbody></table>  
Polkit 的主要绕过漏洞被追踪为 CVE-2025-67859，而可预测的 cookie 和无限制的配置文件持有被判定为低严重性，并且没有被赋予单独的标识符，这与上游一致。   
  
行为成功利用后，本地用户可以篡改电源策略和相关的守护进程行为，这在严格控制的环境中可能会破坏对性能、日志记录和系统行为的安全预期。   
  
根据SUSE 安全团队的说法，这些问题是在协调披露流程下报告的，上游补丁于 2025 年 12 月开发，并于 2026 年 1 月 7 日作为 TLP 1.9.1 发布。   
  
 建议用户通过分发包升级到 TLP 1.9.1 或更高版本，并确保只有受信任的本地用户才能访问 D-Bus 和系统电源管理接口。   
  
