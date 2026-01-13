#  Nginx CRLF注入漏洞解析  
原创 晨星安全团队  晨星安全团队   2026-01-13 08:29  
  
# Nginx CRLF 注入  
## 漏洞基础信息  
### 核心定义  
  
CRLF 是”回车 + 换行”（\r\n）的简称  
  
CRLF Injection 又叫 HTTP Response Splitting，简称 HRS  
### HTTP 协议相关规则  
  
HTTP 协议中，HTTP Header 与 HTTP Body 是用两个 CRLF 分隔的，浏览器就是根据这两个 CRLF 来取出HTTP 内容并显示出来  
  
所以，一旦我们能够控制 HTTP 消息头中的字符，注入一些恶意的换行，这样我们就能注入一些会话 Cookie 或 者 HTML代码  
## Nginx 漏洞成因  
### Nginx 关键特性  
  
Nginx 会将 $uri  
 进行解码，导致传入 %0d%0a  
 即可引入换行符，造成 CRLF 注入漏洞  
### 错误配置文件示例  
  
原本的目的是为了让 HTTP 的请求跳转到 HTTPS 上  
```
// / 表示匹配所有请求路径// $host 是客户端请求中的主机名（例如 example.com）// $uri 是请求的 URI 路径部分，不含查询字符串（例如 /index.html）location / { return 302 https://$host$uri;}
```  
## 漏洞利用方式  
### 注入恶意 Cookie  
  
利用这个漏洞可注入 Set-Cookie 头  
```
http://your-ip:8080/%0d%0aSet-Cookie:%20a=1
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckxmdiaSKxHKT82FhyV7E69bJgYTTcX6PibIJgiaPych2X2589QkwyGYTUw/640?wx_fmt=png&from=appmsg "")  
### 反射型 XSS 攻击  
  
当然，HRS 并不仅限于会话固定，通过注入两个 CRLF 就能造成一个无视浏览器 Filter 的反射型 XSS  
  
比如一个网站接受 URL 参数http://test.sina.com.cn/?url=xxx  
  
xxx 放在 Location 后面作为一个跳转，如果我们输入的是  
```
http://test.sina.com.cn/?url=%0d%0a%0d%0a<img src=1 onerror=alert(/xss/)>
```  
  
我们的返回包就会变成这样  
```
HTTP/1.1 302 Moved Temporarily Date: Fri, 27 Jun 2014 17:52:17 GMT Content-Type: text/html Content-Length: 154 Connection: close Location:<img src=1 onerror=alert(/xss/)>
```  
  
之前说了浏览器会根据第一个 CRLF 把 HTTP 包分成头和体，然后将体显示出来  
  
于是我们这里<img>  
这个标签就会显示出来，造成一个 XSS  
  
新浪某分站含有一个 URL 跳转漏洞，危害并不大，于是我就想到了CRLF Injection，当我测试  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckhsSlLUKhQjw6ibUkypWAPbBgytFDx86PkwFZiaMcZzN5iakU8bqRhJiaKg/640?wx_fmt=png&from=appmsg "")  
XSS 时看控制台，果然被 XSS Filter 拦截了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckIWbdKvbzuicMvhZKjBguXicSGU8KcHZsVzrpdvjTZf8xj2HN9bnOZkJw/640?wx_fmt=png&from=appmsg "")  
  
那么我们就注入一个 X-XSS-Protection:0 到数据包中，看看什么效果  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckgrQfIXslaU7lVbQ8km5JGGPeYVK2doxgYJq5icz0HB4ib78ymN9pzK5w/640?wx_fmt=png&from=appmsg "")  
  
还想到了一个利用字符编码来绕过 XSS Filter 的方法，当编码是 is-2022-kr 时浏览器会忽略%0f  
，这样我们在onerror  
后面加个%0f  
就能绕过 filter，前提是注入一个<meta charset=ISO-2022-KR>  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckz4ItMbU4apTvwmTDS0LysUmLyibfuLW9KkR8H71UI5HVNVh9Qn3seUQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tw9nMcspHZnjrXZq5hEmIN0QI5D2VAckZM7icxgyEEIQicZo4YePek6CDToc7MZvl3QGL9dYtbI39LPJ5N7sCJ9w/640?wx_fmt=png&from=appmsg "")  
  
****  
**-END-**  
  
