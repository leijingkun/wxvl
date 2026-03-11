#  Craft CMS 高危漏洞：黑名单修了四年，最终还是失守  
原创 CVE-SEC
                    CVE-SEC  CVE-SEC   2026-03-11 00:00  
  
# Craft CMS 高危漏洞：黑名单修了四年，最终还是失守  
## 开头  
  
一个内容管理系统，从 2024 年到 2026 年，同一个位置被打了四次，每次修完下次还能绕。直到第四次，官方才彻底换掉了底层机制。  
  
这就是 CVE-2026-28783 的故事，也是安全工程中一个关于"打补丁"与"修根因"之间选择的典型案例。  
## 漏洞是什么  
  
CVE-2026-28783 是一枚存在于 Craft CMS 中的服务端模板注入漏洞（SSTI），CVSS v4.0 评分 9.4，等级为 Critical。  
  
Craft CMS 是一款基于 PHP 的内容管理系统，底层使用 Twig 模板引擎。全球超过 15 万个网站运行在这套系统上。  
  
漏洞的核心问题是：Craft CMS 允许管理员在后台编写包含 Twig 语法的模板内容，而系统通过一份黑名单来阻止危险的 PHP 函数被调用。这份黑名单，不够完整。  
## 为什么黑名单会失守  
  
Craft CMS 在代码里维护了一个方法 checkArrowFunction()  
，每当 Twig 过滤器收到可调用参数时，这个方法就会拦截检查，对比传入的函数名是否在禁用列表里。  
  
禁用列表大概长这样：system  
、passthru  
、exec  
、file_get_contents  
、file_put_contents  
、popen  
。  
  
听起来很全，但 PHP 标准库里能执行命令的函数远不止这几个，而且 Twig 本身提供了多条不经过这个检查的执行路径。  
  
**绕过方式一：call_user_func 不在名单里**  
```
{{ ['system', 'id'] | sort('call_user_func') }}

```  
  
这条 payload 把 system  
 和 id  
 作为两个排序比较项传给 sort  
 过滤器，PHP 实际执行的是 call_user_func('system', 'id')  
，等价于 system('id')  
 直接拿到命令执行结果，而 call_user_func  
 本身没被列入黑名单。  
  
**绕过方式二：Twig 运算符完全绕过检查**  
```
{{'id' has some 'system'}}
{{'cat /etc/passwd'|find('system')}}

```  
  
has some  
、has every  
、find  
 这类 Twig 运算符走的是不同的代码路径，根本不会触发 checkArrowFunction()  
，黑名单对这些路径无效。  
  
**绕过方式三：通过 create() 函数实例化 Symfony Process（CVE-2026-28695）**  
```
{% set p = create("Symfony\\Component\\Process\\Process", [["id"]]) %}
{{ p.mustRun.getOutput }}

```  
  
Craft CMS 捆绑了 symfony/process  
 组件，而 Twig 全局函数 create()  
 可以实例化任意 PHP 类，不需要传递任何函数名，黑名单完全无从拦截。  
  
三种绕过方式，三条独立的攻击路径，指向的都是同一份不完整的黑名单。  
## 四年，四次补丁  
  
这不是第一次出问题，也不是第二次。  
<table><thead><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">时间</span></section></th><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">CVE</span></section></th><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">问题</span></section></th><th style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;font-weight: bold;background-color: #f0f0f0;"><section><span leaf="">修复方式</span></section></th></tr></thead><tbody><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">2024 年 11 月</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">CVE-2024-52293</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">Twig </span><code><span leaf="">has some</span></code><span leaf=""> 运算符绕过 + 路径遍历组合 RCE</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">补充路径检查</span></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: #F8F8F8;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">2025 年 4 月</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">CVE-2025-46731</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><code><span leaf="">popen</span></code><section><span leaf=""> 未在黑名单里</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">追加 </span><code><span leaf="">popen</span></code><span leaf=""> 到列表</span></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: white;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">2025 年 8 月</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">CVE-2025-57811</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">前次修复仍可绕过</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">再次扩充黑名单</span></section></td></tr><tr style="border: 0;border-top: 1px solid #ccc;background-color: #F8F8F8;"><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">2026 年 3 月</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">CVE-2026-28783</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">黑名单仍不完整</span></section></td><td style="font-size: 16px;border: 1px solid #ccc;padding: 5px 10px;text-align: left;"><section><span leaf="">彻底废弃黑名单，引入 Twig 沙箱</span></section></td></tr></tbody></table>  
每一次修复，都是在已知绕过方式上追加条目；每一次追加，都留下了下一次绕过的空间。直到第四次，官方才认定这条路走不通，换掉了整个底层机制。  
  
值得一提的是，2025 年 4 月披露的 CVE-2025-32432 是同期另一枚 Craft CMS 漏洞，CVSS 10.0，不需要认证就能触发 RCE。该漏洞公告发布时，安全机构 SensePost 的调查发现约 300 台服务器在公告发布前已遭入侵。这说明针对 Craft CMS 的真实攻击活动一直存在，对已知漏洞修复的响应速度直接关系到实际风险。  
## 攻击者需要什么  
  
利用 CVE-2026-28783 有前提条件：  
- 持有目标站点的管理员账户，或  
  
- 目标服务器在生产环境中开启了 allowAdminChanges=true  
 配置，或  
  
- 具备 System Messages 功能的访问权限  
  
这不是一个无需认证的漏洞，但管理员账户被盗的情况在实际攻击中并不罕见，而 allowAdminChanges=true  
 在生产环境中的误用也普遍存在。  
  
一旦条件满足，攻击者登录后台，在系统消息模板、条目类型标题格式或邮件模板等可编辑 Twig 内容的位置注入 payload，触发渲染，即可在服务器上执行任意命令。  
  
后续可以做的事包括：读取 .env  
 文件获取数据库密码和 API 密钥，写入 WebShell 建立持久访问，对内网发起 SSRF 探测。  
## 官方是怎么修的  
  
官方在 2026 年 3 月 2 日随漏洞公告一并发布了修复版本 5.9.0-beta.1 和 4.17.0-beta.1，稳定版为 5.9.0 和 4.17.0。  
  
这次修复的核心是放弃黑名单，引入 Twig 沙箱机制。  
  
新增配置项 enableTwigSandbox  
：启用后，所有传给过滤器的可调用参数必须是 Twig 内部闭包（n => expression  
 形式），PHP 函数名字符串不再被允许传递。不靠列表，靠架构。  
  
沙箱的白名单策略通过 craft\web\twig\SecurityPolicy  
 实现，允许使用的标签、过滤器、函数均有明确列表，create()  
 函数和 craft  
 全局对象不在其中。  
  
同时，无论沙箱是否启用，create()  
 函数现在全局限制只能实例化继承自 yii\base\BaseObject  
 的类，直接关闭了 Symfony Process 这类外部类的利用路径。  
  
需要注意的是：enableTwigSandbox  
 对已有安装默认值为 false  
，升级后需要管理员手动在 config/general.php  
 中启用：  
```
'enableTwigSandbox' => true,

```  
  
或通过环境变量：  
```
CRAFT_ENABLE_TWIG_SANDBOX=true

```  
  
不手动开这个开关，沙箱不生效。  
## 如果你在用 Craft CMS  
  
需要做的事情按优先级排列：  
  
第一，升级。5.x 升到 5.9.0，4.x 升到 4.17.0，这是唯一根本解法。  
  
第二，升级后手动启用 Twig 沙箱，新项目默认已开启，存量项目需要手动配置。  
  
第三，生产环境确认 allowAdminChanges  
 设为 false  
，不要让管理员在生产上随意修改配置与模板。  
  
第四，为后台管理面板开启多因素认证，并限制后台路径仅允许受信任 IP 访问。  
  
如果短期内无法升级，至少先把 allowAdminChanges  
 关掉，并对后台访问来源做 IP 限制，这是能把攻击门槛抬高的临时措施。  
## 一点延伸思考  
  
这个漏洞的修复历程值得关注的不只是技术细节，而是它揭示的一种工程模式：用黑名单对抗语义丰富的系统，通常是持续的消耗战。  
  
PHP 可用于命令执行的函数种类庞大，PHP 版本迭代也会引入新的调用方式；Twig 模板引擎本身不断演进，每个新语法特性都可能提供新的执行路径。在这种对抗中，黑名单维护者永远处于被动响应的位置，而攻击者只需要找到一个遗漏。  
  
CVE-2024-52293 到 CVE-2026-28783，四轮补丁，最终用了两年时间走到同一个结论：换架构。  
  
这个结论本身，在 Twig 框架于 2021 年修复 CVE-2022-23614（排序过滤器非闭包绕过）时就已经有了先例，当时的建议也是启用沙箱。  
  
早一点做架构层面的决策，代价往往更低。  
## 参考资料  
- CVE-2026-28783 GitHub Security Advisory (GHSA-5fvc-7894-ghp4)：https://github.com/advisories/GHSA-5fvc-7894-ghp4  
  
- NVD CVE-2026-28783：https://nvd.nist.gov/vuln/detail/CVE-2026-28783  
  
- Craft CMS enableTwigSandbox 官方文档：https://craftcms.com/docs/5.x/reference/config/general.html[#enabletwigsandbox]()  
  
  
- Craft CMS 安全知识库：https://craftcms.com/knowledge-base/security  
  
- Assetnote 研究：PHP footgun 导致 Craft CMS RCE（CVE-2024-56145）：https://www.assetnote.io/resources/research/how-an-obscure-php-footgun-led-to-rce-in-craft-cms  
  
- SensePost：CVE-2025-32432 野外攻击活动分析：https://sensepost.com/blog/2025/investigating-an-in-the-wild-campaign-using-rce-in-craftcms/  
  
披露时间：2026-03-02　CVE：CVE-2026-28783　CVSS v4.0：9.4 Critical  
  
  
