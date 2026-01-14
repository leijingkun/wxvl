#  CNNVD | 关于Apache Struts安全漏洞的通报  
 中国信息安全   2026-01-14 10:35  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1brjUjbpg5yxejW8btwibLnqQYzZ7jrnGv178xDoGqAicWPyyBubXwYII6j7Pwo44d0gxbXib265bYZGFWHFXPhGQ/640?wx_fmt=gif&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/1brjUjbpg5yxejW8btwibLnqQYzZ7jrnGHs5OYuU8fTQHD0uxySq2ib7DovBwQvN0PjKuUkLCYmnVlsAkicCKu5jA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/1brjUjbpg5yxejW8btwibLnqQYzZ7jrnGv178xDoGqAicWPyyBubXwYII6j7Pwo44d0gxbXib265bYZGFWHFXPhGQ/640?wx_fmt=gif&from=appmsg "")  
  
**扫码订阅《中国信息安全》**  
  
  
邮发代号 2-786  
  
征订热线：010-82341063  
  
  
  
**漏洞情况**  
  
近日，国家信息安全漏洞库（CNNVD）收到关于Apache Struts安全漏洞安全漏洞（CNNVD-202601-1787、CVE-2025-68493）情况的报送。成功利用漏洞的攻击者，可获取目标服务器敏感信息或导致服务器拒绝服务。Apache Struts多个版本均受此漏洞影响。目前，Apache官方已发布新版本修复了该漏洞，建议用户及时确认产品版本，尽快采取修补措施。  
  
## 一漏洞介绍  
  
  
Apache Struts是美国阿帕奇（Apache）基金会的一个开源项目，是一套用于创建企业级Java Web应用的开源MVC框架，主要提供两个版本框架产品，Struts 1和Struts 2。  
  
Apache Struts的XWork组件在解析XML配置文件时未对输入进行充分的安全校验，攻击者可通过提交恶意XML文档，诱使服务器在解析过程中加载文件触发漏洞。成功利用漏洞的攻击者，可获取目标服务器敏感信息或导致服务器拒绝服务。  
  
## 二危害影响  
  
  
Apache Struts 2.0.0版本至2.3.37版本、2.5.0版本至2.5.33版本、6.0.0 版本至6.1.0版本在安全漏洞。  
  
## 三修复建议  
  
  
目前，Apache官方已发布新版本修复了该漏洞，建议用户及时确认产品版本，尽快采取修补措施。官方补丁链接：   
  
https://struts.apache.org/download.cgi  
  
本通报由CNNVD技术支撑单位——奇安信网神信息技术（北京）股份有限公司、南京众智维信息科技有限公司、中孚安全技术有限公司、塞讯信息技术（上海）有限公司、黑龙江安信与诚科技开发有限公司、南京禾盾信息科技有限公司、北京山石网科信息技术有限公司、任子行网络技术股份有限公司、中国银联股份有限公司等技术支撑单位提供支持。  
  
CNNVD将继续跟踪上述漏洞的相关情况，及时发布相关信息。如有需要，可与CNNVD联系。联系方式: cnnvd@itsec.gov.cn  
  
  
（来源：CNNVD）  
  
  
  
**分享网络安全知识 强化网络安全意识**  
  
**欢迎关注《中国信息安全》杂志官方抖音号**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/1brjUjbpg5yxejW8btwibLnqQYzZ7jrnGSk8Z0V23tVibGY6UUDzYzhAIvBicwTqBhrwqicRlgPN80LD56v7PdtWXg/640?wx_fmt=jpeg&from=appmsg "")  
  
  
**《中国信息安全》杂志倾力推荐**  
  
**“企业成长计划”**  
  
  
**点击下图 了解详情**  
  
  
  
[](https://mp.weixin.qq.com/s?__biz=MzA5MzE5MDAzOA==&mid=2664162643&idx=1&sn=fcc4f3a6047a0c2f4e4cc0181243ee18&scene=21#wechat_redirect)  
  
  
