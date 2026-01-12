#  安全热点周报：D-Link 老旧 DSL 路由器曝新漏洞，正遭黑客活跃攻击  
 奇安信 CERT   2026-01-12 09:00  
  
<table><tbody><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;border-bottom: 4px solid rgb(68, 117, 241);visibility: visible;"><th align="center" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 0px none;background: rgb(254, 254, 254);max-width: 100%;box-sizing: border-box !important;font-size: 20px;line-height: 1.2;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(68, 117, 241);visibility: visible;"><strong style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;"><span style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;font-size: 17px;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">安全资讯导视 </span></span></strong></span></th></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td valign="middle" align="center" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 0px none;max-width: 100%;box-sizing: border-box !important;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;clear: both;min-height: 1em;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">• </span><span leaf="">工信部等八部门联合印发《“人工智能+制造”专项行动实施意见》</span></p></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td valign="middle" align="center" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 0px none;max-width: 100%;box-sizing: border-box !important;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;clear: both;min-height: 1em;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">• </span><span leaf="">美军利用网络战和情报等能力支持抓捕马杜罗的军事行动</span></p></td></tr><tr style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;border-bottom: 1px solid rgb(180, 184, 175);visibility: visible;"><td valign="middle" align="center" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 0px none;max-width: 100%;box-sizing: border-box !important;font-size: 14px;visibility: visible;"><p style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;clear: both;min-height: 1em;visibility: visible;"><span leaf="" style="-webkit-tap-highlight-color: transparent;margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;visibility: visible;">• </span><span leaf="">罗马尼亚火电龙头企业遭勒索软件攻击，IT基础设施全部瘫痪</span></p></td></tr></tbody></table>  
  
**PART****0****1**  
  
  
**漏洞情报**  
  
  
**1.SmarterMail邮件服务器任意文件上传漏洞安全风险通告**  
  
  
1月9日，奇安信CERT监测到官方修复SmarterMail邮件服务器任意文件上传漏洞(CVE-2025-52691)，该漏洞允许未经身份验证的攻击者上传任意文件到邮件服务器的任何位置，从而利用漏洞执行远程代码，控制邮件服务器，窃取或破坏数据。奇安信鹰图资产测绘平台数据显示，该漏洞关联的全球风险资产总数为40545个，关联IP总数为19685个。目前该漏洞PoC和技术细节已公开。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**2.n8n远程代码执行漏洞安全风险通告**  
  
  
1月8日，奇安信CERT监测到官方修复n8n远程代码执行漏洞(CVE-2026-21858)，该漏洞源于n8n表单（Form）节点在处理请求时，未能正确验证Content-Type头部，导致攻击者可通过类型混淆覆盖req.body.files对象从而操纵文件路径参数，未经身份认证的用户可通过公开的表单节点读取任意文件并配合后台漏洞实现远程代码执行。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为26437个，关联IP总数为8889个。目前该漏洞PoC已公开。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**3.ComfyUI-Manager远程代码执行漏洞安全风险通告**  
  
  
1月7日，奇安信CERT监测到官方修复ComfyUI-Manager远程代码执行漏洞(CVE-2025-67303)，该漏洞源于ComfyUI-Manager的数据与配置目录在旧版本中未受ComfyUI的Web API访问控制充分保护，攻击者可通过发送特殊请求覆盖config.ini配置文件并利用其他危险接口执行恶意脚本代码，从而获取服务器权限。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为28968个，关联IP总数为3719个。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**4.n8n Pyodide命令执行漏洞安全风险通告**  
  
  
1月5日，奇安信CERT监测到官方修复n8n Pyodide命令执行漏洞(CVE-2025-68668)，在n8n的Pyodide Python代码节点中存在沙箱绕过漏洞，允许经过身份验证的用户通过创建或修改工作流来执行任意系统命令，从而获取服务器权限。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为26289个，关联IP总数为8916个。目前该漏洞PoC已公开。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**5.RustFS gRPC身份认证绕过漏洞安全风险通告**  
  
  
2025年12月31日，奇安信CERT监测到官方修复RustFS gRPC身份认证绕过漏洞(CVE-2025-68926)，该漏洞源于RustFS的gRPC认证使用了硬编码的静态令牌 'rustfs rpc'，该令牌在源代码库中公开，客户端和服务器端均为硬编码，不可配置且没有令牌轮换机制，对所有RustFS部署均有效。攻击者可以利用此公开的静态令牌进行身份验证，并执行包括数据销毁、策略操纵和集群配置更改在内的特权操作。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为18378个，关联IP总数为2442个。目前该漏洞PoC和技术细节已在互联网上公开，鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**6.Zimbra本地文件包含漏洞安全风险通告**  
  
  
2025年12月29日，奇安信CERT监测到官方修复Zimbra本地文件包含漏洞(CVE-2025-68645)，该漏洞源于RestFilter servlet对请求参数过滤不严格，未经身份验证的远程攻击者可以构造恶意路径参数，获取服务器WebRoot目录下的敏感文件。奇安信鹰图资产测绘平台数据显示，该漏洞关联的国内风险资产总数为2927个，关联IP总数为622个。鉴于该漏洞影响范围较大，建议客户尽快做好自查及防护。  
  
  
**PART****0****2**  
  
  
**新增在野利用**  
  
  
**1.D-Link DSL 网关设备命令注入漏洞(CVE-2026-0625)******  
  
  
1月7日，攻击者正在利用最近发现的命令注入漏洞，该漏洞会影响多款多年前已停止支持的 D-Link DSL 网关路由器。  
  
该漏洞现已被追踪为 CVE-2026-0625，由于 CGI 库中输入清理不当，导致 dnscfg.cgi 端点受到影响。未经身份验证的攻击者可以利用此漏洞，通过 DNS 配置参数执行远程命令。漏洞情报公司 VulnCheck 于12月15日向 D-Link 报告了该问题，此前 Shadowserver 发现其一个蜜罐遭到命令注入攻击。  
  
D-Link 确认以下设备型号和固件版本受 CVE-2026-0625 影响：DSL-526B ≤ 2.01、DSL-2640B ≤ 1.07、DSL-2740R < 1.17、DSL-2780B ≤ 1.01.14。上述产品自2020年起已停止销售，不会再收到固件更新以修复 CVE-2026-0625 漏洞。因此，厂商强烈建议用户停用受影响的设备，并用受支持的型号进行替换。  
  
目前的分析表明，除了直接检查固件之外，没有其他可靠的型号检测方法。因此，作为调查的一部分，D-Link 正在验证旧平台和受支持平台上的固件版本。尚不清楚是谁在利用该漏洞，以及攻击目标是什么。不过，VulnCheck 指出，大多数消费级路由器的配置仅允许通过局域网访问管理型通用网关接口 (CGI) 端点，例如 dnscfg.cgi。利用 CVE-2026-0625 意味着基于浏览器的攻击或配置为远程管理的目标设备。  
  
D-Link 警告用户，停产设备将不再接收固件更新、安全补丁或任何维护。使用已停产（EoL）路由器和网络设备的用户应将其更换为供应商仍在积极支持的型号，或者将其部署在非关键网络中（最好是分段部署），并使用最新的可用固件版本和严格的安全设置。  
  
  
参考链接：  
  
https://www.bleepingcomputer.com/news/security/new-d-link-flaw-in-legacy-dsl-routers-actively-exploited-in-attacks/  
  
**PART****0****3**  
  
  
**安全事件**  
  
  
**1.美军利用网络战和情报等能力支持抓捕马杜罗的军事行动**  
  
  
1月4日奇安网情局消息，多家美媒报道称，美国军方和情报机构动用网络、太空和情报等能力支持美军突袭委内瑞拉并抓捕该国总统尼古拉斯·马杜罗的“绝对决心行动”。美国参谋长联席会议主席丹·凯恩表示，此次极其精确的军事行动涉及西半球150多架飞机密切协调起飞，将一支美军特战部队运抵委内瑞拉首都加拉加斯市中心，同时保持战术上的出其不意。当美军直升机接近海岸时，美国网络司令部、太空司令部和其他机构在空中部署了多层掩护，以保护飞机并保持突袭效果。美国中央情报局、国家安全局和国家地理空间情报局均参与了此次任务，获取了尼古拉斯·马杜罗日常生活的详细情报，包括行踪、居住地、旅行地、饮食和穿着，甚至宠物等细节。美国总统特朗普发言暗示，美国可能利用网络攻击或其他技术手段切断了加拉加斯市的电力供应。互联网网络追踪组织NetBlocks报告称，1月3日清晨加拉加斯停电期间，该地区互联网连接中断。如果情况属实，这将是近年来美国公开使用网络力量攻击他国的一次最引人注目的行动之一。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/uhvpfcmGMn-UmYVameVGnw  
  
  
**2.昆明某企业核心业务系统遭入侵，公民个人信息泄露**  
  
  
2025年12月31日公安部网安局消息，云南公安机关网安部门近日协同作战，成功斩断一条利用黑客技术窃取公民个人信息的黑色产业链，抓获犯罪嫌疑人2名，现场查获涉案设备多台，涉案金额数十万元，有力震慑了网络违法犯罪活动。2025年10月，昆明市公安局西山分局网安大队接到辖区某企业报案，称其核心业务系统遭遇非法入侵。接报后，云南省公安厅网安总队迅速启动三级联动机制，组建工作专班，对入侵痕迹展开全面溯源调查工作，最终锁定在重庆市活动的李某平、杨某福二人具有重大作案嫌疑，并实施抓捕行动。经查，李某平、杨某福二人利用计算机技术非法入侵多家公司信息系统窃取数据，贩卖给境外犯罪组织开展违法犯罪活动，非法获利数十万元。目前，两人已被依法刑事拘留。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/RccgIUivEb_HGlDo3_2yCw  
  
  
**3.罗马尼亚火电龙头企业遭勒索软件攻击，IT基础设施全部瘫痪**  
  
  
2025年12月29日Bleeping Computer消息，罗马尼亚最大的燃煤能源供应商奥尔特尼亚能源综合体（Complexul Energetic Oltenia）在圣诞节次日遭到勒索软件攻击，内部IT基础设施陷入瘫痪。该公司表示：“受此次攻击影响，部分文档和文件被加密，多款计算机应用程序暂时无法使用，包括ERP系统、文档管理应用、内部电子邮件服务及网站等。”公司还表示：“业务活动受到了一定影响，但没有危及到国家能源系统的运行。”该事件已上报给罗马尼亚国家网络安全局、能源部及其他相关主管部门，公司还向该国有组织犯罪和恐怖主义调查局（DIICOT）提交了刑事投诉。该公司运营了4座发电厂，提供约占全国30%的电力。  
  
  
原文链接：  
  
https://www.bleepingcomputer.com/news/security/romanian-energy-provider-hit-by-gentlemen-ransomware-attack/  
  
  
**4.因未修改默认密码，美国加州多地交通信号灯被黑**  
  
  
2025年12月29日PA Daily Post消息，美国加州帕洛阿尔托、门洛帕克和红木城的行人过街信号灯在今年4月遭到入侵，被用于播放仿冒马斯克、扎克伯格等名人不当言论的AI音频。根据记者近期通过《加州公共记录法》获得的电子邮件显示，原来是加州交通局没有更改信号灯的默认密码，使黑客得以将原有音频文件替换为仿冒扎克伯格和马斯克的声音。恶作剧被发现后，加州交通局一度禁用了过街设备的音频功能，不过后来已经恢复。制造商当时联系了加州交通局和门洛帕克，建议他们为行人过街信号灯设置强密码。加州交通局发言人Jeneane Crawford表示，他们已发现其他同样需要更改密码的路口，并已完成密码更新，以防止未来再次发生类似事件。  
  
  
原文链接：  
  
https://padailypost.com/2025/12/29/crosswalk-signals-were-hacked-because-of-a-weak-password/  
  
  
**PART****0****4**  
  
  
**政策法规**  
  
  
**1.教育部发布《教育数据分类分级指南》教育行业标准**  
  
  
1月9日，教育部发布《教育数据分类分级指南》教育行业标准。该文件给出了教育数据分类分级的规则、方法和流程等，适用于各级教育行政部门及其直属单位、各级各类学校开展数据分类分级工作，并为有关主管监管部门、第三方评估机构开展数据安全检查与评估工作提供参考。该文件不适用于涉及国家秘密的数据。各级教育行政部门及其直属单位、各级各类学校涉及的卫生健康、科技等行业领域数据分类分级，遵照相关行业领域标准规范执行。  
  
  
原文链接：  
  
http://www.moe.gov.cn/srcsite/A16/s3342/202601/t20260107_1425902.html  
  
  
**2.工信部等八部门联合印发《“人工智能+制造”专项行动实施意见》**  
  
  
1月8日，工业和信息化部、中央网信办、国家发展改革委、教育部、商务部、国务院国资委、市场监管总局、国家数据局等八部门联合印发《“人工智能+制造”专项行动实施意见》。该文件单章要求以安全护航制造业，提升安全保障能力，建立安全治理机制。包括攻关深度合成鉴伪、工业模型算法安全防护、训练数据保护、对抗样本检测、智能终端安全测评等关键技术，加强数据安全管理，强化人工智能安全保护能力。构建安全风险库、语料库等资源，建设工业安全大模型。通过知识库优化、训练语料纠错、生成合成内容标识等，增强人工智能透明度、可解释性，降低幻觉风险。研究制定工业和信息化领域人工智能分类分级、评估评测、应急处置等安全政策标准，建立人工智能安全风险监测预警技术能力，制定工业和信息化领域人工智能安全风险信息报送与共享工作指引。  
  
  
原文链接：  
  
https://www.miit.gov.cn/cms_files/demo/pdfjs/web/viewer.html?file=/cms_files/filemanager/1226211233/attach/20261/8df573ca9a964b71a947d2b54edced2f.pdf  
  
  
**3.工信部印发《工业互联网和人工智能融合赋能行动方案》》**  
  
  
1月6日，工业和信息化部印发《工业互联网和人工智能融合赋能行动方案》。该文件包括总体要求、基础底座升级行动、数据模型互通行动、应用模式焕新行动、产业生态融通行动、保障措施六部分。该文件要求，加强数据标注、训练、使用、安全等方面关键技术攻关，加快推动标注算法优化、标注工具与标注模型研发。统筹工业数据分类分级安全管理，完善数据质量、数据安全风险等评估体系，将安全管理贯穿工业数据集建设全过程。  
  
  
原文链接：  
  
https://www.miit.gov.cn/cms_files/demo/pdfjs/web/viewer.html?file=/cms_files/filemanager/1226211233/attach/20261/6b2e7f276c5f4093a024c3b4104f8b2d.pdf  
  
  
**4.交通运输部印发《关于加快交通运输公共数据资源开发利用的实施意见》**  
  
  
1月4日，交通运输部印发《关于加快交通运输公共数据资源开发利用的实施意见》。该文件包括总体要求、建立交通运输高质量数据资源体系、强化交通运输公共数据资源供给、促进交通运输公共数据应用创新、强化数据安全保障、加强政策支持六部分。在强化数据安全保障方面，该文件提出完善数据安全管理规范体系，提升数据安全保障能力。研究制定交通运输数据安全管理制度，建立健全公共数据资源开发利用安全风险识别评估、监测预警、应急处置等数据安全规范体系，明确数据资源生产、采集存储、加工使用、产品开发及经营等过程中相关主体安全责任。加强公共数据可信采集、加密传输、可靠存储、受控交换共享、实时分析、智能应用、销毁确认及存证溯源等环节的安全技术支撑，依法开展数据安全风险评估，提升数据汇聚关联风险识别和管控水平，确保公共数据资源开发利用全流程安全可控。  
  
  
原文链接：  
  
https://xxgk.mot.gov.cn/2020/jigou/kjs/202512/t20251231_4194936.html  
  
  
**5.《网络安全标准实践指南——工业企业数据安全能力成熟度模型》公开征求意见**  
  
  
1月4日，全国网络安全标准化技术委员会秘书处组织编制了《网络安全标准实践指南——工业企业数据安全能力成熟度模型（征求意见稿）》，现面向社会公开征求意见。该文件提出了工业企业数据安全能力成熟度模型， 给出了工业企业数据安全能力成熟度等级评估方法，适用于指导工业企业开展数据安全能力建设，以及对工业企业数据安全能力成熟度等级进行评估。  
  
  
原文链接：  
  
https://www.tc260.org.cn/sysFile/downloadFile/733651ddccd74699a56e07d2e8fa7844  
  
  
**6.工信部等四部门联合印发《汽车行业数字化转型实施方案》**  
  
  
2025年12月30日，工业和信息化部、教育部、市场监管总局、国家数据局等四部门近日联合印发《汽车行业数字化转型实施方案》。该文件要求，完善数据安全保护体系与技术能力。具体包括：加快健全汽车行业数据安全管理制度和标准规范，基本实现规模以上汽车工业企业重要数据识别和目录备案、数据分级保护、风险评估全覆盖。建立高效便利安全的汽车行业数据跨境流动机制，指导企业强化重要数据出境保护，建设出境安全监测、日志审计、应急处置、检查支持等技术能力。深化应用隐私保护计算、区块链等技术，引导构建安全可信的汽车数据开发利用环境。  
  
  
原文链接：  
  
https://www.miit.gov.cn/jgsj/zbys/wjfb/art/2025/art_36a3bdae02c44211865626b94112ed5f.html  
  
  
**7.工信部印发《关于加快推进国家新型互联网交换中心创新发展的指导意见》**  
  
  
2025年12月30日，工业和信息化部印发《关于加快推进国家新型互联网交换中心创新发展的指导意见》。该文件要求提升新型互联网交换中心安全防护能力。具体包括：完善制度管理体系，认真履行国家和行业网络与数据安全监管要求，建立健全网络安全、数据安全、信息安全管理制度，参照行业关键信息基础设施防护标准，定期开展安全评测和风险排查，做好通信网络和数据安全防护工作。加强技术手段建设，提高网络安全、数据安全、信息安全风险监测和事件处置能力，实现重大安全威胁、风险和事件的实时感知与及时上报，保障服务安全可控。  
  
  
原文链接：  
  
https://www.miit.gov.cn/zwgk/zcwj/wjfb/yj/art/2025/art_cf88a2c20571429699eaad98a602090a.html  
  
  
**8.国家网络身份认证公共服务6项公共安全行业标准发布**  
  
  
2025年12月28日，为支撑《国家网络身份认证公共服务管理办法》的落地与实施，公安部近日发布了2025年第2号《中华人民共和国公安部公共安全行业标准公告》，国家网络身份认证公共服务标准体系中6项公共安全行业标准正式发布，于2026年5月1日实施。具体包括《国家网络身份认证公共服务 通用术语》、《国家网络身份认证公共服务 认证服务 第1部分：认证因子》、《国家网络身份认证公共服务 认证服务 第2部分：真实身份认证服务接口要求》、《国家网络身份认证公共服务 人脸活体图像采集控件技术要求》、《国家网络身份认证公共服务 安全接入设备技术规范》、《国家网络身份认证公共服务 个人身份信息处理要求》。  
  
  
原文链接：  
  
https://mp.weixin.qq.com/s/2CRsPNchgPPCW24eysISTg  
  
  
**9.国家网信办《人工智能拟人化互动服务管理暂行办法》公开征求意见**  
  
  
2025年12月27日，国家互联网信息办公室起草了《人工智能拟人化互动服务管理暂行办法（征求意见稿）》，现向社会公开征求意见。该文件要求，作为人工智能拟人化互动服务提供者，应当落实拟人化互动服务安全主体责任，建立健全算法机制机理审核、科技伦理审查、信息发布审核、网络安全、数据安全、个人信息保护、反电信网络诈骗、重大风险预案、应急处置等管理制度，具有安全可控的技术保障措施，配备与产品规模、业务方向和用户群体相适应的内容管理技术和人员。应当在拟人化互动服务全生命周期履行安全责任，明确设计、运行、升级、终止服务等各阶段安全要求，保证安全措施与服务功能同步设计、同步使用，提升内生安全水平，加强运行阶段安全监测和风险评估，及时发现纠正系统偏差、处置安全问题，依法留存网络日志。  
  
  
原文链接：  
  
https://www.cac.gov.cn/2025-12/27/c_1768571207311996.htm  
  
  
**10.国家数据局印发《关于加强数据科技创新的实施意见》**  
  
  
2025年12月27日，国家数据局印发《关于加强数据科技创新的实施意见》。该文件分为总体要求、加强技术攻关与高水平应用、培育数据科技创新产业生态、夯实数据科技创新基础支撑四个部分。该文件要求，加快攻关数据供给、流通、利用、安全等关键技术；推动建设数据安全防护平台，促进跨地域、跨领域、跨主体数据资源可信流通与高效利用，保障数据安全；制定和完善数据技术、数据安全等相关标准，提升数据领域安全风险管理水平。  
  
  
原文链接：  
  
https://www.nda.gov.cn/sjj/zwgk/zcfb/1227/20251227100433509018891_pc.html  
  
  
**11.国家金监总局发布《银行业保险业数字金融高质量发展实施方案》**  
  
  
2025年12月26日，国家金融监管总局发布《银行业保险业数字金融高质量发展实施方案》。该文件分为总体要求、工作任务、组织保障和监督管理3个部分，从数字金融治理、数字金融服务、数字技术应用、数据要素开发、风险管理和监管数字化智能化转型等方面提出了33项工作任务。该文件对安全做了大量要求，共体及32次，包括强化数字化形势下的风险管理、着力建设智能风控体系、切实提升人工智能技术的安全应用能力、有效管理算法模型风险、构建安全可信的数据生态、加强数据安全保护、提高网络安全韧性、防范数字生态外部合作风险等。  
  
  
原文链接：  
  
https://www.nfra.gov.cn/cn/view/pages/governmentDetail.html?docId=1239741&itemId=861&generaltype=1  
  
  
  
  
**往期精彩推荐**  
  
  
[【已复现】ComfyUI-Manager 远程代码执行漏洞(QVD-2026-2759)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247504466&idx=1&sn=a89679af25b388b62faa3d9fa65fcd52&scene=21#wechat_redirect)  
  
  
[【已复现】SmarterMail 邮件服务器任意文件上传漏洞(CVE-2025-52691)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247504458&idx=1&sn=df0dba80f623b350fad0bb89ff0d1e65&scene=21#wechat_redirect)  
  
  
[【已复现】n8n 远程代码执行漏洞(CVE-2026-21858)安全风险通告](https://mp.weixin.qq.com/s?__biz=MzU5NDgxODU1MQ==&mid=2247504447&idx=1&sn=b867a6a515f1ea35f79031f34ae71aac&scene=21#wechat_redirect)  
  
  
  
  
本期周报内容由安全内参&虎符智库&奇安信CERT联合出品！  
  
  
  
  
  
  
  
