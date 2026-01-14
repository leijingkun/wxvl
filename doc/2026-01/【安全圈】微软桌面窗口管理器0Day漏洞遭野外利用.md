#  【安全圈】微软桌面窗口管理器0Day漏洞遭野外利用  
 安全圈   2026-01-14 11:00  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
漏洞  
  
  
   
  
![image](https://mmbiz.qpic.cn/sz_mmbiz_jpg/aBHpjnrGyljibqcYfxGVuS5yOFACw8PZsib0wtxD9RGP3gNSXG1mbJBgnstxMtAmuTBEWaK8ITbiaLfNXjHgPJO4A/640?wx_fmt=jpeg&from=appmsg "")  
  
微软在2026年1月13日的"补丁星期二"更新中修复了其桌面窗口管理器（DWM）中的一个关键0Day信息泄露漏洞，此前已监测到该漏洞在野外遭到主动利用。  
  
该漏洞编号为CVE-2026-20805，允许低权限本地攻击者通过远程ALPC端口暴露敏感的用户模式内存（特别是段地址）。在实际攻击中，这可能有助于构建进一步的权限提升攻击链，促使微软紧急为旧版Windows系统部署补丁。  
  
该漏洞被评为"重要"严重等级，CVSS v3.1基础得分为5.5（AV:L/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N）。虽然无法远程利用，但其低复杂度且无需用户交互的特点，使其成为恶意软件或入侵后操作的主要目标。  
  
微软威胁情报中心（MSTIC）和安全响应中心（MSRC）确认了漏洞利用行为，但指出目前尚未出现公开的PoC。  
  
攻击者利用DWM（负责窗口渲染的核心合成引擎）来泄露内存地址。这种泄露可能暴露内核指针或进程数据，有助于绕过ASLR等缓解措施。微软通过协调披露机制感谢内部团队发现该漏洞。  
## 受影响平台及补丁  
  
该漏洞影响仍处于扩展支持阶段的旧版Windows系统。由于微软将这些更新标记为"必需"，管理员必须优先部署。  
<table><thead><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">平台</span></span></section></th><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">KB文章</span></span></section></th><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">内部版本号</span></span></section></th><th style="box-sizing: border-box;text-align: center;margin: 0px;padding: 0px;border-top: 1px solid rgb(204, 204, 204);border-right: 1px solid rgb(204, 204, 204);border-bottom: none;border-left: 1px solid rgb(204, 204, 204);border-image: initial;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: 550;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;background: rgb(245, 245, 245);"><section><span leaf=""><span textstyle="" style="font-size: 17px;">下载链接</span></span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">Windows 10 v1809 (x64/32位)</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">KB5073723</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">10.0.17763.8276</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">目录</span></span></section></td></tr><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">Windows Server 2012 R2 (核心/完整)</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">KB5073696</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">6.3.9600.22968</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">目录</span></span></section></td></tr><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">Windows Server 2012 (核心/完整)</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">KB5073698</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">6.2.9200.25868</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">目录</span></span></section></td></tr><tr style="box-sizing: border-box;margin: 0px;padding: 0px;border: 0px;text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;"><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">Windows Server 2016 (核心/完整)</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">KB5073722</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">10.0.14393.8783</span></span></section></td><td style="box-sizing: border-box;margin: 0px;padding: 5px;border: 1px solid rgb(204, 204, 204);text-decoration: none;font-style: inherit;font-variant: inherit;font-weight: inherit;font-stretch: inherit;font-size: inherit;line-height: inherit;vertical-align: baseline;font-family: 微软雅黑, &#34;Microsoft YaHei&#34;, &#34;WenQuanYi Micro Hei&#34;, PingFangSC;outline: none;text-align: left;word-break: break-word;white-space: pre-wrap;"><section><span leaf=""><span textstyle="" style="font-size: 17px;">目录</span></span></section></td></tr></tbody></table>  
请查阅MSRC更新获取完整生命周期详情。在此期间，建议限制本地低权限账户并通过EDR工具监控DWM进程。  
  
这轮补丁更新突显了在本地权限提升技术日益盛行的情况下，旧版DWM组件面临的持续风险。使用不受支持版本的组织面临更高的暴露风险。  
  
  
 END   
  
  
阅读推荐  
  
  
[【安全圈】河南省考报名系统高峰时段崩溃](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073695&idx=1&sn=14a84617c207be6b51cea6574f210d58&scene=21#wechat_redirect)  
  
  
  
[【安全圈】知名黑客论坛数据库泄露！逾32万名用户数据被公开](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073695&idx=2&sn=62a46a42e6435c6d3c12e63e2f5a0d27&scene=21#wechat_redirect)  
  
  
  
[【安全圈】Telegram 暴露用户真实IP地址，可一键绕过 Android 和 iOS 代理](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073695&idx=3&sn=4658cd602aecc29e2466f392d21b54a4&scene=21#wechat_redirect)  
  
  
  
[【安全圈】GoBruteforcer 僵尸网络利用弱凭证攻击加密货币项目数据库](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073695&idx=4&sn=080d75ef27ce9b31399927cffaf20808&scene=21#wechat_redirect)  
  
  
  
  
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
  
  
  
  
