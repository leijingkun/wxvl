#  漏洞通告 | Apache Struts S2-069 XXE漏洞  
原创 微步情报局  微步在线研究响应中心   2026-01-12 03:19  
  
漏洞概况  
  
Apache Struts 2 是一个用于开发 Java EE 网络应用程序的开源 MVC（模型-视图-控制器） 框架。它由 Apache 软件基金会维护，旨在简化企业级 Java 应用的开发流程。  
  
近日，微步情报局监测到Apache Struts官方发布通告，修复了Apache Struts s2-069 XXE漏洞（CVE-2025-68493）。XWork 组件在解析 XML 配置时，未对 XML 进行严格的验证和安全配置，导致存在 XML 外部实体注入 (XXE) 漏洞。攻击者可以构造恶意的 XML 实体，从而实现任意文件读取、拒绝服务 (DoS) 或服务端请求伪造 (SSRF)。  
  
鉴于历史上出现过高影响力、且利用struts漏洞的数据泄露攻击事件（如 2017 年 Equifax 数据泄露与 S2-045 相关），建议使用受影响版本的用户密切关注。  
  
漏洞处置优先级(VPT)  
  
**综合处置优先级：中**  
<table><tbody><tr style="font-size: 15px !important;"><td rowspan="3" style="font-size: 15px !important;"><b><span leaf="">基本信息</span></b></td><td style="font-size: 15px !important;"><section><span leaf="">微步编号</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">XVE-2025-52677</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">CVE编号</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">CVE-2025-68493</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">漏洞类型</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">XXE注入</span></section></td></tr><tr style="font-size: 15px !important;"><td rowspan="5" style="font-size: 15px !important;"><b><span leaf="">利用条件评估</span></b></td><td style="font-size: 15px !important;"><section><span leaf="">利用漏洞的网络条件</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">远程</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">是否需要绕过安全机制</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">不需要</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">对被攻击系统的要求</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">无</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">利用漏洞的权限要求</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">无</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">是否需要受害者配合</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">否</span></section></td></tr><tr style="font-size: 15px !important;"><td rowspan="2" style="font-size: 15px !important;"><b><span leaf="">利用情报</span></b></td><td style="font-size: 15px !important;"><section><span leaf="">POC是否公开</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">否</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">已知利用行为</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">否</span></section></td></tr></tbody></table>  
漏洞影响范围  
<table><tbody><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">产品名称</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">Apache软件基金会 | Apache Struts</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">受影响版本</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">2.0.0 &lt;= version &lt;= 2.3.37</span><span leaf=""><br/></span><span leaf="">2.5.0 &lt;= version &lt;= 2.5.33</span><span leaf=""><br/></span><span leaf="">6.0.0 &lt;= version &lt;= 6.1.0</span></section></td></tr><tr style="font-size: 15px !important;"><td style="font-size: 15px !important;"><section><span leaf="">有无修复补丁</span></section></td><td style="font-size: 15px !important;"><section><span leaf="">有</span></section></td></tr></tbody></table>  
修复方案  
  
**官方修复方案：**  
- 官方已发布修复方案，建议升级到 Apache Struts 6.1.1 或更高版本。官方通告链接：   
  
https://cwiki.apache.org/confluence/display/WW/S2-069  
  
**临时缓解措施：**  
  
如果用户无法立即升级到安全版本，  
可以  
采取  
以下两种临时缓解措施中的任意一种：  
  
  
  
方法一：自定义  
 SAXParserFactory 配置，可以通过设置 XWork 的解析工厂类来禁用外部实体：  
- 配置项：设置  
 xwork.saxParserFactory  
  
- 配置方式：将其指向一个自定义的工厂类，在该类中默认禁用外部实体  
  
方法二：通过在  
 Java 虚拟机启动参数中增加以下配置，锁定 XML 解析器的行为，防止其访问外部资源，配置方式如下所示：  
- 禁用外部  
 DTD：-Djavax.xml.accessExternalDTD=""  
  
- 禁用外部架构(Schema)：-Djavax.xml.accessExternalSchema=""  
  
- 禁用外部样式表(Stylesheet)：-Djavax.xml.accessExternalStylesheet=""  
  
- END -  
  
  
微步漏洞情报订阅服务  
  
微步提供漏洞情报订阅服务，精准、高效助力企业漏洞运营：  
- 提供高价值漏洞情报，具备及时、准确、全面和可操作性；  
  
- 可实现对高威胁漏洞提前掌握，缩短漏洞运营 MTTR；  
  
- 提供漏洞完整的技术细节，更贴近用户漏洞处置的落地；  
  
- 结合威胁事件库、APT组织数据，对漏洞风险进行持续动态更新。  
  
扫码在线沟通  
  
↓↓↓  
  
![](https://mmbiz.qpic.cn/mmbiz_png/Yv6ic9zgr5hQl5bZ5Mx6PTAQg6tGLiciarvXajTdDnQiacxmwJFZ0D3ictBOmuYyRk99bibwZV49wbap77LibGQHdQPtA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=4 "")  
  
  
[📞 点此电话咨询]()  
  
  
  
X漏洞奖励计划  
  
"X漏洞奖励计划"是微步X情报社区推出的一款针对未公开漏洞的奖励计划，我们鼓励白帽子提交挖掘到的0day漏洞，并给予白帽子可观的奖励。我们期望通过该计划与白帽子共同努力，提升0day防御能力，守护数字世界安全。  
  
活动详情：  
https://x.threatbook.com/v5/vulReward  
  
  
  
