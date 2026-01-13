#  SRC漏洞挖掘3  
 StudySec   2026-01-13 08:27  
  
   
  
# SRC 学习笔记  
  
![](https://mmbiz.qpic.cn/mmbiz_png/POcjicJBp8OIdIGrablWFADVpUibAf2diakvbsBmibAGxuGAmj8G0z3MO1e6ZyeD1Riblu1PcJkj3wBSFUjGibVcT7RA/640?wx_fmt=png&from=appmsg "null")  
  
## 一、信息收集核心体系  
### （一）信息收集总模板（核心框架）  
  
集团 → 子公司 → 小程序 / APP → 主域名 → ICP 备案 → 子域名 → 域名 → 图标（icon）  
  
注：子公司信息可通过微信、小蓝本、零零信安、ICP 官方平台补充验证  
### （二）集团与子公司信息收集要点  
1. 1. **资产归属规则**  
：部分 SRC 平台仅收录 “持股比例≥50%” 的子公司资产（示例：360 集团分为生活集团、科技集团两大主体）。  
  
1. 2. **操作要求**  
：需严格遵循上述总模板收集，避免资产归属误判。  
  
### （三）测试优先级排序（按重要性 / 价值）  
1. 1. 优先级：APP > 小程序 > Web（APP 测试另有专项课程）  
  
1. 2. 小程序特殊操作：需搭建独立测试环境；部分小程序资产可通过抓包获取域名，直接在 Web 端访问测试。  
  
### （四）关键信息查询方法  
1. 1. **核心关联逻辑**  
：主域名决定子域名范围，ICP 备案信息用于界定 “目标资产归属”，需结合备案信息验证子域名 / 自域名归属。  
  
1. 2. **小程序查询渠道**  
：支持微信、支付宝、抖音平台搜索  
  
- • 微信端：可通过 “模糊关键词匹配 + 备案信息验证” 确认资产归属。  
  
### （五）信息收集特性  
1. 1. **持续性**  
：非一次性任务，挖掘遇瓶颈时需重新补充收集以突破。  
  
1. 2. **准确性提升**  
：可通过 “多语句拼接” 优化查询条件，减少无效资产干扰。  
  
## 二、常用工具使用指南（按功能分类）  
### （一）资产筛选类工具  
#### 1. Firefly  
- • **核心功能**  
：SRC 资产筛选与导出  
  
- • **操作流程**  
：选择目标 SRC 平台 → 进入 “网站 / 子域名列表” → 添加状态码筛选（支持 200、302、403、404 等） → 导出资产列表  
  
- • **适用场景**  
：批量筛选有效 SRC 资产  
  
#### 2. 域名挖掘机（4.2 版本）  
- • **核心功能**  
：域名探测  
  
- • **操作技巧**  
：设置状态码筛选（如仅保留 200 状态码），提升筛选效率  
  
- • **适用场景**  
：初步域名资产探测  
  
#### 3. OneForAll  
- • **核心功能**  
：自动化资产探测与表格导出  
  
- • **操作方式**  
：命令行执行脚本，工具自动导出资产表格  
  
- • **适用场景**  
：批量探测并整理资产信息  
  
### （二）企业 / 子公司信息类工具  
#### 1. enscan  
- • **核心功能**  
：子公司信息收集、域名备案查询、企业基础信息（招聘、投资）查询  
  
- • **关键特性**  
：支持筛选 “持股≥50%” 的子公司，精准匹配 SRC 资产要求  
  
- • **适用场景**  
：集团型企业子公司资产梳理  
  
#### 2. 零零信安  
- • **核心功能**  
：企业资产自动识别（备案信息、子公司、移动端应用）  
  
- • **操作方式**  
：输入企业全称（如 “北京百度网讯科技公司”），工具自动输出关联资产  
  
- • **适用场景**  
：快速获取企业全维度资产  
  
### （三）小程序 / 图标识别类工具  
#### 1. 小蓝本  
- • **核心功能**  
：小程序资产收集  
  
- • **搜索维度**  
：支持按集团、子公司、法定代表人、域名关键词搜索  
  
- • **注意事项**  
：需配合其他工具（如零零信安）验证小程序资产归属  
  
- • **适用场景**  
：小程序专项资产挖掘  
  
#### 2. Hunter（含 Fofa 类似功能）  
- • **核心功能**  
：图标识别、Web 信息查询、备案筛选、时间 / 状态码过滤  
  
- • **常用查询语法**  
：  
  
- • 域名筛选：domain=""  
 / host=""  
  
- • 图标识别：ico  
（需上传目标 ico 文件）  
  
- • 应用筛选：app="RabbitMQ"  
/app="Microsoft-Exchange"  
/app="泛微-EMobile"  
/app="用友-ERP-NC"  
/app="Struts2"  
/app="用友-NC-Cloud"  
  
- • 备案筛选：icp="京ICP证030173号"  
  
- • **推荐用法**  
：sota 自域名查询，精准定位目标资产  
  
- • **适用场景**  
：多维度资产精准检索、图标关联资产挖掘  
  
## 三、漏洞挖掘实战流程  
### （一）前期搜索技巧（漏洞线索定位）  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">挖掘目标</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">搜索语法示例</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">文件上传漏洞</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">site:edu.cn inurl:load</span></code></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">安服水洞（robots.txt）</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">site:edu.cn inurl:robots.txt</span></code></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">系统登录 / 后台</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">`</span><span style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;color: rgb(87, 107, 149);"><span leaf="">site:edu.cn</span></span><span leaf=""> intitle: 登录</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">边缘资产</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">` 浙江大学 -</span><span style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;color: rgb(87, 107, 149);"><span leaf="">edu.cn</span></span><span leaf=""> 登录</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">编辑器漏洞</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-size: 14px;padding: 0.25em 0.5em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">site:edu.cn intitle:留言</span></code><section><span leaf=""> / </span><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: -apple-system-font, BlinkMacSystemFont, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">site:edu.cn inurl:ewebeditor</span></code></section></td></tr></tbody></table>  
- • **编辑器漏洞补充**  
：  
  
1. 1. 常见编辑器类型：ewebeditor、ewebeditornet、fckeditor、editor、southideditor、SouthidcEditor、bigcneditor  
  
1. 2. ewebeditor 默认登录页：  
htt  
ps://  
www.w  
ebshe  
ll.cc  
/eweb  
edito  
r/adm  
in_lo  
gin.a  
sp  
  
1. 3. 参考资料：  
h  
ttps:  
//blo  
g.csd  
n.net  
/2301  
_7773  
2591/  
artic  
le/de  
tails  
/1312  
87672  
  
- • **关键提示**  
：优先挖掘 “历史漏洞”（已验证漏洞点复现 / 变种测试），提升成功率。  
  
### （二）具体漏洞测试流程  
#### 1. SQL 注入测试  
- • **步骤 1：寻找注入点**  
通过特殊字符 / 函数判断：单引号（'）、双引号（"）、加减乘除运算、sleep () 函数（观察响应延迟）。  
  
- • **步骤 2：构造信息提取 “轮子”**  
使用函数组合：length()  
（判断长度）、right()  
/left()  
（截取字符）、database()  
（获取数据库名）、user()  
（获取当前用户）。  
  
- • **步骤 3：爆破数据**  
确认注入点后，用 sqlmap 等工具自动化提取数据库数据。  
  
#### 2. XSS 攻击测试  
- • **步骤 1：测试解析能力**  
插入无害 HTML 标签（如</h1>  
），判断页面是否解析标签（解析则存在 XSS 可能）。  
  
- • **步骤 2：Payload 攻击与绕过**  
解析成功后，使用 XSS Payload（如 `alert (1)）尝试绕过过滤机制（如关键词替换、长度限制）。  
  
#### 3. 文件上传漏洞测试  
- • **核心定位**  
：常见高风险漏洞，需重点关注。  
  
- • **辅助技巧**  
：查看robots.txt  
文件，可能暴露内部未公开资产（如上传接口路径）。  
  
#### 4. 编辑器漏洞测试（以 FCKEditor 为例）  
- • **核心特性**  
：历史漏洞数量多，易成为突破点。  
  
- • **操作要点**  
：掌握对应编辑器的 “漏洞利用方法 + 过滤绕过技巧”（参考上述 “前期搜索技巧” 中编辑器相关资料）。  
  
#### 5. 边缘资产挖掘  
- • **启动时机**  
：当证书网站等核心资产挖掘殆尽时。  
  
- • **目标**  
：扩大漏洞挖掘范围，覆盖非核心但可能存在漏洞的资产（如子公司边缘系统、旧版应用）。  
  
  
  
   
  
