#  让 AI 像黑客一样思考，已斩获 49 个 CVE 的开源漏洞！全新一代AI代审工具漏洞发现效率提升 10 倍  
ez-lbz
                    ez-lbz  渗透安全HackTwo   2026-03-03 16:01  
  
0x01 工具介绍  
  
DeepAudit 是一个基于 Multi-Agent 协作架构的下一代代码安全审计平台（V3.0.4 于 2026-3-3 全新升级）。  
它不仅仅是一个静态扫描工具，而是模拟安全专家的思维模式，通过多个智能体Orchestrator, Recon, Analysis, Verification的自主协作，实现对代码的深度理解、漏洞挖掘和 自动化沙箱 PoC 验证。传统代码审计依赖人工、误报高、效率低，复杂业务逻辑与深层漏洞难以挖掘。如今，新一代 AI 多智能体审计工具彻底改变这一现状，它模拟黑客攻击思维，自主完成侦察、分析、验证全流程，已累计发现 49 个 CVE 漏洞，让代码安全审计从繁重手工变为高效自动化，人人都能拥有专业级审计能力。  
  
注意：  
现在只对常读和星标的公众号才展示大图推送，建议大家把  
**渗透安全HackTwo**  
"**设为****星标⭐️**  
"  
**否****则可能就看不到了啦！**  
  
**下载地址在末尾 #渗透安全HackTwo**  
  
0x02   
功能介绍  
  
📸 界面预览  
  
🤖 Agent 审计入口  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVmvJnUbMRxza0RaX3MRCsZ9tVPOibJgARkiaMpUkPiau0ibBFrvNuu3X3ChPtmHLicVzBrVNicZTqImVPnSdktPJjuAGbkX7eQ8GThs/640?wx_fmt=png&from=appmsg "")  
### 首页快速进入 Multi-Agent 深度审计  
<table><tbody><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.666667px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="50%" width="50%" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.666667px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 16px;">📋 审计流日志</span></span><section nodeleaf="" style="margin-top: 16px;margin-bottom: 0px;"><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUS4uwF1Z0aibOnv8YSlB5VG10NNnQK8Slu28zb9ZRhsWAox7hPBKpTxxWhHEAJvG2QW0KfCibRkIPzZSVBDbic6Sr2l6OdHnAeicM/640?wx_fmt=png&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="0.5351851851851852" data-s="300,640" data-type="png" data-w="1080" type="inline" data-imgfileid="100014121" data-aistatus="1"/></section><span leaf=""><span textstyle="" style="font-size: 16px;">实时查看 Agent 思考与执行过程</span></span></strong></td><td data-colwidth="50%" width="50%" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.666667px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 16px;">🎛️ 智能仪表盘</span></span><section nodeleaf="" style="margin-top: 16px;margin-bottom: 0px;"><img data-src="https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAWAak6WquZHR07ZaJiahS5AQxjQ17rcsRFEkBWPpqEjgvKA0e3KPd2Man2VD02En95VVLyO18vfMuDbwOOtwQeUJWst9rzrbz2E/640?wx_fmt=png&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="0.5398148148148149" data-s="300,640" data-type="png" data-w="1080" type="inline" data-imgfileid="100014122" data-aistatus="1"/></section><span leaf=""><span textstyle="" style="font-size: 16px;">一眼掌握项目安全态势</span></span></strong></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.666667px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="50%" width="50%" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.666667px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 16px;">⚡ 即时分析</span></span><section nodeleaf="" style="margin-top: 16px;margin-bottom: 0px;"><img data-src="https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXcnlZtQ0Ehaq92O9fAu6ftZj31ibhxBxQQF5Vj1eS3m7UB6gqFSTtIDH5UjhIkAj5BfDcd7icebh1nib79X07yUCCDzFPWeM8gVE/640?wx_fmt=png&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="0.5305555555555556" data-s="300,640" data-type="png" data-w="1080" type="inline" data-imgfileid="100014119" data-aistatus="1"/></section><span leaf=""><span textstyle="" style="font-size: 16px;">粘贴代码 / 上传文件，秒出结果</span></span></strong></td><td data-colwidth="50%" width="50%" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.666667px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 16px;">🗂️ 项目管理</span></span><section nodeleaf="" style="margin-top: 16px;margin-bottom: 0px;"><img data-src="https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAWT7gHynZuVNPKGFhiakBzG7CEAmWKUVHpoIKbsxR02JMYTzv4GBHNOMANBVaK49ECWOuyKjwTaUv6RFiaibxXeCD0CEwXbqBydJs/640?wx_fmt=png&amp;from=appmsg" class="rich_pages wxw-img" data-ratio="0.5462962962962963" data-s="300,640" data-type="png" data-w="1080" type="inline" data-imgfileid="100014123" data-aistatus="1"/></section><span leaf=""><span textstyle="" style="font-size: 16px;">GitHub 导入，多项目协同管理</span></span></strong></td></tr></tbody></table>### 📊 专业报告  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVXIfG05DicadJ1icjOaK0WlQOmibaAWtSwP25MwBL72hZvg7C8BspXYz8r7W7eS61wibz66I7CVhe53gYkzEkxpkJWcP5qdVfU8F0/640?wx_fmt=png&from=appmsg "")  
## 💡 为什么选择 DeepAudit？  
<table><thead><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1.33333px solid rgba(209, 217, 224, 0.7);"><th align="left" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">😫 传统审计的痛点</span></span></section></th><th align="left" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">💡 DeepAudit 解决方案</span></span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1.33333px solid rgba(209, 217, 224, 0.7);"><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">人工审计效率低</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">跨不上 CI/CD 代码迭代速度，拖慢发布流程</span></span></section></section></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">🤖 Multi-Agent 自主审计</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">AI 自动编排审计策略，全天候自动化执行</span></span></section></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 1.33333px solid rgba(209, 217, 224, 0.7);"><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">传统工具误报多</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">缺乏语义理解，每天花费大量时间清洗噪音</span></span></section></section></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">🧠 RAG 知识库增强</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">结合代码语义与上下文，大幅降低误报率</span></span></section></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1.33333px solid rgba(209, 217, 224, 0.7);"><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">数据隐私担忧</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">担心核心源码泄露给云端 AI，无法满足合规要求</span></span></section></section></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">🔒 支持 Ollama 本地部署</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">数据不出内网，支持 Llama3/DeepSeek 等本地模型</span></span></section></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 1.33333px solid rgba(209, 217, 224, 0.7);"><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">无法确认真实性</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">外包项目漏洞多，不知道哪些漏洞真实可被利用</span></span></section></section></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1.33333px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;"><span leaf=""><span textstyle="" style="font-size: 14px;">💥 沙箱 PoC 验证</span></span></strong><section><span leaf=""><br/></span><section style="margin-bottom: 0px;margin-top: 16px;"><span leaf=""><span textstyle="" style="font-size: 14px;">自动生成并执行攻击脚本，确认漏洞真实危害</span></span></section></section></td></tr></tbody></table>  
我们致力于解决传统 SAST 工具的三大痛点：  
- **误报率高**  
 — 缺乏语义理解，大量误报消耗人力  
  
- **业务逻辑盲点**  
 — 无法理解跨文件调用和复杂逻辑  
  
- **缺乏验证手段**  
 — 不知道漏洞是否真实可利用  
  
用户只需导入项目，DeepAudit 便全自动开始工作：识别技术栈 → 分析潜在风险 → 生成脚本 → 沙箱验证 → 生成报告，最终输出一份专业审计报告。  
> **核心理念**  
: 让 AI 像黑客一样攻击，像专家一样防御。  
  
### 整体架构图  
### DeepAudit 采用微服务架构，核心由 Multi-Agent 引擎驱动。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAXNUiamtU0g9w5jXtrds0k6IWNpQsiaunqiadXZhegLTw7EDdM6mR5SAcPkiaibammnqfL2dx1ul4lkiaVlclRicVljNR5MCQC7icGFSuY/640?wx_fmt=png&from=appmsg "")  
### 🔄 审计工作流  
<table><thead><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1px solid rgba(209, 217, 224, 0.7);"><th data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">步骤</span></span></section></th><th data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">阶段</span></span></section></th><th data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">负责 Agent</span></span></section></th><th align="left" style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">主要动作</span></span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">1</span></span></section></td><td data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">策略规划</span></span></strong></td><td data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">Orchestrator</span></span></strong></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">接收审计任务，分析项目类型，制定审计计划，下发任务给子 Agent</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 1px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">2</span></span></section></td><td data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">信息收集</span></span></strong></td><td data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">Recon Agent</span></span></strong></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">扫描项目结构，识别框架/库/API，提取攻击面（Entry Points）</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">3</span></span></section></td><td data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">漏洞挖掘</span></span></strong></td><td data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">Analysis Agent</span></span></strong></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">结合 RAG 知识库与 AST 分析，深度审查代码，发现潜在漏洞</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 1px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">4</span></span></section></td><td data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">PoC 验证</span></span></strong></td><td data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">Verification Agent</span></span></strong></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">(关键)</span></span></strong><section><span leaf=""><span textstyle="" style="font-size: 14px;"> 编写 PoC 脚本，在 Docker 沙箱中执行。如失败则自我修正重试</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 1px solid rgba(209, 217, 224, 0.7);"><td data-colwidth="56" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">5</span></span></section></td><td data-colwidth="95" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">报告生成</span></span></strong></td><td data-colwidth="137" align="center" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><strong style="box-sizing: border-box;font-weight: 600;margin-bottom: 0px;"><span leaf=""><span textstyle="" style="font-size: 14px;">Orchestrator</span></span></strong></td><td align="left" style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 1px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf=""><span textstyle="" style="font-size: 14px;">汇总所有发现，剔除被验证为误报的漏洞，生成最终报告</span></span></section></td></tr></tbody></table>###   
  
0x03更新说明  
```
🛠️ 核心功能优化 ✨V3.0.4（2026-03-03）更新
🤖 优化 Agent 协作能力：修复 Agent 间 TaskHandoff 通信机制，多智能体分工协作更稳定
🔍 提升分支选择体验：分支选择功能新增搜索能力，解决分支过多时无法快速定位选择的问题（#143）
⚙️ 增强模型适配性：嵌入模型向量维度支持自定义配置，解决 Ollama 不同参数规模模型维度不匹配的兼容问题
⏱️ 优化 LLM 超时策略：Agent 审计的 LLM 超时时间支持配置化，告别硬编码 30s 超时的限制
🎨 统一交互体验：对齐项目详情页「启动审计」弹窗与项目管理页样式 / 逻辑，操作更统一

🔒 安全防护升级 🛡️
修复 SSRF 防护实现漏洞，强化内网安全防护能力
通过固定返回时间的方式，规避 SSRF 扫描内网端口的风险

📌 其他更新
合并社区贡献分支（#145），同步主分支最新迭代内容
常规功能迭代与细节优化
```  
  
0x04 使用介绍  
  
📦安装  
指南  
### 方式一：一行命令部署（推荐）  
### 使用预构建的 Docker 镜像，无需克隆代码，一行命令即可启动：  
  
```
curl -fsSL https://raw.githubusercontent.com/lintsinghua/DeepAudit/v3.0.0/docker-compose.prod.yml | docker compose -f - up -d
```  
## 🇨🇳 国内加速部署（作者亲测非常无敌之快）  
  
使用南京大学镜像站加速拉取 Docker 镜像（将 ghcr.io  
 替换为 ghcr.nju.edu.cn  
）：  
  
```
# 国内加速版 - 使用南京大学 GHCR 镜像站
curl -fsSL https://raw.githubusercontent.com/lintsinghua/DeepAudit/v3.0.0/docker-compose.prod.cn.yml | docker compose -f - up -d
```  
  
手动拉取镜像（如需单独拉取）（点击展开）  
```
# 前端镜像
docker pull ghcr.nju.edu.cn/lintsinghua/deepaudit-frontend:latest
# 后端镜像
docker pull ghcr.nju.edu.cn/lintsinghua/deepaudit-backend:latest
# 沙箱镜像
docker pull ghcr.nju.edu.cn/lintsinghua/deepaudit-sandbox:latest
```  
  
💡 镜像源由   
南京大学开源镜像站  
 提供支持### 💡 配置 Docker 镜像加速（可选，进一步提升拉取速度）（点击展开）  
  
  
如果拉取镜像仍然较慢，可以配置 Docker 镜像加速器。编辑 Docker 配置文件并添加以下镜像源：  
  
**Linux / macOS**  
：编辑 /etc/docker/daemon.json  
  
**Windows**  
：右键 Docker Desktop 图标 → Settings → Docker Engine  
```
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://dockerproxy.com",
    "https://hub.rat.dev"
  ]
}
```  
  
保存后重启 Docker 服务：  
```
# Linux
sudo systemctl restart docker
# macOS / Windows
# 重启 Docker Desktop 应用
```  
  
🎉 **启动成功！**  
 访问   
http://localhost:3000  
 开始体验。###   
  
  
### 方式二：克隆代码部署  
### 适合需要自定义配置或二次开发的用户：  
  
  
```
# 1. 下载项目
cd DeepAudit
# 2. 配置环境变量
cp backend/env.example backend/.env
# 编辑 backend/.env 填入你的 LLM API Key
# 3. 一键启动
docker compose up -d
```  
> 首次启动会自动构建沙箱镜像，可能需要几分钟。  
  
## 🔧 源码开发指南  
  
适合开发者进行二次开发调试。  
### 环境要求  
- Python 3.11+  
  
- Node.js 20+  
  
- PostgreSQL 15+  
  
- Docker (用于沙箱)  
  
### 1. 手动启动数据库  
```
docker compose up -d redis db adminer
```  
### 2. 后端启动  
```
cd backend
# 配置环境
cp env.example .env
# 使用 uv 管理环境（推荐）
uv sync
source .venv/bin/activate
# 启动 API 服务
uvicorn app.main:app --reload
```  
### 3. 前端启动  
```
cd frontend
# 配置环境
cp .env.example .env
pnpm install
pnpm dev
```  
### 3. 沙箱环境  
  
开发模式下需要本地 Docker 拉取沙箱镜像：  
```
# 标准拉取
docker pull ghcr.io/lintsinghua/deepaudit-sandbox:latest
# 国内加速（南京大学镜像站）
docker pull ghcr.nju.edu.cn/lintsinghua/deepaudit-sandbox:latest
```  
  
  
###   
  
###   
  
  
**0x05 内部VIP星球介绍-V1.4（福利）**  
  
          
如果你想学习更多**渗透测试技术/应急溯源/免杀工具/挖洞SRC赚取漏洞赏金/红队打点等**  
欢迎加入我们**内部星球**  
可获得内部工具字典和享受内部资源和  
内部交流群，  
**每天更新1day/0day漏洞刷分上分****(2026POC更新至5412+)**  
**，**  
包含全网一些**付费扫描****工具及内部原创的Burp自动化漏****洞探测插件/漏扫工具等，AI代审工具，最新挖洞技巧等**  
。shadon/Zoomeye/Quake/  
Fofa高级会员/  
AI账号/CTFShow等各种账号会员共享。详情点击下方链接了解，觉得价格高的师傅后台回复"   
**星球**  
 "有优惠券名额有限先到先得  
**❗️**  
啥都有  
**❗️**  
全网资源  
最新  
最丰富  
**❗️****（🤙截止目前已有2500+多位师傅选择加入❗️早加入早享受）**  
  
****  
最新漏洞情报分享：  
https://t.zsxq.com/VuWGw  
  
****  
  
**👉****点击了解加入-->>内部VIP知识星球福利介绍V1.5版本-1day/0day漏洞库及内部资源更新**  
  
****  
  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
**公众号回复20260304获取下载、回复"加群"获取交流群**  
  
# 最后必看-免责声明  
  
  
      
文章中的案例或工具仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，勿用于非法行为。如  
用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用。  
如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章或来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
  
# 往期推荐  
  
  
**1.内部VIP知识星球福利介绍V1.5（AI自动化）**  
  
**2.CS4.8-CobaltStrike4.8汉化+插件版**  
  
**3.全新升级BurpSuite2026.2专业(稳定版)**  
  
**4. 最新xray1.9.11高级版下载Windows/Linux**  
  
**5. 最新HCL AppScan Standard**  
  
  
渗透安全HackTwo  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
  
上一篇文章：  
[Nacos配置文件攻防思路总结|揭秘Nacos被低估的攻击面](https://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247492839&idx=1&sn=b6f091114fbd8e8922153a996c8f4f1c&scene=21#wechat_redirect)  
  
  
