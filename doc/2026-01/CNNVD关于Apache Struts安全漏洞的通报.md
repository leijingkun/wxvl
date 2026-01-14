#  CNNVD关于Apache Struts安全漏洞的通报  
原创 CNNVD  CNNVD安全动态   2026-01-14 06:55  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/g1thw9GoocfpeKv1eicF4icEx1vUX4LQ1JjlMnGl5z2XiaAQGZdFulYs0vsE3icB8RUiawPqDSb5lvm8G0drb7iaw7sQ/640?wx_fmt=gif&from=appmsg "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/g1thw9GoocfpeKv1eicF4icEx1vUX4LQ1Js3VkKswpUtkoDWibZ1YQl1lIdcctfqePCcSPEdc38SnhJGdqGJUFx9w/640?wx_fmt=gif&from=appmsg "")  
  
**点击蓝字 关注我们**  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/g1thw9GoocfpeKv1eicF4icEx1vUX4LQ1Js3VkKswpUtkoDWibZ1YQl1lIdcctfqePCcSPEdc38SnhJGdqGJUFx9w/640?wx_fmt=gif&from=appmsg "")  
  
  
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
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/g1thw9GoocfpeKv1eicF4icEx1vUX4LQ1JMd8aMOqNkic25xydKvYcCVEsHXvm506icfXiaFep4AfohjraUj3F2jMfg/640?wx_fmt=gif&from=appmsg "")  
  
  
  
