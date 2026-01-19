#  CyberStrikeAI新增fofa_search MCP，自然语言一键搞定靶场发现与漏洞利用  
学安全也就图一乐
                    学安全也就图一乐  阿乐你好   2026-01-19 02:51  
  
CyberStrikeAI是一个AI驱动的自主渗透测试平台，基于Golang开发，内置数百个安全工具，通过MCP协议实现智能AI决策，使安全测试变得像对话一样简单。通过自然语言交互，AI可以自动分析目标、选择最佳测试策略、执行工具并给出专业建议。今天CyberStrikeAI最新版本新增fofa_search MCP功能。  
## 新增功能：fofa_search MCP  
  
fofa_search MCP是CyberStrikeAI与fofa搜索引擎的深度集成，允许用户通过自然语言直接查询fofa，快速定位目标资产。无需再手动编写复杂的fofa查询语法，只需一句话，AI就能帮你找到目标。  
## 实战演示：从靶场发现到漏洞验证  
  
让我们通过一个具体案例，展示CyberStrikeAI的智能化能力：  
  
> 用 @fofa_search 找一个pikachu靶场，输出100条，在50-100中随机选一个靶场， 再通过 @http-framework-test 测试一个命令执行漏洞并上传webshell（在eval页面测，不要再ping页面测），返回带webshell的报告  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YJchnEpjSxib4SmR4noxT5c6t5nNEoKQR6m4IwYibVzoknAeJEef04ZRQ/640?wx_fmt=png&from=appmsg "")  
  
**1. 靶场发现**  
  
AI自动调用fofa_search工具，搜索包含pikachu关键字的靶场，快速定位到可用的靶场IP。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YuPLB9BhyVVPJWfMS0vqdQICKD67bDBRvsmne4Ev9EBX3UY0pLm3Ouw/640?wx_fmt=png&from=appmsg "")  
  
**2. 漏洞测试**  
  
AI根据靶场信息，自动调用http-framework-test工具，检测目标是否具有命令执行漏洞，并尝试上传webshell。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YWCfkPe1opXXTia27MgiaJNSUoqiaayNWKrcU4oHdHeicGLwnEXMJlwS8aQ/640?wx_fmt=png&from=appmsg "")  
  
漏洞验证  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YOgWVpxNhjkYolD3BqshcENhictbZT7gADibicYmFNdff6xZjFI0e2su9A/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YoicruILCZwEmnrAr84PWchuM1TSjJTuoicibGFFz1nqqqDdgNqOgSvotA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YUFaSv3Vxfq562B9SjGvU6oA0dLqtuhL6gAicrp9C66aTJHsic73Ngrpg/640?wx_fmt=png&from=appmsg "")  
  
**3. 结果验证**  
  
AI自动验证上传的webshell是否有效，确认是否能成功执行命令。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YjxAOADzbjDoD4CVGCqnPKMfWQ8eWia7ibSSxGlqZnnPBndfgia3A6tpbg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YFgbkPcpk2AZ9Slz1xsT2qub1NXXhY7nJQibkeM8TqKYYQmFuiaSYtuBA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YbT0oiarNHS8IuzcnZ87UVt2kC1QvfGRpqJ8T7ib1XIuynTHCDJMx1ZEg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YrIFmBjLrMia4lYVKAbGoApeMfZVMk1rczMPMq15hH9CI6ME3ADiadX7w/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YDeiaGkbnLxdJtLqkRuhAEyzOoftIR1VP4icmyUMSuD4RBDYn48LG6t9g/640?wx_fmt=png&from=appmsg "")  
  
报告总结  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/KzNYA6icKe6moWZeBcrfRKNSKMyryAc5YxjyrJmNXbX8q3bzibytsL8d71ChTC9tuBtSuZJuib97aw7x7G8SiaStvA/640?wx_fmt=png&from=appmsg "")  
  
整个过程，用户只需用自然语言描述需求，AI会自动完成所有步骤，无需手动编写复杂命令或配置。  
  
## 如何开启fofa_search MCP？  
  
  
1、在fofa_search.yaml中或环境变量中配置  
 FOFA_EMAIL 和F  
OFA_API_KEY  
  
  
  # ==================== FOFA配置 ====================  
  
  
  # 请在此处配置您的FOFA账号信息  
  
  
  # 您也可以在环境变量中设置：FOFA_EMAIL 和 FOFA_API_KEY  
  
  
  FOFA_EMAIL = ""  # 请填写您的FOFA账号邮箱  
  
  
  FOFA_API_KEY = ""  # 请填写您的FOFA API密钥  
  
  
  # ==================================================  
  
2、配置  
fofa_search.yaml：  
  
enabled  
:   
true （默认为true）  
## 为什么选择CyberStrikeAI？  
- **智能决策**  
：AI自动分析目标，选择最佳测试策略和工具组合  
  
- **高效执行**  
：无需手动操作，AI自动执行测试流程  
  
- **结果清晰**  
：AI提供详细测试报告和下一步建议  
  
- **安全可靠**  
：工具执行隔离，错误处理和超时控制  
  
## 如何开始？  
  
只需3步：  
1. 访问GitHub项目：https://github.com/Ed1s0nZ/CyberStrikeAI  
1. 按照快速启动指南配置好API密钥和工具  
1. 用自然语言描述你的测试需求  
CyberStrikeAI让安全测试变得更简单、更智能。无论你是安全研究员、渗透测试工程师，还是安全爱好者，都能通过这个平台大幅提升工作效率。  
  
👉 **立即体验AI驱动的渗透测试新方式：**  
https://github.com/Ed1s0nZ/CyberStrikeAI  
  
