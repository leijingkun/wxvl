#  Burpsuite插件：autorize实现抓包实时越权漏洞检测  
网安武器库  网安武器库   2026-01-11 16:06  
  
**更多干货  点击蓝字 关注我们**  
  
  
  
**注：本文仅供学习，坚决反对一切危害网络安全的行为。造成法律后果自行负责！**  
  
**往期回顾**  
  
  
  
  
  
  
·[Tor浏览器：匿名浏览器实现网络IP隐身](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485793&idx=1&sn=fde90afa005a931568a23c5d36e29882&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[本地电脑没有PIN密码也可以实现文件窃取? Windows11WinRE权限控制绕过漏洞演示](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485773&idx=1&sn=b923c1ccf9852c8dab35b514a9e93c7e&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Apt_t00ls：Java 开发的OA设备高危漏洞集成利用工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485760&idx=1&sn=8ff7bd1519db2edf019ed5125997e6e6&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Hashcat密码工具：快速识别百种哈希密码类型+GPU加速密码爆破](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485727&idx=1&sn=da20b5443572fb7e1550eb1e42b28dbc&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[Nikto：一款Web安全漏洞扫描器](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485724&idx=1&sn=8daf51f5fb3946907463096de6309731&scene=21#wechat_redirect)  
  
  
  
  
  
  
·[HTTrack爬虫：网站递归式资源抓取工具](https://mp.weixin.qq.com/s?__biz=MzYzNTExNDYwMg==&mid=2247485715&idx=1&sn=819198a15960118702253dffd868f127&scene=21#wechat_redirect)  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
autorize介绍  
  
  
  
插件概述  
  
    Autorize是BurpSuite 的一款专门用于检测越权漏洞的扩展插件，能够自动化测试 Web 应用中的水平越权和垂直越权问题。越权漏洞是 Web 应用中常见且危害较大的安全问题，Autorize通过模拟不同权限用户的访问行为，快速识别权限控制缺陷。  
  
  
安装方法  
  
  从 BAppStore 安装  
```
1. 打开 Burp Suite
2. 导航到 Extender → BApp Store
3. 在搜索框中输入 "Autorize"
4. 点击 "Install" 按钮安装插件
```  
  
![在这里插入图片描述](https://mmbiz.qpic.cn/sz_mmbiz_jpg/3ibCZqSDX9ugfU1ibeko7SO4KmAM6wEJ1E7kpqSlmR9WW4zfUvrnFR5FLWXAq8bAQZZicPibFYBia9qEoS5Ioln6gZg/640?wx_fmt=jpeg "")  
  
 手动安装  
```
1. 从 GitHub 或其他可靠来源下载 Autorize 插件的 JAR 文件
2. 打开 Burp Suite
3. 导航到 Extender → Extensions
4. 点击 "Add" 按钮
5. 选择下载的 JAR 文件并点击 "Next" 完成安装
```  
  
  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eFcsfYatVNptgRDr3kqeFwpGYKFziaX9s7BBcG8prEJFW1g1EickibFyug/640?wx_fmt=png&from=appmsg "")  
  
autorize  
配置与使用  
  
  
  
基本配置  
  
1. 安装完成后，在 Burp Suite 中会出现 "Autorize" 标签页  
  
2. 配置以下核心参数：  
```
Proxy Mode ：选择代理模式，通常使用 "Intercept requests based on rules"
Authorization Header ：设置授权头，通常为 "Authorization"
Unauthenticated Response ：设置未认证响应的特征，用于识别未授权访问
Forbidden Response ：设置禁止访问响应的特征，用于识别权限不足
```  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/3ibCZqSDX9ugfU1ibeko7SO4KmAM6wEJ1E0Vc9GqTuCAEGiaencAlTweSp9a91nf1IaBJkcibkJayfKxiaFyTDX0S7A/640?wx_fmt=jpeg "")  
  
  
检测流程  
  
1. 登录多个账号 ：  
```
 使用不同权限的账号登录目标应用（如管理员账号和普通用户账号）
 分别获取这些账号的认证凭证（如 Cookie、Token 等）
```  
  
2. 配置认证状态 ：  
```
 在 Autorize 标签页中，设置 "Authorized Requester" 为高权限用户的凭证
 设置 "Unauthorized Requester" 为低权限用户或匿名用户的凭证
```  
  
3. 开始检测 ：  
```
 启用 Autorize 插件（点击 "Intercept is on"）
 正常浏览目标应用，执行各种操作
 Autorize 会自动拦截请求并使用不同权限的凭证重新发送
 分析响应差异，识别越权漏洞
```  
  
**例如：**  
  
**登录低权限用户获取cookie**  
  
****  
**把cookie值复制到Autorize插件中并保存，随意命名一下。**  
  
****  
**开启捕获功能，点击一下**  
  
****  
通过点击进行不同的操作来发送流量，  
登录管理员用户并浏览：  
  
****  
****  
检测原理  
  
Autorize 的核心检测原理是 对比不同权限用户对同一资源的访问结果 ：  
```
1. 请求拦截 ：拦截通过 Burp Proxy 的 HTTP 请求
2. 凭证替换 ：使用配置的不同权限凭证替换原始请求中的认证信息
3. 响应对比 ：对比不同权限用户访问同一资源的响应
4. 漏洞识别 ：根据响应差异判断是否存在越权漏洞
   - 水平越权 ：相同权限级别但不同用户间的资源访问
   - 垂直越权 ：低权限用户访问高权限资源
```  
  
  
结果分析  
  
Autorize 使用颜色编码直观显示检测结果：  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/3ibCZqSDX9ugfU1ibeko7SO4KmAM6wEJ1Ef5Wof1tOic3SI4gW2h3ETN0SCwfvdBOTOvpicwNmUw7E8vESBtzqavzg/640?wx_fmt=jpeg "")  
```
绿色 ：请求正常，无越权问题
黄色 ：存在潜在越权问题，需要进一步验证
红色 ：确认存在越权漏洞
蓝色 ：请求被过滤或未处理
```  
  
  
详细结果查看  
```
点击具体请求可查看详细的请求/响应对比
可导出检测结果为 CSV 或其他格式，便于后续分析
```  
  
  
  
参考：  
https://developer.aliyun.com/article/1326542  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/3ibCZqSDX9ugSGKJibovaia9YxcaLfMJib6eRUtCzBCFbaMYy1c7utlweibCFXWsicmm9ebyvInBtdsD0QRlUDTdLib1g/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
