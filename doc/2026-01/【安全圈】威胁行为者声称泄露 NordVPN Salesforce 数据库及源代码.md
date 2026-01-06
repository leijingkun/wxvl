#  【安全圈】威胁行为者声称泄露 NordVPN Salesforce 数据库及源代码  
 安全圈   2026-01-06 11:01  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgOvEXHviaXu1fO2nLov9bZ055v7s8F6w1DD1I0bx2h3zaOx0Mibd5CngBwwj2nTeEbupw7xpBsx27Q/640?wx_fmt=other&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")  
  
  
**关键词**  
  
  
  
数据泄露  
  
  
一位使用标识符1011进行活动的威胁行为者在暗网论坛上公开声称，其**已获取并泄露了来自NordVPN开发基础设施的敏感数据**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/aBHpjnrGylgjibmJHTxIiacssGuaeh7CiaRSjrWjiaoqRvkqBfdQkDsuvmrxdNrOc4FaO4mTuMxeO2Pwl1Tia3UzNow/640?wx_fmt=png&from=appmsg "")  
  
据报道，此次泄露暴露了**超过十个数据库的源代码**  
，以及可能对该VPN提供商运营安全构成重大风险的关键身份验证凭证。  
  
攻击者声称，他们是通过位于巴拿马的一台配置错误的开发服务器获得访问权限的。这一发现凸显了整个科技行业存在安全防护不足的开发环境所带来的持续脆弱性。  
  
**根据最初的披露，泄露的受损数据包括NordVPN核心系统的源代码仓库、Salesforce API密钥和Jira令牌。**  
  
这些凭证允许直接访问用于客户关系管理和项目跟踪的关键业务工具。  
  
该威胁行为者已发布了样本SQL转储文件，揭示了敏感数据库表的结构，包括salesforce_api_step_details  
表和api_keys  
配置，这证明了其对NordVPN后端基础设施的访问权限。  
  
Dark Web Informer的分析师在2026年1月4日威胁行为者在暗网论坛分享证据后，确认了此次泄露。  
  
研究人员指出，这一事件例证了开发服务器为何常常因其相比生产环境更为宽松的安全配置而成为有吸引力的目标。  
  
数据库架构信息和API密钥结构的泄露，显著增加了针对NordVPN更广泛生态系统发起后续攻击的风险。  
  
此次攻击途径的核心是针对配置错误服务器进行凭证暴力破解，这种技术对于缺乏适当速率限制和访问控制的系统而言，仍然具有令人不安的效力。  
  
这种方法涉及系统性地尝试各种密码组合直至获得进入权限，在防御措施缺失或不足的情况下，这是一种直接但有效的手段。  
  
此次泄露与标准数据盗窃的不同之处在于源代码本身的暴露，这使得攻击者获得了数百万用户赖以保护隐私的系统的架构知识。  
  
其影响超出了NordVPN的直接运营范围。随着API密钥和Jira令牌进入公开流通，威胁态势已扩大到包括在集成服务内进行潜在的横向移动，以及对内部项目管理系统的可能操纵。  
  
安全研究人员建议NordVPN立即对所有开发基础设施进行安全审计，在所有平台上轮换已泄露的凭证，并通过强制执行多因素认证来加强身份验证协议。  
  
管理类似开发环境的组织应实施更严格的访问控制和持续监控，以防止发生类似的泄露事件。  
  
  
  END    
  
  
阅读推荐  
  
  
[【安全圈】《英雄联盟》证书过期登热搜！草台班子闯大祸？](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=1&sn=d369456917b21dd3d87047f028e3d9fa&scene=21#wechat_redirect)  
  
  
  
[【安全圈】黑客入侵员工网络时落入Resecurity蜜罐陷阱](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=2&sn=56bf0dc5381b8bb1a65ba9902ce5aeb2&scene=21#wechat_redirect)  
  
  
  
[【安全圈】自传播恶意软件GlassWorm利用VS Code扩展攻击macOS用户](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=3&sn=74679f09ae68daf09fdc27f17f57c6e9&scene=21#wechat_redirect)  
  
  
  
[【安全圈】别乱用！下载的Skills竟成后门，聊聊Skills不为人知的安全风险](https://mp.weixin.qq.com/s?__biz=MzIzMzE4NDU1OQ==&mid=2652073568&idx=4&sn=0e19f07eec0b7cf1b18c9a05a9f2e9f7&scene=21#wechat_redirect)  
  
  
  
  
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
  
  
  
