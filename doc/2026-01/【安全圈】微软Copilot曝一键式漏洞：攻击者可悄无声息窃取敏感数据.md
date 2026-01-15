#  【安全圈】微软Copilot曝一键式漏洞：攻击者可悄无声息窃取敏感数据  
 安全圈   2026-01-15 11:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
漏洞  
  
  
研究人员发现针对微软Copilot Personal的新型一键式攻击，攻击者可通过该漏洞静默窃取用户敏感数据。该漏洞现已被修复，此前威胁分子可通过钓鱼链接劫持会话而无需进一步交互。  
## 攻击原理：参数注入实现会话劫持  
  
攻击者通过发送包含恶意"q"参数的Copilot合法URL钓鱼邮件发起Reprompt攻击，该参数会在页面加载时自动执行预设指令。这种"参数转指令"（P2P）注入技术利用受害者已认证的会话（即使关闭标签页仍持续有效），可查询用户名、地理位置、文件访问记录和休假计划等个人信息。  
  
攻击链随后采用服务器驱动的后续操作，随着指令动态展开规避客户端检测。  
  
![攻击链示意图](https://mmbiz.qpic.cn/sz_mmbiz_jpg/aBHpjnrGylgWSdE1T4ALWlQD5ttESkB6cmhmrQ1dJ0g9vum6dPJCng1JBb7ezoh4aLV7PtyNLXkblDzibg5pdLw/640?wx_fmt=jpeg&from=appmsg "")  
  
攻击链示意图（来源：Varonis）  
## 三大核心技术突破防护机制  
  
Varonis详细披露了突破Copilot防护机制的三大核心技术，这些防护原本用于阻止URL获取和数据泄露：  
<table><thead><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">技术手段</span></span></section></th><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">工作原理</span></span></section></th><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">绕过方式</span></span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">参数转指令(P2P)</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">通过&#34;q&#34;参数注入指令自动填充并执行，窃取会话记忆或数据</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">要求&#34;双重检查...每个函数调用两次&#34;，在第二次尝试时暴露类似&#34;HELLOWORLD1234!&#34;的密钥</span></span></section></td></tr><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">双重请求</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">Copilot的防泄露机制仅作用于初始请求</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">服务器根据响应生成连续指令，通过分阶段URL实现从用户名获取到时间、位置、用户信息摘要及会话主题的无限泄露链</span></span></section></td></tr><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">链式请求</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">服务器根据响应生成连续指令</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">通过分阶段URL实现从用户名获取到时间、位置、用户信息摘要及会话主题的无限泄露链</span></span></section></td></tr></tbody></table>  
这些技术使数据窃取难以察觉，看似无害的指令背后信息正逐步泄露至攻击者服务器。  
  
![敏感数据泄露示例](https://mmbiz.qpic.cn/sz_mmbiz_jpg/aBHpjnrGylgWSdE1T4ALWlQD5ttESkB6qgGDuXC7WRXY160gJZx82CTl1Hhvh4406mxHjH9ibfo1ct6Abh1CZQw/640?wx_fmt=jpeg&from=appmsg "")  
  
泄露的敏感数据类型（来源：Varonis）  
## 影响范围与修复情况  
  
Reprompt攻击针对集成在Windows和Edge中的Copilot Personal消费者版本，可获取指令记录、历史记录及微软数据（如近期文件或地理位置）。使用Microsoft 365 Copilot的企业用户不受影响，因其具备Purview审计、租户DLP和管理控制等防护措施。  
  
虽然尚未发现实际攻击案例，但通过电子邮件或聊天的一键式攻击低门槛特性，仍对财务计划、医疗笔记等数据构成风险。Varonis于2025年8月31日向微软负贵披露该问题，微软在2026年1月13日的"补丁星期二"发布了修复程序。用户应立即安装最新Windows更新以彻底阻断攻击。  
## 行业警示与防护建议  
  
与此前EchoLeak（CVE-2025-32711）等漏洞不同，Reprompt攻击无需文档或插件即可实施，凸显了AI平台中URL参数的风险。各组织应将AI的URL输入视为不可信来源，并对链式指令实施持续防护。Copilot Personal用户应警惕预填充指令，避免点击不可信链接，并监控异常数据请求行为。  
  
安全专家呼吁微软等厂商深度审计外部输入，在AI应用场景中预设内部级访问控制，以预防类似攻击链。  
  
  
 END   
  
  
阅读推荐  
  
  
[【安全圈】男子 3 个月内 25 次入侵美国最高法院电子文件系统，将出庭认罪](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073709&idx=2&sn=bdcf4fd4004c144accb7db47b946bbd6&scene=21#wechat_redirect)  
  
  
  
[【安全圈】微软桌面窗口管理器0Day漏洞遭野外利用](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073709&idx=3&sn=28e46901cfcbe9c4eaf461852c6db800&scene=21#wechat_redirect)  
  
  
  
[【安全圈】恶意Chrome扩展程序伪装交易工具窃取MEXC交易所API密钥](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073709&idx=4&sn=3fa972269a967909f36a43433f24760d&scene=21#wechat_redirect)  
  
  
  
[【安全圈】河南省考报名系统高峰时段崩溃](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073695&idx=1&sn=14a84617c207be6b51cea6574f210d58&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEDQIyPYpjfp0XDaaKjeaU6YdFae1iagIvFmFb4djeiahnUy2jBnxkMbaw/640?wx_fmt=png "")  
  
**安全圈**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCEft6M27yliapIdNjlcdMaZ4UR4XxnQprGlCg8NH2Hz5Oib5aPIOiaqUicDQ/640?wx_fmt=gif "")  
  
  
←扫码关注我们  
  
**网罗圈内热点 专注网络安全**  
  
**实时资讯一手掌握！**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
**好看你就分享 有用就点个赞**  
  
**支持「****安全圈」就点个三连吧！**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/aBHpjnrGylgeVsVlL5y1RPJfUdozNyCE3vpzhuku5s1qibibQjHnY68iciaIGB4zYw1Zbl05GQ3H4hadeLdBpQ9wEA/640?wx_fmt=gif "")  
  
  
  
  
