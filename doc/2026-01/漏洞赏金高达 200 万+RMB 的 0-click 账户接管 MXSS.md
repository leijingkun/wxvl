#  漏洞赏金高达 200 万+RMB 的 0-click 账户接管 MXSS  
原创 一个不正经的黑客
                    一个不正经的黑客  一个不正经的黑客   2026-01-18 12:17  
  
   
  
![](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLenYugPj9eMWshzaHHG0sicaN9LZUicVSvuy2KPJR09lZqq4KKN4mWmvyQ/640?wx_fmt=png&from=appmsg "")  
## 前言  
  
今天跟大家分享一篇来自 Youssef Sammouda（sam0） 老哥最近在 Meta 挖出2个   
MXSS 漏洞 ，最终收获 **31.5 万美元**  
 的赏金的文章。  
  
不得不说一句，这老哥是真的猛——多年持续把 Meta 当“提款机”，而且还非常慷慨地在博客里分享了不少**看似朴实无华、实则骚到不行**  
的实战漏洞挖掘技术文章，含金量拉爆。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLecMyIPWvlcETNPP95Bd3o2mH1iag3MCicUERYgibN5JrBicMMxqtcF2NX2g/640?wx_fmt=png&from=appmsg "")  
  
接下来，一起欣赏下这场技术视觉盛宴吧！  
## 正文  
### 引言  
  
Meta Conversions API Gateway 是 Meta（前 Facebook）推出的一套**服务端事件传输方案**  
，用于帮助企业将用户行为、购买数据等网站事件，直接从服务器发送到 Meta 平台，从而绕过 Facebook Pixel 这类传统的浏览器端埋点方式。  
  
这个 Gateway 本质上充当了一个**中间层**  
：负责收集、处理并安全地把来自网站或 App 的事件数据传递给 Meta。  
  
与依赖 Cookie 和浏览器信号的客户端追踪不同，Conversions API Gateway 即使在用户关闭 Cookie、使用广告拦截器，或受到浏览器隐私策略限制的情况下，依然能够保证数据成功送达 Meta。  
  
它既可以部署在云服务器上，也可以使用 Meta 提供的托管基础设施，整体接入成本和技术门槛都相对较低。  
  
从技术角度看，Meta Conversions API Gateway 是一套**开源方案**  
。  
  
这意味着企业可以将其部署在自有基础设施中，而不是完全依赖 Meta 的服务器。  
  
Meta 以容器化应用的形式提供该 Gateway，使其能够轻松运行在主流云平台或本地环境中。  
  
通过这种方式，企业在向 Meta 发送转化事件的同时，仍然可以**完全掌控数据流向、安全策略以及合规性**  
。  
  
此外，该 Gateway 针对高并发的服务端事件传输进行了优化，能够高效处理大规模的 server-to-server 数据流量。  
### Conversions API Gateway 与 capig-events.js  
  
Conversions API Gateway（gw.conversionsapigateway.com）是 Meta 官方托管的一套 Gateway 实例。这里我们选择**直接对 Meta 自己的 Gateway 进行测试**  
，原因也很简单：  
  
一旦这里存在漏洞，影响的将是 Meta 旗下的大量核心资产。  
  
为了支持转化事件的采集与处理，该 Gateway 会对外提供一个名为 **capig-events.js**  
 的 JavaScript 脚本，路径如下：  
```
https://gw.conversionsapigateway.com/sdk/<pixel_id>/capig-events.js
```  
  
在 Meta 自家的体系中，这个脚本**并不是由开发者手动引入的**  
，而是由 Meta 的 fbq 客户端 JavaScript 模块自动加载。因此，它会在以下站点上执行：  
- • www.meta.com  
  
- • business.facebook.com  
  
- • developers.facebook.com  
  
- • 各类第三方客户网站  
  
这也意味着，一旦该脚本存在安全问题，它将**天然继承被引入站点的执行权限**  
——影响范围和危害级别都会被直接拉满。  
### Bug #1：盲目信任 event.origin，化身脚本加载器  
  
第一个漏洞完全存在于客户端，位于 **capig-events.js**  
 中，只在一个特定条件下触发：**页面存在 window.opener**  
。  
  
当 window.opener 存在时，脚本会注册一个 message 事件监听器，用于接收 IWL 的配置数据：  
```
window.addEventListener("message", function (event) {    var data = event.data;    if (        !localStorage.getItem("AHP_IWL_CONFIG_STORAGE_KEY") &&        !localStorage.getItem("FACEBOOK_IWL_CONFIG_STORAGE_KEY") &&        data.msg_type === "IWL_BOOTSTRAP"    ) {        if (checkInList(g.pixels, data.pixel_id) !== -1) {            localStorage.setItem("AHP_IWL_CONFIG_STORAGE_KEY", {                pixelID: data.pixel_id,                host: event.origin,                sessionStartTime: data.session_start_time            });            startIWL();        }    }});
```  
  
问题出在这里：**信任边界被直接跨越了**  
。  
  
当收到 msg_type 为 IWL_BOOTSTRAP 的消息时，脚本只校验了 pixel_id 是否存在于内部列表中，却**完全没有验证消息来源**  
。  
  
它没有对 event.origin 做任何 allowlist 校验，而是直接将其存入 localStorage，并当作可信的 host 使用。  
  
在后续初始化流程中，这个被信任的 origin 会被用于动态加载脚本：  
```
<origin>/sdk/<pixel_id>/iwl.js
```  
  
此时漏洞的本质就很清楚了：  
  
攻击者无法控制加载路径，但**可以完全控制 origin**  
。只要浏览器接受来自该 origin 的脚本，就能在引入该脚本的网站上下文中执行任意 JavaScript。  
  
这是一个非常典型且危险的安全反模式：  
> **将来自 postMessage 的、未经校验的 event.origin，直接用于脚本加载和代码执行。**  
  
  
简而言之：  
  
**不校验 event.origin，等于把脚本执行权直接交给攻击者。**  
#### CSP and COOP 限制  
  
从表面上看，**CSP（Content Security Policy）**  
 和 **COOP（Cross-Origin-Opener-Policy）**  
 似乎在很大程度上限制了这个漏洞的利用。  
  
在 www.meta.com 上，CSP 默认**不允许加载任意外部脚本**  
。  
  
同时，大多数页面还设置了：  
```
Cross-Origin-Opener-Policy: same-origin-allow-popups
```  
  
这使得基于 window.opener 的简单利用方式难以成立。  
  
但安全从来不是只看**单个页面或单条策略**  
，而是要看 **代码运行的所有上下文整体效果**  
。  
#### CSP Bypass  
  
在**未登录状态**  
下，一些 Meta 页面（尤其是 /help/ 路径下）会放宽 CSP，允许加载第三方分析或统计服务，例如：  
```
*.THIRD-PARTY.com*.THIRD-PARTY.net
```  
  
这一步直接扩大了攻击面。  
  
只要这些被 CSP 允许的第三方域名中**任意一个**  
存在以下问题之一：  
- • 子域接管  
  
- • XSS  
  
- • 任意文件上传  
  
攻击者就可以在该域名上托管恶意脚本，并发送所需的 postMessage，从而触发整个漏洞链。  
#### Cross-Origin-Opener-Policy 绕过  
  
在 **Facebook Android App 的 WebView**  
 环境中，利用 window.name 复用配合 window.open()，可以重新获得对 opener 的访问权限：  
```
window.name = "test";window.open(target, "test");
```  
  
这个绕过并不是让攻击者直接控制 opener，而是让**当前窗口变成自己的 opener**  
。  
  
在这种情况下，跨窗口的 postMessage 需要从**页面中的 iframe**  
 发出。  
#### Iframe Hijacking  
  
在实际环境中，meta.com 很可能会通过 iframe 加载第三方内容，例如广告或社交插件。  
  
我们正是利用了其中某个第三方组件的漏洞，**完全劫持了该 iframe**  
，并从这个被控制的 iframe 中发送恶意 postMessage。  
  
这样一来，攻击者控制的 iframe 就能够与父窗口（www.meta.com）交互，向其投递恶意 postMessage，最终触发从 **CSP 允许的第三方站点**  
 加载 iwl.js 脚本。  
#### 攻击流程总结  
  
攻击流程如下，场景发生在 **Facebook App 的 WebView 中，用户已登录 Facebook**  
：  
```
0) 用户在 Facebook 应用内打开攻击者控制的链接1) 攻击者网站上的脚本执行 opener 绕过，并将用户重定向到 https://www.meta.com/help/2) 在 help 页面中，被劫持的 iframe 跳转到 (*.THIRD-PARTY.com)（该域名被 Meta 的 CSP 允许），并向父窗口（www.meta.com）发送 IWL_BOOTSTRAP postMessage3) 同时，攻击者在 (*.THIRD-PARTY.com) 上托管 /sdk/{pixel_id}/iwl.js，其中包含 XSS payload4) www.meta.com 从该第三方站点加载并执行 iwl.js，XSS 在 meta.com 上下文中触发
```  
#### 漏洞影响  
  
该漏洞触发于：  
- • www.meta.com  
  
- • **未登录状态**  
  
- • **Facebook App WebView 环境**  
  
最终目标是将其升级为 **Facebook 账号接管（ATO）**  
。  
  
我们发现 www.facebook.com 上存在一个接口，**在 CORS 策略中信任 www.meta.com**  
。借助这一点，攻击者可以：  
- • 发起带身份凭证的跨域请求  
  
- • 从响应中读取 CSRF Token  
  
- • 执行高权限操作（如修改账号绑定的邮箱或手机号）  
  
最终实现 **完整的 Facebook 账号接管**  
。  
### Bug #2：Gateway 后端中的存储型 JavaScript 注入  
  
第二个漏洞存在于**服务端**  
，而且严重程度可以说更高。  
  
在提交完第一个漏洞后，我开始直接审计 Gateway 的源码，也正是在这个过程中，我注意到： 这个被加载的 **iwl.js**  
 脚本，实际上到底在干什么？  
  
当 iwl.js 成功从 https://gw.conversionsapigateway.com 加载后 www.meta.com 会展示一个图形化工具，大致如下：  
  
![capig-iwl-ui](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLezZnBzibNRvianZugUvnHDA8nykSPmRntCic5HuAqND0f4Fkgoiat6ZcwXQ/640?wx_fmt=png&from=appmsg "")  
  
capig-iwl-ui  
  
在使用这个工具时，我发现它允许创建 **IWL 事件规则和参数**  
。  
  
每当新增一条事件匹配规则，前端都会触发一个 **POST 请求**  
。例如，当创建一条匹配 testing_page 页面加载事件的规则时：  
- • 请求返回 200 OK  
  
- • 响应中包含新创建规则的 ID  
  
添加其他 IWL 规则时，行为完全一致。  
  
![capig-iwl-post](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLexfzrazOX6aZ2FicEamfuN601oGWm3sF99AqLianu51M5ApMrKhfKZrGQ/640?wx_fmt=png&from=appmsg "")  
  
capig-iwl-post  
  
规则添加完成后，表面上看似乎什么也没发生。这时，我决定直接查看后端源码。该项目的代码**公开托管在 Amazon ECR**  
。  
  
下载后发现里面打包了大量 JAR 依赖，于是我全部解压，并根据前面请求里的关键字段进行搜索。  
  
很快，我定位到了生成 **capig-events.js**  
 部分逻辑的位置：  
  
就在那时，我发现负责生成部分 **capig-events.js**  
 逻辑的代码位于：  
- • **Lib：pipeline-sources-common-1.0.0-SNAPSHOT.jar**  
  
- • **AHPixelIWLParametersPlugin.java**  
  
关键代码如下（简化后）：  
```
StringsKt.trimIndent(  "\n cbq.loadPlugin(\"ESTRuleEvaluator\");" +  "cbq.optIn(\"" + this.pixelID + "\", \"ESTRuleEvaluator\");\n ");StringsKt.trimIndent(  "\n cbq.config.set(\"" + this.pixelID +  "\",\"IWLParameters\",{params: " + paramsCommand + "});\n ");
```  
  
以及：  
```
if (extractorConfig != null) {    String extractorConfigSerilized = serilizeExtractorConfig(extractorConfig);    paramsCommand.append("{\"domain_uri\":\"");    paramsCommand.append(domainUrl);    paramsCommand.append("\",\"event_type\": \"");    paramsCommand.append(eventType);    paramsCommand.append("\",\"extractor_config\": ");    paramsCommand.append(extractorConfigSerilized);    paramsCommand.append(",\"extractor_type\": \"");    paramsCommand.append(extractorType);    paramsCommand.append("\",\"id\": \"");    paramsCommand.append(id);    paramsCommand.append("\",},");}
```  
  
本质上，这段代码做了一件非常危险的事情：  
> **直接使用请求中提供的 JSON 字段，拼接生成一段 JavaScript 字符串。**  
  
#### 动态脚本  
  
cbq.loadPlugin 和 cbq.config.set 这两个调用我非常熟悉——它们在 Meta Pixel 和 Signals 的前端脚本中大量存在。  
  
这让我意识到：  
  
**生成的这段代码，最终一定会被拼接进 capig-events.js。**  
  
重新查看 capig-events.js 后，这一点得到了验证。在文件末尾，确实可以看到动态注入的代码，例如：  
```
cbq.config.set("1701366827799", "IWLParameters", {    params: {        "domain_uri": "https://www.meta.com",        "event_type": "AddToCart",        "extractor_config": "...",        "extractor_type": "CSS",        "id": "id"    }});
```  
  
这也确认了一个关键事实：  
> **capig-events.js 并不是静态脚本，而是会根据后端配置动态变化。**  
  
  
回到后端代码，可以明显看出一个重大问题：它在**没有进行任何转义**  
的情况下，使用**用户可控的值**  
进行字符串拼接。  
```
if (extractorConfig != null) {    String extractorConfigSerilized = serilizeExtractorConfig(extractorConfig);    paramsCommand.append("{\"domain_uri\":\"");    paramsCommand.append(domainUrl);    paramsCommand.append("\",\"event_type\": \"");    paramsCommand.append(eventType);    paramsCommand.append("\",\"extractor_config\": ");    paramsCommand.append(extractorConfigSerilized);    paramsCommand.append(",\"extractor_type\": \"");    paramsCommand.append(extractorType);    paramsCommand.append("\",\"id\": \"");    paramsCommand.append(id);    paramsCommand.append("\",},");}
```  
  
本质上，这段代码获取请求中提供的 JSON 键，并通过追加对应的值来构造一个 JavaScript 字符串。  
  
利用方式非常简单 —— 只需注入一个单引号，就足以跳出字符串上下文并开始注入任意 JavaScript。  
  
通过注入诸如 "]}  
, 这样的字符，攻击者控制的 JavaScript 会被追加到生成的输出中。  
  
最终结果是在 capig-events.js 本身中形成一个**存储型 XSS 漏洞**  
。  
  
一旦被注入，该 payload 会被提供给每一个加载该脚本的用户 —— **包括访问 Meta 自有域名的访客**  
。  
  
![capig-iwl-poc](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLeKNRmwPKC95rn9GzHyyJ0bxOI0W3HLobW1gteDGJbBtU73kOEA2nMvw/640?wx_fmt=png&from=appmsg "")  
  
capig-iwl-poc  
#### 为什么这比典型的 XSS 更严重  
  
这不是一个单页面问题，而是一个**共享分析工具中的供应链漏洞**  
。攻击者不需要诱导用户点击任何链接，就可以触发 JavaScript 执行：  
- • 在 www.meta.com、www.facebook.com、business.facebook.com 以及 Meta 内部域名上  
  
- • 在已认证的浏览器会话中  
  
- • 以极大规模发生  
  
一旦在 Meta 的攻击面上实现代码执行，就可能进一步升级 —— 包括与 Facebook endpoints 交互、账号接管（ATO），甚至在目标为 Meta 内部域名员工的情况下实现远程代码执行。  
  
即使不立即实现 ATO，在一个全球部署的分析脚本中**静默执行攻击者控制的 JavaScript**  
本身就已经是灾难性的。  
#### 超出 Meta 的影响范围  
  
由于 Conversions API Gateway 是开源的，任何部署了该网关并从自身网关加载 capig-events.js 的公司同样可能受到影响。  
  
我们的研究表明，该应用至少已被部署 **1 亿次**  
。  
  
因此，在自身域名上托管该脚本的组织，同样暴露在这一**存储型 XSS 漏洞**  
之下。  
  
在实际情况下，这意味着在数小时内，几乎任何访问互联网的 Facebook 用户（或其他用户），只要访问了**包含该脚本的任意页面**  
，就可能被静默攻击。  
  
由于漏洞代码在受信任的 Facebook 以及第三方上下文中执行，攻击者可以在极短时间内、在无交互和无任何警告的情况下，攻陷数百万用户。  
  
![conversions-api-gateway-deployments](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLer5te9x8ZnXdu31XjEuPKCmHn2nh24cajO6IGMG0bcbGKK4icmL1tUCw/640?wx_fmt=png&from=appmsg "")  
  
conversions-api-gateway-deployments  
### 结语  
  
这两个漏洞本质上都指向同一个根因：  
  
**分析与统计类基础设施被误认为是“低风险代码”。**  
  
一旦 JavaScript 被跨产品、跨域名、跨客户复用，它就不再是边缘组件，而是直接成为平台**可信计算基（TCB）**的一部分。  
  
在这种前提下：  
- • origin 校验  
  
- • CSP 的严格设计  
  
- • 安全的代码生成与拼接方式  
  
都不再是加分项，而是**不可妥协的底线要求**  
。  
  
Conversions API Gateway 清楚地展示了这样一个现实：  
  
**局部、看似无害的信任决策，一旦被放入平台级运行环境中，就可能级联放大，最终演变为系统性的安全失效。**  
### Timeline  
- • **2024‑11‑22**  
 — 提交 Bug [#2]()  
  
  
- • **2024‑11‑22**  
 — Facebook 确认 Bug [#2]()  
  
  
- • **2024‑11‑24**  
 — 提交 Bug [#1]()  
  
  
- • **2024‑11‑25**  
 — Facebook 确认 Bug [#1]()  
  
  
- • **2024‑12‑11**  
 — 修复 Bug [#1]()  
  
  
- • **2024‑12‑24**  
 — Bug [#1]()  
 奖金 **$62,500**  
  
- • **2025‑01‑03**  
 — 修复 Bug [#2]()  
  
  
- • **2025‑01‑16**  
 — Bug [#2]()  
 奖金 **$250,000**  
  
## 漏洞点评  
  
首先，第一个漏洞的利用条件非常苛刻。以我这种水平的人来看，就算看到了event.origin 不验证的问题，我也难以挖掘出这个漏洞。  
  
因为 Meta 的 CSP 限制了 script-src 的来源，并有白名单机制。这个老哥不仅挖到了一个第三方组件的漏洞，**完全接管了指定路径的脚本**  
，而且还利用WebView 的“自己打开自己”操作，满足了事件监听的触发条件，最终将XSS成功加载到核心域中。  
  
接下来是第二个漏洞。这个漏洞的存在确实有些反直觉，问题在于：**网关会自动下发受动态配置影响的 JS 脚本**  
，而这个配置的修改似乎没有严格的权限控制，反而有一种“自动学习机制”。  
  
具体逻辑我也没完全搞清楚，但从整体来看，第二个漏洞的成因（动态JS代码生成逻辑缺少过滤）和利用其实相对简单，正如文章中的老哥提到的，**对业务的非常熟悉**  
是发现这个漏洞的关键。  
  
通过分析请求发送的 setup event 后，发现 capig-events.js 被动态注入内容。这种绕过 CSP 限制的 inline JavaScript，自然是乱杀核心域的。  
  
最后是关于这个漏洞奖金的点评。  
  
第一个漏洞可以类比为反射 XSS 绕过 CSP 导致账户接管。按照国内的 SRC评分，这大概是核心应用的中危漏洞，奖金肯定不会这么高。就算是在其他公司，花这么多精力去挖第三方组件的漏洞，奖励也肯定不会比这多。  
  
Meta 确实是出了名的“大手笔”，这也让我大开眼界。  
  
至于第二个漏洞，影响面非常广，最终给了百万奖金。说明 Meta 对这种大范围的 XSS 攻击带来的账户接管问题深有体会。**Meta 对前后端结合类型的漏洞，尤其是主要影响前端的漏洞，给出了高额奖金**  
，让我觉得这超出了我的认知。  
  
毕竟很多 Server RCE 类型的漏洞也没有这么高的奖金，Meta 就是 Meta，确实不一样。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/cxf9lzscpMqGWFOPZKanJVdicAwWUkwLefFRdSaZl8sR388XgYK8icw9ZCqsS98JGibA9ICnh8XOCl7j1TEhDicrfg/640?wx_fmt=png&from=appmsg "")  
  
  
   
  
