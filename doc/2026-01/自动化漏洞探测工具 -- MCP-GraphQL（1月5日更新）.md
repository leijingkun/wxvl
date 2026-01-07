#  自动化漏洞探测工具 -- MCP-GraphQL（1月5日更新）  
Lserein  Web安全工具库   2026-01-07 08:53  
  
各类资料学习下载合集    
  
链接：https://pan.quark.cn/s/770d9387db5f  
  
===================================  
  
**免责声明**  
  
请勿利用文章内的相关技术从事非法测试，由于传播、利用此文所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，作者不为此承担任何责任。工具来自网络，  
安全性自测  
，  
大家都要把工具当做病毒对待，在虚拟机运行。  
如有侵权请联系删除。个人微信：  
ivu123ivu  
  
  
**0x01 工具介绍**  
  
Model-assisted Cyber Penetration for GraphQL 一个轻量级、AI 驱动的 GraphQL 自动化漏洞探测工具。利用大语言模型（LLM）智能分析 Schema，自动构造并验证 SSRF、RCE、信息泄露等漏洞。功能特性：  
  
```
✅ 自动 GraphQL 指纹识别（支持 100+ 常见路径）
✅ 内省（Introspection）泄露检测与 Schema 获取
✅ 完整内省查询：获取所有类型、枚举、输入类型、接口信息
✅ Mutation & Query 参数自动提取与风险分析
✅ AI 驱动：大模型生成 SSRF/RCE/SQLi/信息泄露 Payload
✅ 智能 Fuzzing：AI 分析响应并迭代优化 Payload（默认启用，3 轮迭代）
✅ 智能字段处理：使用__typename 避免猜测字段错误
✅ 多维度 RCE 检测：时间盲注 + 回显检测（whoami/id）+ OAST 外连
✅ 自动漏洞验证（OAST + 时间盲注 + 关键词匹配）
✅ 自动错误修复：GraphQL 语法错误自动修复与重试
✅ HTML 报告生成：精美的漏洞报告，支持 HTML/JSON/Markdown 格式
✅ 认证支持：自定义 Headers、Cookies、认证文件
✅ 代理支持：HTTP/HTTPS/SOCKS5 代理，方便与 Burp Suite 联动
✅ 清晰的控制台彩色输出
✅ 支持多种 LLM（Qwen、Ollama/Llama3）
```  
  
**0x02 安装与使用**  
  
常用命令：  
```
# 最简单用法：直接启用 AI Fuzz 模式（默认 3 轮迭代）
python mcp-graphql.py --url https://target.com

# 使用 Qwen（默认）
python mcp-graphql.py --url https://target.com --oast-domain yourid.oastify.com

# 使用本地 Llama3
python mcp-graphql.py --url https://target.com --model llama3

# 跳过 LLM，仅做基础扫描
python mcp-graphql.py --url https://target.com --skip-llm

# 禁用 AI Fuzz，使用传统单次生成模式
python mcp-graphql.py --url https://target.com --no-fuzz

# 自定义迭代次数
python mcp-graphql.py --url https://target.com --max-iterations 5

# 保存报告（支持 .html, .json, .md 格式）
python mcp-graphql.py --url https://target.com --output report.html
```  
  
运行界面  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8H1dCzib3UibsGwB1cfvAjvEOUr6ZqXc4HSWMTdicav7fug2fSCobprZWB8tyI7DsCSDiazWDAeiaLlRL2NJG211eLw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8H1dCzib3UibsGwB1cfvAjvEOUr6ZqXc4HqYXYr0Yb9zruq2UeoZAvOlLvzNHAYibtk0uVuic0AAqe32uh9NZ7VDSQ/640?wx_fmt=png&from=appmsg "")  
  
<table><tbody><tr><td data-colwidth="287"><section><span leaf=""><img class="rich_pages wxw-img" data-aistatus="1" data-imgfileid="100034149" data-ratio="1.4015518913676042" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/8H1dCzib3UibuzZMmDJXkGjLC9jdfcib0wMKwrhmbpdz9YMaPIOWNqeHbkALhuHouhdhZGY2I2ia7TIQdTibPfBjjZg/640?wx_fmt=jpeg&amp;from=appmsg" data-w="1031" type="inline"/></span></section></td><td data-colwidth="287"><section><span leaf=""><img class="rich_pages wxw-img" data-aistatus="1" data-imgfileid="100033627" data-ratio="1.2469635627530364" data-s="300,640" data-src="https://mmbiz.qpic.cn/sz_mmbiz_jpg/8H1dCzib3UibsC4yYFwgTnJrN0q57DearHJhaWSE6XQllpkUviaibg5MqTYgdUQYDNt8ysfV2v6o4jsN34pmq3DAOg/640?wx_fmt=jpeg&amp;from=appmsg" data-type="jpeg" data-w="1235" type="inline"/></span></section></td></tr></tbody></table>  
****  
  
  
****  
  
