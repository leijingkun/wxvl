#  僵尸网络攻击全球Linux服务器、ChatGPT曝高危漏洞|一周特辑  
 威努特安全网络   2026-01-09 23:59  
  
#   
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**GoBruteforcer僵尸网络**  
  
**对全球Linux服务器发起攻击**  
  
  
1月8日，一款名为GoBruteforcer的高复杂度Go语言僵尸网络正对全球Linux服务器发起大规模攻击，通过暴力破解FTP、MySQL等服务的弱密码，已成功入侵数万台设备。2025年的变种在技术上大幅升级，采用深度混淆和进程伪装，并利用了大量因直接套用AI生成的、带有默认弱密码的服务器配置而暴露的目标，使得攻击效率极高。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlUzic8BsnjibUZpHKpeeQsGLN9AjwafugJQwkhPrzdtibpqCOmLNkxiaUHg/640?wx_fmt=png&from=appmsg "")  
  
相关威胁指标（IOC）  
  
  
该僵尸网络不仅窃取服务器资源进行加密货币挖矿，其攻击还兼具广谱扫描与对特定行业（如使用XAMPP、WordPress的服务器）的精准打击能力。防护此威胁的关键在于为所有公网服务实施强密码策略、启用多因素验证，并关闭不必要的对外暴露端口。  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**ChatGPT曝高危漏洞**  
  
**攻击者可从Gmail、GitHub等窃取数据**  
  
  
1月9日，ChatGPT被曝存在一组名为ShadowLeak和ZombieAgent的严重安全漏洞，攻击者可通过恶意文件或邮件中的隐藏指令，在用户无感知的情况下，利用其“连接器”与“记忆”功能，从已绑定的Gmail、GitHub等服务中窃取敏感信息。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlhCPPcU0AUic8GXibichT3lka5656OmhIDAPJ22djiaTG8UUfiaUMSGMEkkA/640?wx_fmt=png&from=appmsg "")  
  
攻击类型及影响范围  
  
  
该漏洞允许实现“零点击”攻击，甚至可修改AI的记忆功能实现持久化数据泄露，并自动传播至受害者联系人。OpenAI已修复相关问题，建议用户加强输入监控与数据净化。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**西班牙国家航空泄露77GB敏感数据**  
  
  
1月5日，网络安全机构Hudson Rock曝光西班牙国家航空——伊比利亚航空遭遇数据泄露事件。威胁组织Zestix通过信息窃取恶意软件获取员工凭证，入侵了该航司的ShareFile云文件共享系统，窃取了约77GB数据，其中包括空客A320/A321机型的技术手册、维修记录、机密机队数据，以及部分客户的姓名、邮箱、电话号码和航班预订码。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlKrlibNUHMiaF3U860HJtoFj1mKaWicI9ZsOTfaW8beoYFGmiaJ2H66jpzg/640?wx_fmt=png&from=appmsg "")  
  
  
伊比利亚航空后续证实其为去年11月已确认的同一旧案，并表示此次泄露不涉及航班运营核心，已向监管机构报备并为受影响客户启用双重验证。据悉，Zestix组织与伊朗籍人员有关联，专门窃取并转售企业系统访问权限。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**工信部发布关于防范**  
  
**SleepyDck恶意软件的风险提示**  
  
  
工业和信息化部网络安全威胁和漏洞信息共享平台（CSTIS）近期发布关于防范SleepyDck恶意软件的风险提示，该软件伪装成合法Solidity工具，通过名称抢注手段在扩展市场传播，一旦激活可窃取系统信息、完全控制Windows主机并窃取敏感数据，造成数据泄露和业务中断等风险。  
  
  
SleepyDuck专门针对使用Cursor、Windsurf等代码编辑器的开发者发起攻击，官方建议开发者立即排查开发环境，仅从官方渠道安装扩展，强化网络安全监控，并阻断与相关恶意域名及以太坊地址的通信。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**全球保险基础设施平台Bolttech**  
  
**遭勒索攻击泄露186GB敏感数据**  
  
  
Everest勒索软件组织1月6日声称已入侵全球保险基础设施平台Bolttech，窃取约186GB高度敏感数据，包括员工与客户信息、保单数据、抵押贷款记录及内部运营标识符等，并威胁若未在限期内支付赎金将公开数据。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlANE0Ojj4OZ6ylFgRhoAPczl6436StqBfxA0wdDyNaSt3ibbH1Mpt66A/640?wx_fmt=png&from=appmsg "")  
  
  
Bolttech作为连接150余家保险公司的数字化平台，年保费处理量超500亿美元，此次泄露可能导致客户面临网络钓鱼、欺诈索赔及人肉搜索等风险。Everest是近年活跃的勒索软件团伙，过去12个月内已攻击超100家组织，此前曾针对华硕、伊比利亚航空等企业实施类似攻击。目前Bolttech尚未公开回应事件，但数据泄露的潜在影响已引发广泛关注。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**n8n平台曝满分高危漏洞**  
  
**可致敏感文件遭未授权窃取**  
  
  
1月7日，开源自动化平台n8n披露一个无需身份验证即可远程利用的严重漏洞（CVE-2026-21858，亦称“Ni8mare”），其CVSS评分高达满分10分。该漏洞源于webhook请求处理中的内容类型混淆缺陷，攻击者通过伪造请求头即可绕过安全限制，直接读取服务器上的任意本地文件，影响全球超过10万台n8n实例。  
  
  
n8n是AI工作流中广泛应用的热门工具，其配置文件中常存有API密钥、OAuth令牌等高敏感信息，此次漏洞可能导致大规模关键凭证泄露。平台已在1.121.0版本中修复此问题，强烈建议所有用户立即升级至该版本或更高版本。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlo4tToyDWJTUNAgwPE6qRMIW0t2jCQ7etd4RY8ic3qodGIISjtMDWvKg/640?wx_fmt=gif&from=appmsg "")  
  
  
**美国Covenant Health遭勒索软件攻击**  
  
**近48万名患者敏感信息泄露**  
  
  
美国医疗服务机构Covenant Health本周确认，其在2025年5月遭遇的勒索软件攻击导致超过47.8万名患者的个人信息泄露，远超此前报告的7800人。此次攻击由Qilin勒索软件团伙实施，该团伙声称窃取了高达850GB的敏感数据。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vEkwp3V9Uttrib9FdCa9NOh8VRecNMWMlBAWX6iamzBia2W2CeWHkEcG3amZwPqzgicsEGZYr0y6MjoGOxQ79UcpAw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
泄露信息包括患者姓名、出生日期、住址、社会安全号码、医疗记录及健康保险详情等。Covenant Health表示已通知受影响个人，并加强了其系统安全。该事件影响了其在多个州的业务，但未造成关键医疗服务中断。  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/vEkwp3V9UttO1byVSbuod03z4vTwBZa0QPXxjXLFBAUUpkYOYz78KpuM3Lic13XvZSLwMRqwPWx2RcI41KF0fcw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/vEkwp3V9Uts5xDIquib8QoxCBg2UE7s4MdTdMuicWrbcPhrED3qiawibOwd3lLX2ge6dXqpzFCY9Y3Pd7pdgX7qC1A/640?wx_fmt=jpeg&from=appmsg "")  
  
渠道合作咨询   田先生 15611262709  
  
稿件合作   微信:shushu12121  
  
