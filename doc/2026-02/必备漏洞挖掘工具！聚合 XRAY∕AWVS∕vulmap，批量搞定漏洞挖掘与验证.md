#  必备漏洞挖掘工具！聚合 XRAY/AWVS/vulmap，批量搞定漏洞挖掘与验证  
78778443
                    78778443  渗透安全HackTwo   2026-02-11 16:00  
  
0x01 工具介绍  
  
  
在漏洞挖掘与渗透测试中，高效整合多款扫描工具、批量处理目标是提升效率的核心需求。QingScan 作为一款热门聚合型漏洞挖掘工具（GitHub 1.8k 星标），完美解决了工具分散、操作繁琐的痛点！它不局限于单一扫描功能，而是深度整合 XRAY、AWVS、vulmap 等 30 款主流工具，覆盖 Web 扫描、主机探测、子域名收集、目录扫描、POC 批量验证、SSH 测试等全场景需求。只需添加目标，工具便自动联动执行扫描，所有结果聚合展示，无需手动切换操作。  
  
注意：  
现在只对常读和星标的公众号才展示大图推送，建议大家把  
**渗透安全HackTwo**  
"**设为****星标⭐️**  
"  
**否****则可能就看不到了啦！**  
  
**下载地址在末尾 #渗透安全HackTwo**  
  
0x02   
功能简介  
  
  
✨ 主要特性  
#### 多工具聚合联动，覆盖全场景扫描  
- 无需手动切换各类扫描工具，添加目标后自动调用 XRAY、AWVS、vulmap、nmap 等 30 款主流扫描工具；  
  
- 覆盖 Web 漏洞扫描、系统漏洞探测、子域名收集、目录扫描、主机发现、组件识别、URL 爬虫、POC 批量验证、SSH 批量测试等全维度安全检测场景，满足渗透测试全流程需求。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVjXHS40icicZibPIkricT7ycYB1PPiafTkMKESaXS8vogjSkvAib4mib7okpotgT5Kd8s7YY7XR6l3CkUbleaeqA5DMhsXylIgq9Ee1g/640?wx_fmt=png&from=appmsg "")  
#### 自动化扫描，大幅提升效率  
- 仅需录入目标资产，工具自动完成多维度扫描任务调度，无需人工干预；  
  
- 扫描结果自动录入平台并聚合展示，统一整理不同工具的检测数据，避免分散查看、重复分析，显著降低人工成本。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAVk8iaMld8aPN0qF7VY4na6L9YhrpUPVUc1aiccichCTgzTR7K14K4g2YGcgRjRNdMMuPJFw8KjBsmtTBXOVOmlNAF48s39Cn9Zlw/640?wx_fmt=png&from=appmsg "")  
#### 灵活的工具管理机制  
- 支持精细化工具安装：可通过 PHP 命令查看可安装工具列表，按需安装指定工具（如 nmap）或一键安装全部工具；  
  
- 适配 Ubuntu22.04 系统，提供自动安装脚本，简化部署流程，降低使用门槛。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ibrevicNauKAX3l5VqbvgK2QVYU7zaiaCT1YwLiaxBf8p4nibBUZxy3msGPmeR0OfiaFMDvkFuicRJ2ZugVqhmUlvxkGuedUicX4QVM4rNHc8qQtu1A/640?wx_fmt=png&from=appmsg "")  
#### 可视化 Web 管理界面  
- 基于 PHP 搭建轻量化 Web 页面，无需命令行操作即可完成目标添加、扫描任务管理、结果查看；  
  
- 扫描结果分类清晰，便于快速定位高风险漏洞，提升漏洞分析与处置效率。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAUf6z8zWqicbdicm2BsFOa4qLMkMMcOLWz9e4uXu1aEFiaGsODLInRBX6RicvTYqMEy7JK8oJAb2TskPMIYDYthGpUqJOc57AcTkgo/640?wx_fmt=png&from=appmsg "")  
#### 高度可扩展，支持定制化需求  
- 本身不限制扫描能力边界，可灵活适配新增扫描工具的整合需求；  
  
- 提供私人订制服务，支持二次开发适配企业 / 个人的个性化漏洞挖掘场景。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ibrevicNauKAVdNPHd3sEgk4SxwO9ggTQXTicSdV1LNo0jQZpVicbPYhEicmKqb6YukDcuWpuGBdkbabZm6CvZ4xCPFWqPZ0lbeV0g3vBJSx2UgY/640?wx_fmt=png&from=appmsg "")  
#### 合规安全的使用边界  
- 仅面向合法授权的企业安全建设行为，工具设计聚焦漏洞检测与安全评估，规避非授权使用风险；  
  
- 结果仅用于安全加固，配套完善的使用规范与免责说明，保障合规使用。  
  
0x03更新说明  
```
1、更新镜像地址
2、相关工具的更新
3、关闭默认的进程
```  
  
  
0x04 使用介绍  
  
📦安装  
指南  
  
视频教程  
  
  
需要再Ubuntu22.04系统下安装，其他系统请自行安装  
1. 安装PHP扩展和项目依赖  
  
```
apt install php php-xml php-gd php-mysqli php-dom php-cli php-zip unzip php-curl composer
cd QingScan/code && composer install  
```  
1. 用PHP启动项目web页面  
  
```
php think run -p 80
```  
1. 新建数据库，并导入数据表，SQL文件在deploy  
下的qingscan.sql  
  
1. 访问web页面  
  
```
http://127.0.0.1/
```  
1. 启动调用脚本  
  
```
./script.sh
```  
### 工具安装  
  
QingScan 提供了两种方式来安装所需的扫描工具：  
#### 方法一：使用PHP命令安装（推荐）  
```
# 查看可安装的工具列表
php think install list
# 安装所有工具
php think install all
# 安装指定工具
php think install nmap
```  
  
  
**0x05 内部VIP星球介绍-V1.4（福利）**  
  
        如果你想学习更多**渗透测试技术/应急溯源/免杀工具/挖洞SRC赚取漏洞赏金/红队打点等**  
欢迎加入我们**内部星球**  
可获得内部工具字典和享受内部资源和  
内部交流群，  
**每天更新1day/0day漏洞刷分上分****(2026POC更新至5212+)**  
**，**  
包含全网一些**付费扫描****工具及内部原创的Burpsuite自动化漏****洞探测插件/漏扫工具等，AI代审工具，最新挖洞技巧等**  
。shadon/Zoomeye/Quake/  
Fofa高级会员，CTFShow等各种账号会员共享。详情点击下方链接了解，觉得价格高的师傅后台回复"   
**星球**  
 "有优惠券名额有限先到先得  
**❗️**  
啥都有  
**❗️**  
全网资源  
最新  
最丰富  
**❗️****（🤙截止目前已有2400+多位师傅选择加入❗️早加入早享受）**  
  
****  
最新漏洞情报分享：  
https://t.zsxq.com/lFN5j  
  
****  
  
**👉****点击了解加入-->>内部VIP知识星球福利介绍V1.4版本-1day/0day漏洞库及内部资源更新**  
  
****  
  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
**公众号回复20260212获取下载**  
  
# 最后必看-免责声明  
  
  
      
文章中的案例或工具仅面向合法授权的企业安全建设行为，如您需要测试内容的可用性，请自行搭建靶机环境，勿用于非法行为。如  
用于其他用途，由使用者承担全部法律及连带责任，与作者和本公众号无关。  
本项目所有收录的poc均为漏洞的理论判断，不存在漏洞利用过程，不会对目标发起真实攻击和漏洞利用。文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用。  
如您在使用本工具或阅读文章的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。本工具或文章或来源于网络，若有侵权请联系作者删除，请在24小时内删除，请勿用于商业行为，自行查验是否具有后门，切勿相信软件内的广告！  
  
  
  
# 往期推荐  
  
  
**1.内部VIP知识星球福利介绍V1.4（AI自动化工具）**  
  
**2.CS4.8-CobaltStrike4.8汉化+插件版**  
  
**3.全新升级BurpSuite2025.12专业(稳定版)**  
  
**4. 最新xray1.9.11高级版下载Windows/Linux**  
  
**5. 最新HCL AppScan Standard**  
  
  
渗透安全HackTwo  
  
微信号：关注公众号获取  
  
后台回复星球加入：  
知识星球  
  
扫码关注 了解更多  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6qFFAxdkV2tgPPqL76yNTw38UJ9vr5QJQE48ff1I4Gichw7adAcHQx8ePBPmwvouAhs4ArJFVdKkw/640?wx_fmt=png "二维码")  
  
  
  
上一篇文章：  
[Nacos配置文件攻防思路总结|揭秘Nacos被低估的攻击面](https://mp.weixin.qq.com/s?__biz=Mzg3ODE2MjkxMQ==&mid=2247492839&idx=1&sn=b6f091114fbd8e8922153a996c8f4f1c&scene=21#wechat_redirect)  
  
  
