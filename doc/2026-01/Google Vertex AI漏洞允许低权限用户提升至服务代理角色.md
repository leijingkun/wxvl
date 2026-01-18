#  Google Vertex AI漏洞允许低权限用户提升至服务代理角色  
原创 网络安全9527
                    网络安全9527  安全圈的那点事儿   2026-01-18 04:40  
  
安全研究人员在谷歌的 Vertex AI 平台中发现了严重的权限提升漏洞，攻击者只需极少的权限即可劫持高权限的服务代理账户。  
  
这些缺陷会影响 Vertex AI Agent Engine 和 Vertex AI 上的 Ray，其中默认配置允许低权限用户访问具有项目级权限的强大托管身份。  
  
随着企业快速部署生成式 AI 基础设施（目前 98% 的企业正在试验或实施Google Cloud Vertex AI 等平台），这些被忽视的身份风险对云环境构成了重大威胁。  
  
服务代理是由 Google Cloud 管理的特殊服务账户，代表用户执行内部操作，通常会自动获得广泛的权限。  
  
研究人员发现了两种不同的攻击途径，可以将这些“隐形”的受管身份转化为可利用的权限提升途径。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/pcgSUGCDdKIuSIuvQuLewnjHPuBWf4ZO8F4ibNawoDZPiaygTiagYTQjgZcy3c2NzZibaDzdG7LhFNiaXgbsaqh11uw/640?wx_fmt=png&from=appmsg "")  
  
当谷歌得知此事后回应称，这些服务“运行正常”，这意味着这些配置目前仍是默认配置。  
  
平台工程师和安全团队必须了解这些技术机制，才能立即保护他们的环境。  
## Vertex AI 代理引擎工具注入漏洞  
  
第一个漏洞针对的是 Vertex AI Agent Engine，它使开发人员能够使用 Google 的 ADK 等框架在 GCP 基础架构上部署 AI 代理。  
<table><thead><tr style="box-sizing: border-box;"><th style="box-sizing: border-box;padding: 2px 8px;text-align: left;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">特征</span></font></font></th><th style="box-sizing: border-box;padding: 2px 8px;text-align: left;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">Vertex AI 代理引擎</span></font></font></th><th style="box-sizing: border-box;padding: 2px 8px;text-align: left;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><span style="color: rgb(19, 31, 73);font-family: Poppins;font-size: 16px;font-style: normal;font-variant-ligatures: normal;font-variant-caps: normal;font-weight: 700;letter-spacing: normal;orphans: 2;text-align: left;text-indent: 0px;text-transform: none;widows: 2;word-spacing: 0px;-webkit-text-stroke-width: 0px;background-color: rgb(255, 255, 255);text-decoration-thickness: initial;text-decoration-style: initial;text-decoration-color: initial;display: inline !important;float: none;" data-pm-slice="0 0 []"><span leaf="">Ray on Vertex AI</span></span></th></tr></thead><tbody><tr style="box-sizing: border-box;background-color: rgb(240, 240, 240);"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">主要目标</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">推理引擎服务代理</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">自定义代码服务代理</span></font></font></td></tr><tr style="box-sizing: border-box;"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">漏洞类型</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">恶意工具调用（远程代码执行）</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">不安全的默认访问权限（查看者到根目录）</span></font></font></td></tr><tr style="box-sizing: border-box;background-color: rgb(240, 240, 240);"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">初始许可</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">aiplatform.reasoningEngines.update</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">aiplatform.persistentResources.get/list</span></font></font></td></tr><tr style="box-sizing: border-box;"><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">影响</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">LLM 回忆录、聊天记录、GCS 访问</span></font></font></td><td style="box-sizing: border-box;padding: 2px 8px;border: 1px solid rgba(0, 0, 0, 0);word-break: break-word;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><font dir="auto" style="box-sizing: border-box;vertical-align: inherit;"><span leaf="">Ray 集群根；BigQuery/GCS 读/写</span></font></font></td></tr></tbody></table>  
研究人员发现，拥有 aiplatform.reasoningEngines.Update 权限的攻击者可以将恶意 Python 代码注入到推理引擎中的工具调用中。  
  
该攻击的原理是使用包含恶意代码的工具（例如嵌入在标准函数中的反向 shell）来更新现有的推理引擎。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/pcgSUGCDdKIuSIuvQuLewnjHPuBWf4ZOBRzqlEOp6Aia1OBXGCCbnISew42ZtyLzjjTujwnlMuicHOmpyc0nBXZg/640?wx_fmt=png&from=appmsg "")  
  
触发后，该代码会在推理引擎的计算实例上执行，攻击者可以通过实例元数据服务提取“推理引擎服务代理”的凭据。  
  
默认情况下，此服务代理拥有广泛的权限，包括访问 Vertex AI 内存、聊天会话、存储桶和日志记录功能。  
  
攻击者可以读取所有聊天记录，访问 LLM 内存，并从存储资源中检索敏感信息。  
  
关键在于，这种攻击只需要极少的权限，因为任何账户的公共存储桶都可以用作暂存位置。  
## Vertex AI上的Ray  
  
第二个漏洞会影响 Vertex AI 集群上的 Ray，其中“自定义代码服务代理”会自动附加到集群头节点。  
  
XM Cyber 的研究人员发现，只有 aiplatform.persistentResources.list 和 aiplatform.persistentResources 的用户。  
  
拥有标准“Vertex AI 查看器”角色权限的用户可以通过 GCP 控制台获得对头节点的根访问权限。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/pcgSUGCDdKIuSIuvQuLewnjHPuBWf4ZOLo0LcrMwiae5dOA44ibTcqdORcHxqLwwmna9uicT0U6jcK5w1K5o6qbIA/640?wx_fmt=png&from=appmsg "")  
  
即使拥有只读查看权限，攻击者也可以点击控制台中的“Head node interactive shell”链接来获取 root shell。  
  
然后，他们查询元数据服务以检索自定义代码服务代理访问令牌。  
  
虽然该令牌的 IAM 操作范围有限，但它赋予了对存储桶、BigQuery 资源、发布/订阅以及整个云平台的只读访问权限的完全控制权。  
  
使用 Vertex AI 的组织应使用自定义角色撤销不必要的服务代理权限，关闭头节点 shell，在更新前验证工具代码，并通过安全指挥中心监控元数据服务访问。  
  
