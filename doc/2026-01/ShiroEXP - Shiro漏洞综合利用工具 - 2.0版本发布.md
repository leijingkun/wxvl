#  ShiroEXP - Shiro漏洞综合利用工具 - 2.0版本发布  
原创 Y5neKO  Y5Sec   2026-01-07 05:30  
  
   
  
# ShiroEXP  
  
这个项目出于某些BUG原因，再加上我太懒了（），1.0版本就没有放出来，最近终于有时间完善了，中间又迭代了好几个小版本，这次直接发布2.0版本；原来命令行版本参数太多了，比较难用，改成了GUI，又整合了一堆Shiro的利用功能，大家使用过程中发现的Bug欢迎随时向我反馈。  
  
FindClassByURLDNS、FindClassByBomb实现思路来自@c0ny1  
师傅；  
Shiro721实现思路来自@feihong-cs  
师傅；  
  
再次感谢Thanks列表里各位大佬的优秀项目提供的思路🙏  
## 开发环境  
  
JDK 8u431  
 | Intellij 2025  
> ⚠️ 使用其他JDK版本可能出现未知的错误  
  
## 功能  
- • 爆破rememberMe  
  
- • 漏洞探测(Shiro550、Shiro721)  
  
- • 探测回显链  
  
- • 探测依赖（FindClassByURLDNS、FindClassByBomb）  
  
- • 命令执行  
  
- • 注入内存马  
  
- • 全局代理  
  
## 帮助  
### Shiro550  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJF5X4XeXtmwe5lTvAKAutAicGcadAiatIJLNHFntlyztTV1cc6dxRmwcw/640?wx_fmt=png&from=appmsg "")  
  
### Shiro721  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJMOI7yPgEC1GQGyeNLia22D0MElllzHErUnddBPVX99yLHGiaIhq1oIeQ/640?wx_fmt=png&from=appmsg "")  
  
### FindClassByURLDNS  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJAbia8A4BvPJnxocbhylICttkcyLibn7ibcUShAELkfEfbMbJ6cOxOOsWg/640?wx_fmt=png&from=appmsg "")  
  
自定义探测类  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJpBnbWdIeJJUmfaxGbCQrFtn5mO2o8icO73NcB5KLo2H7fDCMx6aj6kw/640?wx_fmt=png&from=appmsg "")  
  
### FindClassByBomb  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJZZTicynHqXicZOaxQE8phMiaua4LvtS5J6Co0FjnLlPqmibvDub9pDefIw/640?wx_fmt=png&from=appmsg "")  
  
### 命令执行  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJH1pYbbLwyNUziaeamVcuEa2x0EyHR4ebLiaTdN8RGQ9vseoBFoxiaXHMQ/640?wx_fmt=png&from=appmsg "")  
  
### 内存马注入  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJYbyyQ2qI1HCXwojC896OVULhFfAe81ibD6IF1CnpqgVoC0kwDlia9SGQ/640?wx_fmt=png&from=appmsg "")  
  
自定义内存马  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJ8bgj5Xgr2myPtxL4u8DWJHdtafSUrzSlic49KFtTbiaib6CgmpfAyIbtQ/640?wx_fmt=png&from=appmsg "")  
  
## 构建  
  
自行编译打jar包时，可能会遇到**找不到或无法加载主类**  
的问题，经排查是因为bcprov包中的签名文件，打包后删除*.DSA、*.SF文件即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/GcusaH7xzy8GyKHXFEjgI7T4fzk81ibRJ0hVDuOy5xl0icE0GSka5a9qbRE8B48ME3Q8uNfIVQIgrWxKWL2uDoew/640?wx_fmt=png&from=appmsg "")  
  
## 感谢  
  
@frohoff   https://github.com/frohoff/ysoserial  
  
@SummerSec  https://github.com/SummerSec/ShiroAttack2  
  
@c0ny1 https://github.com/woodpecker-framework/ysoserial-for-woodpecker  
  
@feihong-cs https://github.com/feihong-cs/ShiroExploit-Deprecated  
## 项目地址  
  
**https://github.com/Y5neKO/ShiroEXP**  
  
   
  
  
