#  端口扫描 + 指纹识别 + POC 验证｜cscan 一站式资产安全扫描平台  
tangxiaofeng7  渗透安全HackTwo   2026-01-05 16:01  
  
0x01 工具介绍  
  
  
在资产安全扫描领域，工具的轻量化、高性能与全流程覆盖始终是核心需求。市面上多数工具或笨重难用，或功能单一，难以适配大规模资产梳理场景。cscan 分布式资产安全扫描平台应运而生，基于 Go-Zero 架构打造，集成端口扫描、指纹识别、POC 验证全流程能力。依托 Go 协程优势实现极速扫描，搭配 Vue3 可视化界面，让资产发现与风险识别更高效。本文记录平台开发历程与核心特性，助力安全从业者快速搞定资产盘点与漏洞排查。  
  
**注意：**  
现在只对常读和星标的公众号才展示大图推送，建议大家把  
**渗透安全HackTwo**  
"**设为****星标⭐️**  
"  
**否****则可能就看不到了啦！**  
  
**下载地址在末尾 #渗透安全HackTwo**  
  
0x02   
功能简介  
  
  
✨ 主要特性  
  
资产发现：  
端口扫描 (Naabu/Masscan)，端口服务识别 (Nmap)  
  
子域名枚举：   
Subfinder 集成，支持多数据源  
  
指纹识别 ：  
Httpx + Wappalyzer + 自定义指纹引擎，3W+ 指纹规则  
  
漏洞检测 ：  
Nuclei SDK 引擎，800+ 自定义 POC  
  
Web 截图：  
 Chromedp / HTTPX 引擎  
  
在线数据源：  
 FOFA / Hunter / Quake API 聚合搜索与导入  
  
报告管理：  
任务报告生成，支持 Excel 导出  
  
分布式架构：  
Worker 节点水平扩展，支持多节点并行扫描  
  
多工作空间：  
项目隔离，团队协作  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6Fhs0wVf9v0Ar7vJEZJ33CcDibbzGAVN9ygOFGAq097WfkTo8JjCTgsuDKCV4m60GEzfNwkQ12iavw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6Fhs0wVf9v0Ar7vJEZJ33CSqhia0CkYZ731xqeCKTDZOhMU1MDEWFWvfJHTUMxmSRvKttlQmVvxMA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6Fhs0wVf9v0Ar7vJEZJ33CY0plz2w5fpaphoDR2r4Qvm3l1n2d08YJrEGcSF6iar2QHx5ZQibHd3iaQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6Fhs0wVf9v0Ar7vJEZJ33CnQuiaVkuOL5GzDTeiapXvEHsS7c76ezbEmVt53gn2ssTUiaaxvAytNDUQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RjOvISzUFq6Fhs0wVf9v0Ar7vJEZJ33CBM38n6lI2t2YKGnvzYSqyZ8oE6IvI8zxBmRR8o8knhm8fhaliafZtMA/640?wx_fmt=png&from=appmsg "")  
  
  
0x03更新说明  
```
🎉优化前端BUG
```  
  
  
0x04 使用介绍  
  
📦  
使用指南  
  
下载cscan  
```
cd cscan
docker-compose up -d
```  
  
访问 http://ip:3000，默认账号 admin / 123456  
## 本地开发  
  
1. 启动依赖服务  
```
docker-compose -f docker-compose.dev.yaml up -d
```  
  
2. 启动后端服务  
```
# RPC 服务
go run rpc/task/task.go -f rpc/task/etc/task.yaml
# API 服务
go run api/cscan.go -f api/etc/cscan.yaml
```  
  
3. 启动 Worker  
  
从 Web 界面获取安装密钥后：  
```
go run cmd/worker/main.go -k <install_key> -s http://localhost:8888
```  
  
Worker 参数说明：  
<table><thead><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.606061px solid rgba(209, 217, 224, 0.7);"><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">参数</span></span></section></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">说明</span></span></section></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">默认值</span></span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.606061px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin-top: 0px;margin-right: 0px;margin-left: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf=""><span textstyle="" style="font-size: 16px;">-k</span></span></code></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">安装密钥 (必须)</span></span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">-</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.606061px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin-top: 0px;margin-right: 0px;margin-left: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf=""><span textstyle="" style="font-size: 16px;">-s</span></span></code></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">API 服务地址 (必须)</span></span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">http://localhost:8888</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.606061px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin-top: 0px;margin-right: 0px;margin-left: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf=""><span textstyle="" style="font-size: 16px;">-n</span></span></code></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">Worker 名称</span></span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">自动生成</span></span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.606061px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin-top: 0px;margin-right: 0px;margin-left: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf=""><span textstyle="" style="font-size: 16px;">-c</span></span></code></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">并发数</span></span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.606061px;border-image: none 100% / 1 / 0 stretch;"><section style="margin-bottom: 16px;"><span leaf=""><span textstyle="" style="font-size: 16px;">5</span></span></section></td></tr></tbody></table>  
4. 启动前端  
```
cd web
npm install
npm run dev
```  
  
访问 http://localhost:3000  
  
部署  
  
Docker Compose (推荐)  
```
docker-compose up -d
```  
  
独立 Worker 部署  
  
Worker 支持独立部署在任意服务器，只需连接到 API 服务：  
```
# Linux
./cscan-worker -k <install_key> -s http://<api_host>:8888
# Windows
cscan-worker.exe -k <install_key> -s http://<api_host>:8888
```  
  
  
  
**0x05 内部VIP星球介绍-V1.4（福利）**  
  
        如果你想学习更多**渗透测试技术/应急溯源/免杀工具/挖洞SRC赚取漏洞赏金/红队打点等**  
欢迎加入我们**内部星球**  
可获得内部工具字典和享受内部资源和  
内部交流群，  
**每天更新1day/0day漏洞刷分上分****(2026POC更新至5021+)**  
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
**❗️****（🤙截止目前已有2300+多位师傅选择加入❗️早加入早享受）**  
  
****  
最新漏洞情报分享：  
https://t.zsxq.com/cfn15  
  
****  
  
**👉****点击了解加入-->>内部VIP知识星球福利介绍V1.4版本-1day/0day漏洞库及内部资源更新**  
  
****  
  
  
结尾  
  
# 免责声明  
  
  
# 获取方法  
  
  
**公众号回复20260106获取下载**  
  
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
  
  
