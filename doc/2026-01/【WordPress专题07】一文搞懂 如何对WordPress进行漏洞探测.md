#  【WordPress专题07】一文搞懂 如何对WordPress进行漏洞探测  
原创 a1batr0ss  天翁安全   2026-01-14 09:01  
  
**免责声明：**  
本公众号所发布的全部内容，包括但不限于技术文章、POC脚本、漏洞利用工具及相关测试环境，均仅限于合法的网络安全学习、研究和教学用途。所有人在使用上述内容时，应严格遵守中华人民共和国相关法律法规以及道德伦理要求。未经明确的官方书面授权，严禁将公众号内的任何内容用于未经授权的渗透测试、漏洞利用或攻击行为。 所有人仅可在自己合法拥有或管理的系统环境中进行本地漏洞复现与安全测试，或用于具有明确授权的合法渗透测试项目。所有人不得以任何形式利用公众号内提供的内容从事非法、侵权或其他不当活动。 如因违反上述规定或不当使用本公众号提供的任何内容，造成的一切法律责任、经济损失、纠纷及其他任何形式的不利后果，均由相关成员自行承担，与本公众号无任何关联。  
## 不错过最新的漏洞POC  
  
为保证您可以在第一时间接收到本公众号分享的**漏洞复现及POC**  
信息，建议您在公众号“天翁安全”主页界面将“天翁安全”设为**星标**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0NIM3ia9zNibJdMyqL3j8zmWuBdhWSYpItia9XhnYibdVVEpCuBgWtscmzQ/640?wx_fmt=png&from=appmsg "")  
## 写在前面  
  
当你阅读了很多和WordPress相关的漏洞利用、代码审计相关的文章，你是否发出过这样的疑惑：  
> 当我拿到一个WordPress站点时，需要对这个站点做渗透测试或安全评估。可我如何知道它存在哪些插件、可能存在哪些漏洞呢？  
  
  
这就是我们本篇文章要介绍的：**WordPress站点的漏洞探测**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0zfgOL4gxvP1Mv1XYKGd9tv82JvAKsB30BD8hI2LicdiaDZeIEHgb5dSg/640?wx_fmt=png&from=appmsg "")  
## 专题介绍  
  
本文为【WordPress专题】的第七篇文章，意在补充漏洞利用、代码审计前空缺的**漏洞探测**  
环节。  
  
本专题详细介绍对WordPress框架中**公开Or未公开**  
漏洞的**漏洞挖掘**  
、**代码审计分析**  
及**POC构造**  
。以小见大，通过个性的漏洞总结WordPress中漏洞的共性，实现对WordPress漏洞的全面掌握。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0CL2ESXho2aJPmGfiaLURicgGaA5qIgYLKHBMogL3QzfVUNzbU15MO8Cw/640?wx_fmt=png&from=appmsg "")  
  
感兴趣的师傅们可以查看前四篇文章进行共同学习  
- [【WordPress专题01】前台管理员账户创建漏洞（CVE-2025-4334）漏洞分析及POC](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247484874&idx=1&sn=201d4be14b19110dd810479005c241ea&scene=21#wechat_redirect)  
  
  
- [【WordPress专题02】CVE-2025-2563漏洞分析及POC](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247484958&idx=1&sn=9cef3027500d25c935256d0e196b70eb&scene=21#wechat_redirect)  
  
  
- [【WordPress专题03】遇到不按规范编写的插件如何进行代码审计？](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247485055&idx=1&sn=b7ea163ea3178b48f716d6e55941700e&scene=21#wechat_redirect)  
  
  
- [【WordPress专题04】通过漏洞介绍构造CVE-2025-11457的POC](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247485111&idx=1&sn=73c6db35dd7cfb5b2bc6d51a02497de0&scene=21#wechat_redirect)  
  
  
- [【WordPress专题05】CVE-2025-11499前台文件上传漏洞 全网首发POC及漏洞分析](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247485207&idx=1&sn=d3a4f5828ccfbb2f68c39c6ce69f7bcf&scene=21#wechat_redirect)  
  
  
- [【WordPress专题06】CVE-2025-13342 选项修改导致的管理员用户注册漏洞POC及漏洞分析](https://mp.weixin.qq.com/s?__biz=MzkwMzUyMjk2MQ==&mid=2247485312&idx=1&sn=0425719767b7010a004d6ea750a6495b&scene=21#wechat_redirect)  
  
  
## 漏洞探测逻辑  
  
首先，我们来讲一讲WordPress插件漏洞探测的**逻辑**  
。  
  
WordPress本身的漏洞很少，能在实际情况下用到的几乎可以算是没有。实际中可以利用的漏洞一般都来源于WordPress上安装的**插件和主题**  
。当然，几乎没有人使用WordPress不安装插件的，所以几乎或多或少都会有一些插件是有漏洞的，只是还得分析是否有利用价值等。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0VTaCuInkVYsteqcrA4cCBZp3V6ibTBtFnrdsptzf1AN6b4E77ibGS5DA/640?wx_fmt=other&from=appmsg "")  
  
其次，WordPress插件是否有漏洞可以和其插件版本完全挂钩，也就是你只要发现这个插件的版本存在漏洞，几乎可以断定这个漏洞可以利用，不存在有无补丁这一说，因为WordPress插件修复插件基本都是通过发布新版本插件来实现的（当然肯定有些漏洞会有自己的利用条件，比如开启某个参数，调用某个功能）。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0qZD72T9BCAxlMUypn4sV0IpLic0YRPFgCCxSu8uCKiaWNw90nB4LTH6Q/640?wx_fmt=png&from=appmsg "")  
  
而WordPress插件的版本又可以在类似如下的路径中查看到：（更重要的是，这个插件名是安装默认的，按照使用逻辑几乎没有人会去修改）  
```
http://[IP]/wp-content/plugins/[pluginName]/readme.txt
```  
  
如通过下图我们可以确定“Table Field Add-on for ACF and SCF”插件的版本是1.3.30  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia07HgxEQLL41icibO2iakptrZnEAMSYLSHU5XOh8DSYuurtgSvchyA4yfWA/640?wx_fmt=png&from=appmsg "")  
  
有了如上的逻辑，我们很容易能通过脚本的方式来获取到WordPress插件及其对应的版本。  
  
同时，有了插件的版本，我们又要如何能找到**插件版本与漏洞的对应关系**  
呢？其实还真有这样一个专属的漏洞库：Wordfence  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0UswG2FcLdtb5JOjicS5RGwbIz6gRjYiaUaPicbK66gT6PtoibxegR9H93w/640?wx_fmt=png&from=appmsg "")  
  
这是个非常好用的网站，如果你想研究WordPress漏洞一定要熟练使用这个网站。  
  
首先，这个网站可以通过漏洞类型、漏洞严重程度、漏洞公开日期等筛选出你想要研究的漏洞。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0gtZ15W5GaicPBtxicicib0GVyhjjmZSz47sBG4OJIfprtRDibHm4OpQ98VQ/640?wx_fmt=png&from=appmsg "")  
  
其次，我们可以通过插件名称寻找该插件存在的漏洞。  
  
根据之前我们发现的“Table Field Add-on for ACF and SCF”插件的版本是1.3.30，我们可以推测其可能存在CVE-2025-12067漏洞。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0a3ZO2hCec7qAmCeT1u9YY1RdsqM0X36AzIcsD3v2JNHKWoANjiaC0Zw/640?wx_fmt=png&from=appmsg "")  
  
我们点开CVE-2025-12067，可以看到插件漏洞的详细信息。包括插件的名称、包含漏洞的版本、漏洞类型、漏洞介绍等。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0E6AkeHXhBCCxR1VBweIelq5ibQjSZdTk3oe7WXJ8v4ZbT1WkN7IAvnQ/640?wx_fmt=png&from=appmsg "")  
  
根据以上信息，我们就可以实现一个WordPress漏洞探测的逻辑：  
1. 利用 http://[IP]/wp-content/plugins/[pluginName]/readme.txt  
获取到插件的版本  
  
1. 通过获取到插件的版本与Wordfence漏洞库中的版本对比，如果能匹配就证明存在漏洞  
  
## 漏洞探测工具  
  
这里我们对两种市面上有名的WordPress插件扫描工具进行测试：  
1. WPScan（https://github.com/wpscanteam/wpscan）  
  
1. nuclei-wordfence-cve（https://github.com/topscoder/nuclei-wordfence-cve）  
  
### WPScan  
  
**WPScan**  
 是一款专门针对 WordPress 网站的安全扫描工具，它可以检测目标站点的 WordPress 核心版本、插件、主题等组件是否存在已知漏洞，并支持用户/插件枚举、弱密码检测等多种安全审计功能。WPScan 拥有自己的漏洞数据库并持续更新，可帮助安全研究人员或站点管理员发现常见安全问题从而及时修复。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0iatc0jBYhCib8IhF5tqJgBRpTZNibENJqTiaZNcryph4wo3JEiaoV1n5JZg/640?wx_fmt=png&from=appmsg "")  
  
我们这里使用docker版的wpscan。  
  
首先我们使用普通模式进行扫描（这个模式只会扫描非常流行的一批插件）  
```
docker run -it --rm wpscanteam/wpscan --url http://host.docker.internal/ --enumerate p
```  
  
假如你一段时间没有使用，会提示你更新扫描库，输入Y更新即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0ZKnwRsegenkMq5XUfdmp5zNpDtxhopTxCz5ibq8X2icW7iaZW4IJuKdbg/640?wx_fmt=png&from=appmsg "")  
  
这里只能扫到一些基本信息，比如是否开启注册、WordPress的版本、主题名称等  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia01wiccrPI8mgG40VTEicWHXjJVloTCbKPAWwEbtXeQavy7Cu8LHKiaicJVw/640?wx_fmt=png&from=appmsg "")  
  
而对于插件，只是被动扫描一些流行的插件，这里我们的插件就没有被扫到。  
  
于是我们使用Aggressive模式扫描所有的插件，  
```
docker run -it --rm wpscanteam/wpscan --url http://host.docker.internal/ --enumerate ap --plugins-detection aggressive
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0zVGicpdT9ffSA9GdcQdPXsATP88YRGxhv47VjJCZ3dBvEhOmIuM1JgQ/640?wx_fmt=png&from=appmsg "")  
  
可以看到一共扫描了11万多的插件，全部扫描完成花了将近两小时。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0yJxibbvRGZawOaCseCpicRVtuzpcyxBBohIiaUcfUWjGyLicFbQ8Sib1icCQ/640?wx_fmt=png&from=appmsg "")  
  
当然也成功扫到了“Table Field Add-on for ACF and SCF”插件及其版本，给出了发现的位置，并提醒你更新。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0demefawia1n3IKRP1RpQRajSv4hxJv0z1DFxJRDJrR2lhRvOjpxpImA/640?wx_fmt=png&from=appmsg "")  
  
如果配置API Token，还会直接输出该版本存在的漏洞，这里暂时不展示了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0OSTF4GDVz6VG9OE7V4qMg1zA0L2syt9H6IjJeAqOMWuianVfcoUxlpA/640?wx_fmt=png&from=appmsg "")  
### nuclei-wordfence-cve  
  
**nuclei-wordfence-cve**  
 是一个为通用安全扫描器 Nuclei 提供的漏洞模板库，专注于 WordPress 生态中的已知 CVE 漏洞检测。它由大量基于 Wordfence 威胁情报的数据生成的 Nuclei 模板组成，可直接用于扫描 WordPress 核心、插件和主题的安全缺陷，支持按严重性、标签等过滤扫描结果，从而实现快速自动化的漏洞识别。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0j4SNv5upEdJ5YKAO5GFYHX9R74dvAK7QBuhFNYAy4buTU20vmACkhg/640?wx_fmt=png&from=appmsg "")  
  
这个工具是利用了nuclei为主体，能直接扫描出具体漏洞。  
```
#更新插件
export GITHUB_TEMPLATE_REPO=topscoder/nuclei-wordfence-cve 
nuclei -update-templates
#扫描
nuclei -t github/topscoder/nuclei-wordfence-cve -u http://127.0.0.1/ -stats
```  
  
一段时间后也是发现了我们的目标漏洞CVE-2025-12067  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0NtSfHook7WO5NZ5bF6kA5Gerhy8nMgaNvgCDswy7KKWELQLcnYTwmQ/640?wx_fmt=png&from=appmsg "")  
  
最终全部扫描完花了差不多13分钟  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0IkTVo80IAvD2DcnD3FicnIKnD9zsOXML9MSZ6ORiaibQwI5l9SznW1ouw/640?wx_fmt=png&from=appmsg "")  
### 工具总结  
  
很明显的，nuclei-wordfence-cve的效率更高，而且能直接扫描出具体的漏洞。而WPScan能扫描出一些WordPress的基本信息。  
  
因此，当我们想对一个WordPress站点做安全评估时，可以先使用WPScan的普通扫描获取一些站点的**基本信息**  
```
docker run -it --rm wpscanteam/wpscan --url http://host.docker.internal/ --enumerate p
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia01wiccrPI8mgG40VTEicWHXjJVloTCbKPAWwEbtXeQavy7Cu8LHKiaicJVw/640?wx_fmt=png&from=appmsg "")  
  
接着再使用nuclei-wordfence-cve做一个**全面的漏洞扫描**  
评估  
```
nuclei -t github/topscoder/nuclei-wordfence-cve -u http://127.0.0.1/ -stats
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0oBFrTqia0RWpjs9CxG41uAPMEkSVJpQ5j7zia9783n3Bls04O3ZxSV3g/640?wx_fmt=png&from=appmsg "")  
## 漏洞利用/代码审计  
  
当我们通过漏洞探测发现了这个站点存在某些漏洞时，就可以对其进行代码审计  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0jKBySOCkPiafFM8rWgnSg201ZogoqDiaLAWSbutCAuYFM2RDNxC2Dyjg/640?wx_fmt=png&from=appmsg "")  
  
以及漏洞利用  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0VsyVy69xgEU6iaDZ2yM5r1Y9CscoZZKFnlEOh7vicG6FwibGfwgEcCneg/640?wx_fmt=png&from=appmsg "")  
  
具体如何对WordPress进行**漏洞挖掘**  
、**代码审计分析**  
及**POC构造**  
，可移步至[WordPress专题](https://mp.weixin.qq.com/mp/appmsgalbum?__biz=MzkwMzUyMjk2MQ==&action=getalbum&album_id=4237007721708765198&from_itemidx=1&from_msgid=2247485312&subscene=159&subscene=&scenenote=https%3A%2F%2Fmp.weixin.qq.com%2Fs%2FsWGBmck0lVyRoiWJfw6Mzw&nolastread=1#wechat_redirect)  
。  
## 知识星球  
  
相关内容已同步更新至知识星球  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0q9HfMRnSkzyBia4eQXBNZ39so49nibrAh4vlOY9lQvJ4ksdpfhqCVlhw/640?wx_fmt=png&from=appmsg "")  
> 星球加入方式见文章底部二维码，欢迎加入交流和学习  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/I2eHcAFia5S5S2uF5JGDWUSl36RhdDnia0csZjN9cMACYU1qeo07cYAJp1gNYTtqGmXCLDA80EiayichWxoLPSu1cA/640?wx_fmt=png&from=appmsg "")  
  
  
