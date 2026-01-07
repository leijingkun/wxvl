#  漏洞情报(已验证) | 时空智友企业流程化管控系统formservice 存在 XXE漏洞  
原创 wdgs  WDCIA   2026-01-06 23:00  
  
动动小手  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/wR4G22TgErjfNpreeU6zMsUZOotoQgCSFfDdOpb1BgkAUaA1Ijhdd2MbcXKK6cwvfkY6G7m0BRoUt22zXu4nMQ/640?from=appmsg "")  
  
点个  
关注  
～  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/HtBSeqUljlEX2U86CRIHg5OYkFjEichJCBOiao1DictoCzicWNibby5tkTS2mHic455icX62rECv7RrxTavz0Q4xHRZdw/640?from=appmsg "")  
  
内容仅用于学习交流自查使用，由于传播、利用本公众号所提供的POC信息及POC对应脚本而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号WDCIA及作者不为此承担任何责任，一旦造成后果请自行承担！  
  
如文章有侵权烦请及时告知，我们会立即删除文章并致歉。谢谢！  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/3P3vXeGVfAjC9ib3Kr2ouds4E1crXD1wROicQHa8JpRHJEdxsIicN9t1fhUrsnyEEzgynlxZ3cGOdXkhGfpyAh0YQ/640?from=appmsg "")  
  
# 前情回顾11.25-20260105已更新如下金和C6高端协同管理平台outerappTIDSave.aspx存在SQL注入漏洞 金和OA XXE-Examinenodsingletonxml.aspx漏洞CVE-2025-58360天地伟业-easy7-getAuthorityByUserId-SQL注入 天地伟业-easy7-queryUserbyDesc-SQL注入用友时空-KSOA-agent_work_report.jsp-sql注入未分类的 nuclei 合集喰星云-数字化餐饮服务系统home_check SQL注入天地伟业-easy7-addGateWayOptLog-SQL注入友加畅捷U+财会通-DataHandler.ashx-XXESmartbi未授权远程代码执行漏洞恒友儿童影楼ERP myTask.ashx SQL注入天地伟业-easy7-addUserZwTz-SQL注入致远后台任意代码执行漏洞友加畅捷U+RepFile.ashx文件上传金和OA-PersonalBankEdit.aspx-SQL注入、获取session 值SRM智联云采系统-verifyToken-反序列化福建科立讯通信-指挥调度平台-uploadtext.php-SQL注入某某蓝凌零day东胜物流软件-ZWCCX.aspx-SQL注入东胜物流软件-GetAuthorityRange-SQL注入朗新天霁人力资源管理系统StandardBusiness.asmx SQL注入金和C6-ProjectPlanReportDetail.aspx-SQL注入用友时空KSOA-updateResult.jsp-SQL注入帆软报表FineReport export/excel接口存在SQL注入漏洞东方通 TongWeb 应用服务器 ejbserver 远程代码执行孚盟云AjaxCustomerList.ashx-SQL注入孚盟云-AjaxCustomerInfoAtion-SQL注入安科瑞-GetEnterpriseInfoYHM-SQL注入宏景-ajaxAnonService-SQL注入云网（一米）OA 任意文件上传0day东胜物流软件-GetLanesList-SQL注入福建科立讯通信-指挥调度平台phoneeventlist.php-sql注入金蝶OA-0day 文件上传PostilUpload 及一些某某二开工具  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/nvJGLMmlzsEzkvjgiaMSVwYsrQu8shpFs8ia6lIt50QZMhX9rXbskKrcPpzAVPJwQSbRS4lq74ouNoZT3iac2icjicQ/640?wx_fmt=gif&from=appmsg "")  
  
**01**  
  
**漏洞介绍**  
  
      
  
      
  
      
时空智友企业流程化管控系统formservice 存在 XXE漏洞，  
未经身份验证攻击者可以通过此漏洞读取服务器上敏感文件或探测内网服务信息，进一步利用可导致服务器失陷。  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/nvJGLMmlzsEzkvjgiaMSVwYsrQu8shpFs8ia6lIt50QZMhX9rXbskKrcPpzAVPJwQSbRS4lq74ouNoZT3iac2icjicQ/640?wx_fmt=gif&from=appmsg "")  
  
**02**  
  
**影响版本**  
```
V10.1
```  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/nvJGLMmlzsEzkvjgiaMSVwYsrQu8shpFs8ia6lIt50QZMhX9rXbskKrcPpzAVPJwQSbRS4lq74ouNoZT3iac2icjicQ/640?wx_fmt=gif&from=appmsg "")  
  
**03**  
  
**测绘语法**  
  
  
测绘语句:  
```
关注公众号，回复 时空智友 获取
```  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/nvJGLMmlzsEzkvjgiaMSVwYsrQu8shpFs8ia6lIt50QZMhX9rXbskKrcPpzAVPJwQSbRS4lq74ouNoZT3iac2icjicQ/640?wx_fmt=gif&from=appmsg "")  
  
**04**  
  
**POC**  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/nvJGLMmlzsFTqFT8n4UaDc2AUvlicqu2aGyRIZI04ZHdeXK2VyL3sI0t1JABbuAkk0jV4gqicfVn7Eicv6vNgbxdg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/nvJGLMmlzsFTqFT8n4UaDc2AUvlicqu2aDxjKAOjIib3ErxoKiaXSZiarHa5UJia17oeXYBBSp3hlNG8f2iaOIDrPyLA/640?wx_fmt=png&from=appmsg "")  
  
POC已放置圈子  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/HtBSeqUljlEX2U86CRIHg5OYkFjEichJCBOiao1DictoCzicWNibby5tkTS2mHic455icX62rECv7RrxTavz0Q4xHRZdw/640?from=appmsg "")  
  
圈子  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/3P3vXeGVfAjC9ib3Kr2ouds4E1crXD1wROicQHa8JpRHJEdxsIicN9t1fhUrsnyEEzgynlxZ3cGOdXkhGfpyAh0YQ/640?from=appmsg "")  
  
  
  
    圈子刚刚成立，专注于漏洞情报分享，不发烂大街的东西，不定时分享红队工具，欢迎加入，每日更新1-2个 0/1 Day（目前已更新100+）  
  
圈子新人优惠券已发放完，后期不定时发送，价格随人数增加而增加  
  
![](https://mmbiz.qpic.cn/mmbiz_png/nvJGLMmlzsFvvkgWGewJdZJCcnVTJDMQ136zQewU0FWFNeAqlquOvoogc5WziaJYVQXU6YJ6LtK6bO6NyanWViaw/640?wx_fmt=png&from=appmsg "")  
  
  
  
往期推荐  
  
  
[漏洞情报(已验证) | 0 Day 多客圈子论坛系统getManageinfo 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483920&idx=1&sn=b58bf8b8338aafc7de284cd7313105ea&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 西部数码（NAS）internal_backup.php 存在RCE漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483919&idx=1&sn=f980ccbeb319ea4410ed1c0c38ed296f&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 金和OA C6 ArchivesDocNew.aspx 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483918&idx=1&sn=840ab0be4f3d2d1f62b486f0b8807f46&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 西部数码（NAS）usb_backup.php 存在RCE漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483905&idx=1&sn=848753a36a7716513b3982820133bf76&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 天锐绿盘云 getNextAutoCode 存在Fastjson反序列化漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483900&idx=1&sn=93940af6f3abf1a4f1f1fd8b19479260&scene=21#wechat_redirect)  
  
  
[漏洞复现 | 用友 U8 Cloud 文件上传绕过漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483895&idx=1&sn=d0bbaa35a7ea04d8a63b8c56285ca8b0&scene=21#wechat_redirect)  
  
  
[漏洞预警已复现| 0 Day 力软敏捷开发框架***********ad存在任意文件上传漏洞(非CrawlerHandler)](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483887&idx=1&sn=a00974871b90a9d61f0867e8544f6d94&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 百易云资产管理运营系统 bankTradePlan.save.php 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483879&idx=1&sn=f7b000b0695b853ec70c75cc7d0b88cd&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 万户OA getNextAutoCode 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483873&idx=1&sn=dc82d0c0ce79856e640eacf8d1647cd0&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 用友NC getOtherData 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483869&idx=1&sn=ce152b7ff79798b14bc3c1a86e6a28d6&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 昂捷CRM CWSOffice.asmx 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483865&idx=1&sn=7eb53efeccdfdd2b54de1d80c4a3f064&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 安科瑞 GetEnterpriseInfoMapHM 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483854&idx=1&sn=545a440d437413127400cd2f1e95e49a&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 蓝凌EIS validate 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483848&idx=1&sn=d12519a4cc56a9caee1fb8f61cde7517&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 1 Day 富勒仓储管理系统 getExportTemplateFileAction 任意文件下载漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483824&idx=1&sn=f5bf083aec3bf790924e7089c8398654&scene=21#wechat_redirect)  
  
  
[漏洞预警 | 0/1 Day 天锐绿盾审批、天锐绿盘、天锐数据漏洞合集](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483812&idx=1&sn=633e4f7cd66606dfbbe68f1a5140f8df&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 用友时空KSOA del_catalog.jsp 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483791&idx=1&sn=b433d9d66322be5d9fd48257c9ae3bca&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 1 Day 帆软pdf接口远程命令执行漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483768&idx=1&sn=264e2b4abb63eb6be34543186a6b3d1c&scene=21#wechat_redirect)  
  
  
[【全网首发】金和OA 0/1/N Day漏洞合集](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483755&idx=1&sn=a01d7046d23ac676869feeb5c782bb2e&scene=21#wechat_redirect)  
  
  
[漏洞情报 | 0 Day红帆OA NetCAUserLogin.aspx存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483667&idx=1&sn=456d11f8d2b6f1a6dc3377f44f19f272&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 科立讯指挥调度管理平台 get_user_xxxxxx.php 存在SQL注入漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483712&idx=1&sn=6db67e3f67ee9467224aaedeb8bbb643&scene=21#wechat_redirect)  
  
  
[漏洞情报(已验证) | 0 Day 金和OA XMLHttp.aspx存在XXE漏洞](https://mp.weixin.qq.com/s?__biz=MzUyNzk1NjA5MQ==&mid=2247483729&idx=1&sn=ae6688f9e53933ee98ebe781ebb1ca8d&scene=21#wechat_redirect)  
  
  
  
