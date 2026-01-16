#  Nacos系列漏洞：风险点与综合利用工具详解  
原创 m3x1
                    m3x1  梦醒安全   2026-01-16 00:02  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Aj9tsa4DmOmra6EI659XoNIvNn9wnMpVqicmqFmpJAwWJA0fw90SqBVOCEicpkLlV63QbNibrCx4BYT4JKia33l1Yg/640?wx_fmt=png&from=appmsg "")  
  
免责声明：本公众号内容仅用于知识分享和学习，由于传播、利用本公众号所提供的信息而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号梦醒安全及作者不为此承担任何责任，一旦造成后果请自行承担！  
  
PART.01  
  
前言  
  
Nacos作为一款广泛应用的动态服务发现、配置管理和服务管理平台，其安全稳定性直接关系到整个微服务架构的安全。然而，Nacos存在多个因默认配置不当、代码逻辑缺陷引发的安全漏洞，这些漏洞被恶意利用后，可能导致服务器被入侵、数据泄露等严重后果。本文将详细梳理Nacos常见高危漏洞，并介绍一款针对性的综合利用工具，帮助安全从业者快速检测与验证相关风险。  
  
PART.02  
  
介绍  
## 一、Nacos核心高危漏洞清单  
  
经过安全研究与验证，以下是Nacos现阶段需重点关注的高危漏洞，涵盖默认配置漏洞、注入漏洞、反序列化漏洞等多个类型：  
<table><thead><tr><th><section><span leaf="">漏洞类型</span></section></th><th><section><span leaf="">PoC验证</span></section></th><th><section><span leaf="">Exploit利用</span></section></th><th><section><span leaf="">漏洞编号</span></section></th></tr></thead><tbody><tr><td><section><span leaf="">Nacos 默认关闭认证</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">/</span></section></td></tr><tr><td><section><span leaf="">Nacos 默认密码 (nacos/nacos)</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2021-896025</span></section></td></tr><tr><td><section><span leaf="">Nacos 默认 server.identity</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">/</span></section></td></tr><tr><td><section><span leaf="">Nacos 默认 token.secret.key</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2023-1655789</span></section></td></tr><tr><td><section><span leaf="">Nacos 默认 User-Agent</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2021-29441</span></section></td></tr><tr><td><section><span leaf="">Nacos Derby SQL Injection</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2021-897468</span></section></td></tr><tr><td><section><span leaf="">Nacos JRaft Hessian 反序列化</span></section></td><td><section><span leaf="">/</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2023-1700159</span></section></td></tr><tr><td><section><span leaf="">Nacos JRaft Services 文件操作漏洞</span></section></td><td><section><span leaf="">/</span></section></td><td><section><span leaf="">✅</span></section></td><td><section><span leaf="">AVD-2024-1743586</span></section></td></tr></tbody></table>  
上述漏洞中，默认配置类漏洞（如默认关闭认证、默认密码）是最易被利用的类型——多数运维人员在部署Nacos时未修改默认配置，直接将服务暴露在公网，攻击者可轻松登录后台接管服务；而Derby SQL注入、JRaft反序列化等漏洞，则可通过构造恶意请求，实现数据库操控、远程代码执行等高危操作。  
## 二、漏洞检测与利用实操展示  
  
针对上述漏洞，安全团队开发了Nacos综合漏洞利用工具，可实现漏洞一键检测、认证绕过、漏洞利用等核心功能，以下是工具核心功能的实操效果：  
### 1. 漏洞检测  
  
工具可批量检测目标Nacos服务是否存在上述高危漏洞，快速输出漏洞清单与风险等级，帮助安全人员定位薄弱环节：  
  
![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOlz8HV1I0qK9pZt69qvHmIiaCicIbVkLN2gcr12OHDKPnIlwovtaXjWeFMxvr3aA9YOlLOKH9AoiaMjQ/640?wx_fmt=jpeg&from=appmsg "")  
### 2. 认证绕过  
  
针对Nacos认证机制缺陷，工具可实现无权限情况下绕过认证，直接访问Nacos核心接口，验证认证漏洞的实际危害：  
  
![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOlz8HV1I0qK9pZt69qvHmIiabkC1ECHPucaiao7MVpIX3g3atHU0KeuvcCD9N8kT3z1icLcv9g7cEthg/640?wx_fmt=jpeg&from=appmsg "")  
### 3. Derby SQL 注入  
  
构造恶意SQL语句，通过工具可直接利用Derby SQL注入漏洞，读取、篡改Nacos数据库中的核心配置数据：  
  
![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOlz8HV1I0qK9pZt69qvHmIiaU9nNzGeBUz9GNFBEQVS67q9DCzRdf7o24XTTI6ptjmLqic6w7z0mia1w/640?wx_fmt=jpeg&from=appmsg "")  
### 4. JRaft 反序列化漏洞  
  
利用JRaft组件的Hessian反序列化缺陷，工具可触发远程代码执行，实现对服务器的完全控制：  
  
![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOlz8HV1I0qK9pZt69qvHmIiaHhZN3SaxgMfmMwOPiaWWeO1dGibgWcvXholQjSuVtOFCKibPBYIN4PdhA/640?wx_fmt=jpeg&from=appmsg "")  
### 5. 批量任务处理  
  
针对多目标检测场景，工具支持批量添加检测任务，自动完成漏洞扫描与利用，提升安全测试效率：  
  
![Image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOlz8HV1I0qK9pZt69qvHmIiaNUaQDMr21MibpKh2m3HMskzafVRYdvicSrUNickIkYv0NBKbDBHoQFROg/640?wx_fmt=jpeg&from=appmsg "")  
## 三、安全防护建议  
  
针对Nacos系列漏洞，结合实际运维场景，给出以下核心防护措施：  
1. **修改默认配置**  
：立即修改Nacos默认密码（nacos/nacos），开启认证授权功能，禁用公网直接访问Nacos控制台；  
  
1. **更新版本**  
：及时升级Nacos至官方最新版本，修复已知的反序列化、SQL注入等漏洞；  
  
1. **配置安全加固**  
：修改默认的server.identity、token.secret.key等核心配置，避免因默认密钥导致的漏洞利用；  
  
1. **网络隔离**  
：将Nacos服务部署在内网环境，仅允许可信IP访问，通过防火墙、安全组限制访问范围；  
  
1. **定期检测**  
：使用专业工具定期扫描Nacos服务，及时发现并修复潜在漏洞。  
  
PART.03  
  
交流群  
  
欢迎大家加入我的交流群：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Aj9tsa4DmOl6rB8SMy4SCnsKVhiaE3iaX3dCZ85qFkQztjEiacNZAlk4Wg0vwMpwIInlIfmCgOUltCfhRrtpHB6Fw/640?wx_fmt=jpeg&from=appmsg "")  
  
如果链接过期，可以在公众号后台点击下方菜单“加群”添加我wx，备注“加群”  
  
PART.04  
  
往期推荐  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/b7iaH1LtiaKWW8vxK39q53Q3oictKW3VAXz4Qht144X0wjJcOMqPwhnh3ptlbTtxDvNMF8NJA6XbDcljZBsibalsVQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1#imgIndex=49 "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=5&tp=webp&wx_lazy=1#imgIndex=50 "")  
  
**往期好文**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/EXTCGqBpVJRSicyOOePGE9sGceAg4JcsCFHMqeE6O6zJJaSXkw6VEiaHibGnD0DzgYpbzhdbaTbsMKhJLte7sOt1g/640?wx_fmt=png&from=appmsg&wxfrom=5&tp=webp&wx_lazy=1#imgIndex=53 "")  
  
  
  
[高危预警！Apache Tika曝XXE漏洞(CVE-2025-66516)，可窃取服务器文件](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484832&idx=1&sn=df2960e41ef09a051e17164c62d7a055&scene=21#wechat_redirect)  
  
  
[Java漏洞实战平台上线！覆盖30+安全场景](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484833&idx=1&sn=f146f4facccde026a3bd980f979fd07b&scene=21#wechat_redirect)  
  
  
[【效率翻倍】14款渗透必备插件，这款Firefox定制版帮你一键集齐](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484834&idx=1&sn=6ce6214a51634deecb9ca3621d2f6096&scene=21#wechat_redirect)  
  
  
[【AI靶场练习】Gandalf靶场过关wp解析](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484882&idx=1&sn=fb9717659963fa3427a7dc5e9aa2fcd3&scene=21#wechat_redirect)  
  
  
[【AI靶场练习】Prompt Injection lab靶场wp解析](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484883&idx=1&sn=af3596bcad9aaa6938faa890bab3c2f1&scene=21#wechat_redirect)  
  
  
[深挖HTTP请求走私漏洞：原理、利用与防御](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484905&idx=1&sn=bc94966f0c1e940688f3f8eb9dfc1d4c&scene=21#wechat_redirect)  
  
  
[自开CVE-2025-55182 漏洞检测与利用工具（GUI版）使用指南](https://mp.weixin.qq.com/s?__biz=MzYzNjAwMjQ3OQ==&mid=2247484916&idx=1&sn=b1f6b5a0f8d6a3a6cd5d321d7f704b52&scene=21#wechat_redirect)  
  
  
PART.05  
  
项目获取  
  
本文介绍的Nacos综合漏洞利用工具仅用于安全测试与漏洞验证，禁止用于未授权的攻击行为！  
  
开源地址：https://github.com/h0ny/NacosExploit  
  
