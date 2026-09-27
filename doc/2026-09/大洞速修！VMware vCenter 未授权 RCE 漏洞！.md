#  大洞速修！VMware vCenter 未授权 RCE 漏洞！  
原创 ChinaRan404
                    ChinaRan404  知攻善防实验室   2026-09-26 02:17  
  
   
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/a3etiafIAYXGFzhbQSyxdOTXBhMT7RovdU3gsN30YZfssWd4aVhjHzmVgygdhGMmkGVXf6hib8jjxXWnhH31PvdhLpbTyibiaqmvwh7vA4APBzU/640?wx_fmt=gif&from=appmsg "")  
  
简报  
  
  
   
  
利用条件：  
  
利用前置：仅需能访问 UDP/TCP 514，无需任何凭据  
  
影响版本  
```
VMware vCenter 9.1 分支 < 9.1.0.0300
VMware vCenter 9.0 分支 < 9.0.2.0100
VMware vCenter 8.0 U3 分支 < 8.0 U3k
VMware vCenter 8.0 U2 分支 < 8.0 U2f
VMware vCenter 8.0 初始版本及 U1 分支
VMware vCenter 7.0 分支中未安装对应扩展支持补丁的版本
```  
  
上述范围包括独立部署，以及 VMware Cloud Foundation、VMware vSphere Foundation 和 VMware Telco Cloud 产品中使用的受影响 vCenter 组件。  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/a3etiafIAYXGBZqT9w7vRDv249KtsFicpbpCXy3icCVgGd5FajmbIIu8dRa89yRF7Vt7PSBlJDzPeatmUIcib9JyqyEEVWdNnAE3HzMKxUuGx1Y/640?wx_fmt=gif&from=appmsg "")  
  
利用链  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/a3etiafIAYXGbJ8VR1FVhzJBzhXCcn2ibrw80IAJfLrNt9sSD5OZrYtj85X1dueqp4OkbibD622dqLVhxZrFwFzuSeELwf7OeSiaZrRLibyWfrcs/640?wx_fmt=gif&from=appmsg "")  
  
  
完整利用链：无认证 UDP 包 → 任意文件写（root）→ 计划任务 → 任意代码执行（root）。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/a3etiafIAYXFH6zX1vQQj8fgmaksm3MlXkajmBgibtuusLgQDVYSakU6PlSzrnyKuw0TZYPwEKXS5JcykJLcSwElOssVEDyRHauedEjcDyfPc/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/a3etiafIAYXGjKgwEpWjQ5aef10KBk6Ujkh8CAjygiaIlz2ssNhBMIF6dDym0YRbJ7pibUzlvV8aZu9uHak754lfzHIPObvqvt72Pxjic0HWCcc/640?wx_fmt=png&from=appmsg "")  
  
POC:  
https://github.com/ChinaRan0/CVE-2026-59310-POC  
   
  
请勿在生产环境直接复测，如需验证漏洞存在请用本地测试环境  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/a3etiafIAYXHNxNkReh7CjHX3ec7XfRLALhpAhBeOjfibuPYujRG0kKXlo6PiaaSZYmyiagYoZicNLMMbZicEycbqfjiaRvGTlAicmyAeOImAicyhdrw/640?wx_fmt=gif&from=appmsg "")  
  
临时缓解（无法立即升级时）  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/a3etiafIAYXF3N912GOe0h6CelPKd8EOjyIwYgBMfibq1taGsmo8sO0LAxWmqpINb10l27vLF5ZticgntUBEEnojGaUnTMBe3yOCBL6p5ZTOj4/640?wx_fmt=gif&from=appmsg "")  
  
  
1.禁用 RFC5424 攻击面：为 514/1514 的输入显式绑定 pmrfc3164 解析器， 或在规则集入口丢弃带版本号 1 的 RFC5424 报文：  
```
parser(name="p3164" type="pmrfc3164")
input(type="imudp" port="514" ruleset="all" parser="p3164")
```  
  
2.显式路径净化：给 omfile 配置 securepath="normal" 与 secpath-drop="replace"， 不依赖解析器默认行为（上游 rsyslog GHSA-xmp9-244p-5ggv 明确说明 securepath 才是可靠的路径边界）。  
  
3.关闭换行穿透：$EscapeControlCharactersOnReceive on，阻断 MSG 内换行写入。  
  
4.收紧选择器：将 :app-name, startswith, "rsyslog" 改为精确匹配， 并把第 87 行规则改为按来源 IP/网段白名单，避免任意外部发送者进入动态路径模板。  
  
5.网络隔离：514/1514 仅对受管 ESXi 与受信日志转发器开放，禁止从非管理网访问。  
  
   
  
![](https://mmbiz.qpic.cn/mmbiz_gif/a3etiafIAYXFFCoCz3VD5TBvWpsuFjRahHkVo95QJgZxF0h22bkT3jfMcs96ibyfbnDoflhzbIft4KRKvgtLePTiakzzasTfXiatlfpficKdQpbw/640?wx_fmt=gif&from=appmsg "")  
  
交流群  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/a3etiafIAYXHpMXp2PtYxYkImia0meuFhUFD1eEH4Ihib4k9Mp3BPNAFPbd4h7uLxfq3ZkBu5M0763BlC4yFXjbBUonM665p2t4hzYia94UNQ2k/640?wx_fmt=gif&from=appmsg "")  
  
  
 后台回复“  
交流群”，我拉你进网安技术交流群  
  
