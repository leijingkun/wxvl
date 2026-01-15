#  关于 VulnHunter -AI 漏洞猎人v6.0 版本发布推迟  
原创 暗夜铭少  黑帽渗透技术   2026-01-14 22:39  
  
由于这几天测试跟新增太多功能和不断反复测试，功能太多GUI界面装不下，反复调整依然未能得到解决，后面又转成了web界面，浪费不少时间，  
  
5.0版本有些卡顿是完美用错方式，功能太多了不能使用一种编程语言来写，现在6.0版本使用混合编程语言，性能是得到了质的飞升了，加上这个版本就是双系统 APP渗透，也就是支持安卓APP和  
iPhone做深度渗透，  
  
还有就是微信小程序跟支付宝小程序也支持，目前主要是针对这几个功能加强测试，导致时间推迟，前天很多人问什么时候发布新版本，完美说13号发布，但是由于遇到各种问题，所以未能如实发布，同时在这里也感谢大家大力支持，不离不弃。  
  
下面是web界面功能图片，也没有做美化。先给大家看看  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdaDEG6PoNrZfiaTmRxJVNg24xb61VjFhnCPZ6EgHK8HgMjlAQShVk37sQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0Kcsqnmda2xQ7avvbgKOVAgvW74regojWVtE4du24ZCulANriaUy8cQy2Tl5609w/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdawKFT7bZHN9SJO70qjONfwBK6hlv5FBTbWHfNHzEr1kXfBfyE0NicKXg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdaPXAdUJDVSxJFd7ngZh6c4iapdCC9IUibZpcfIXHMkBhQjAS2SDZYQnbQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdaicTcoWOrISejDdF5UtZIhvfVp4TOMjKSq054uktTlO6IrIEhia5icpEOg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdaibUvl6NvolCgsnicIrK3wrS1MNn7dM92bkXcXlhE4qdUDrLobohKdYAA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0Kcsqnmda1hnDlEDAHFibxrzCqTQA1PNzW2qJU3ybPiazjy3y2JeAE62PVuU7K5Yg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DfDsjZoLQS0BUhLXrSSkUem0KcsqnmdaNs6wPH6Ok1CIdW2N1t4rLLsK2qoOqtZGZmf05dsxqnib6ibbHrGCUZqA/640?wx_fmt=png&from=appmsg "")  
  
等全部测试和调整好了，大家做渗透测试不需要用到其他工具，这个工具就能完整做渗透包括渗透后功能都有的，  
# # Web界面功能与Scanner文件映射表  
# ## 1. Web漏洞扫描  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| Web扫描 | ai_scanner.go | AnalyzeTarget | web |  
  
| 后台扫描 | backend.go | ScanBackend | backend |  
  
| 路径遍历 | path_traversal.go | Scan | path |  
  
| 路径发现 | path_discovery.go | ScanPaths | discover |  
  
| 文件上传 | file_upload.go | ScanUpload | upload |  
  
| 安全特性 | security_detection.go | DetectSecurityFeatures | security |  
  
| 0day检测 | zero_day.go | DetectZeroDay | zero |  
  
| 技术栈分析 | tech_stack.go | AnalyzeTechStack | tech |  
  
| 指纹识别 | fingerprint.go | DetectFingerprint | fingerprint |  
# ## 2. 注入漏洞  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| SQL注入 | sqli.go | Scan | sqli |  
  
| XSS漏洞 | xss.go | Scan | xss |  
  
| 命令注入 | cmd_injection.go | Scan | cmdi |  
  
| XXE漏洞 | xxe.go | Scan | xxe |  
  
| 反序列化 | deserialization.go | Scan | deser |  
  
| 本地文件包含 | file_inclusion.go | ScanLFI | lfi |  
  
| 远程文件包含 | file_inclusion.go | ScanRFI | rfi |  
# ## 3. 漏洞利用  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| LFI利用 | file_inclusion_exploit.go | ExploitLFI | lfi_exploit |  
  
| RFI利用 | file_inclusion_exploit.go | ExploitRFI | rfi_exploit |  
  
| SSRF利用 | ssrf_exploit.go | Exploit | ssrf_exploit |  
  
| SSTI利用 | ssti_exploit.go | Exploit | ssti_exploit |  
  
| XXE利用 | xxe_exploit.go | Exploit | xxe_exploit |  
  
| RCE利用 | rce_exploit.go | Exploit | rce_exploit |  
  
| 反序列化利用 | deserialization_exploit.go | Exploit | deser_exploit |  
# ## 4. 渗透测试  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| Webshell管理 | webshell_manager.go | Manage | webshell |  
  
| 横向移动 | lateral_movement.go | Move | lateral |  
  
| Linux提权 | linux_privilege_escalation.go | Escalate | linux_privilege |  
  
| Windows提权 | windows_privilege_escalation.go | Escalate | windows_privilege |  
  
| 权限维持 | persistence_manager.go | Manage | persistence |  
  
| 数据窃取 | data_exfiltration.go | Exfiltrate | exfiltrate |  
# ## 5. 信息收集  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| Whois查询 | whois_query.go | Query | whois |  
  
| DNS枚举 | dns_enumeration.go | Enumerate | dns |  
  
| 网络隧道 | network_tunnel.go | CreateTunnel | tunnel |  
  
| 数据包分析 | packet_analyzer.go | Analyze | packet |  
  
| 会话重建 | session_reconstruction.go | Reconstruct | session |  
  
| 流量分析 | traffic_analyzer.go | Analyze | traffic |  
# ## 6. 网络工具  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| 端口扫描 | port_scanner.go | Scan | port_scan |  
  
| 子域名扫描 | subdomain.go | Enumerate | subdomain |  
  
| 网络隧道 | network_tunnel.go | CreateTunnel | tunnel |  
  
| 数据包分析 | packet_analyzer.go | Analyze | packet |  
  
| 会话重建 | session_reconstruction.go | Reconstruct | session |  
  
| 流量分析 | traffic_analyzer.go | Analyze | traffic |  
# ## 7. 移动安全  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| Android APK | apk_analyzer.go | Analyze | apk |  
  
| iOS IPA | ipa_analyzer.go | Analyze | ipa |  
  
| 微信小程序 | miniprogram_analyzer.go | Analyze | wechat |  
  
| 支付宝小程序 | miniprogram_analyzer.go | Analyze | alipay |  
# ## 8. 工具管理  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| 字典管理 | dictionary.go | Manage | dict |  
  
| Payload管理 | custom_payload.go | Manage | payload |  
  
| 暴力破解 | login_brute.go | Brute | brute |  
# ## 9. AI助手  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| AI扫描 | ai_scanner.go | AnalyzeTarget | ai |  
# ## 10. 数据包工具  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| 数据包捕获 | packet_capture.go | Capture | capture |  
  
| 数据包伪造 | packet_forge.go | Forge | forge |  
  
| 数据包代理 | packet_proxy.go | Proxy | proxy |  
# ## 11. 反向Shell  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| 反向Shell | reverse_shell.go | Listen | reverse_shell |  
# ## 12. UAC绕过  
  
| Web界面功能 | Scanner文件 | 对应函数/类型 | API参数 |  
  
|------------|-------------|--------------|---------|  
  
| UAC绕过 | uac_windows.go | Bypass | uac |  
  
=====================================  
  
测试APK分析功能而创建的Android应用，包含多种常见的安全漏洞。  
# ## 包含的漏洞类型  
## ### 1. 硬编码敏感信息  
- **API密钥**  
: OpenAI API Key, AWS Access Key, Google API Key  
  
- **第三方密钥**  
: Facebook App ID/Secret, Twitter Consumer Key/Secret  
  
- **数据库凭据**  
: 数据库密码硬编码在代码中  
  
- **加密密钥**  
: AES加密密钥硬编码  
  
## ### 2. SQL注入  
- **登录功能**  
: 直接拼接用户输入到SQL查询  
  
- **搜索功能**  
: 使用LIKE语句直接拼接搜索词  
  
- **ContentProvider**  
: query方法存在SQL注入漏洞  
  
## ### 3. 不安全的加密  
- **ECB模式**  
: 使用AES/ECB/PKCS5Padding（不安全的加密模式）  
  
- **硬编码密钥**  
: 加密密钥硬编码在代码中  
  
- **弱哈希算法**  
: 使用MD5进行密码哈希  
  
## ### 4. 组件导出漏洞  
- **导出的Activity**  
: VulnerableActivity设置为exported=true  
  
- **导出的Service**  
: VulnerableService设置为exported=true  
  
- **导出的BroadcastReceiver**  
: VulnerableReceiver设置为exported=true  
  
- **导出的ContentProvider**  
: VulnerableProvider设置为exported=true  
  
## ### 5. 不安全的Intent处理  
- **命令注入**  
: 通过Intent传递的命令可能被执行  
  
- **敏感数据泄露**  
: 通过Intent传递敏感数据  
  
- **未验证的Intent**  
: 不验证Intent来源就处理数据  
  
## ### 6. 权限过度申请  
- **危险权限**  
: READ_PHONE_STATE, READ_CONTACTS, SEND_SMS, CALL_PHONE  
  
- **不必要权限**  
: 同时申请了多个敏感权限  
  
## ### 7. 网络安全配置  
- **允许明文流量**  
: android:usesCleartextTraffic="true"  
  
- **调试模式**  
: android:debuggable="true"  
  
## ### 8. 日志信息泄露  
- **敏感日志**  
: API密钥、密码等敏感信息被记录到Logcat  
  
- **调试信息**  
: 包含详细的调试日志  
  
我们尽量这两天再发布6.0版本，不会让大家等太久。后面我们还会推出更高端工具，也就是硬件渗透，可以把木马植入到编程硬件，比如充电宝硬件之类任何其他通信硬件。都是可以植入木马的，这个太高端，我们做出了也只是做演示，不会运用到市场，也就是教大家怎么防这种木马，保护个人财产安全。  
  
  
