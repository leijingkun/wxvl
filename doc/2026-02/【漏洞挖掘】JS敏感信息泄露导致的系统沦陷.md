#  【漏洞挖掘】JS敏感信息泄露导致的系统沦陷  
原创 进击的HACK
                    进击的HACK  进击的HACK   2026-02-03 23:50  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/DuibU3GqmxVmRsdItbBVRKegNHicHQvAHDdZsGpLVU7touSU1AU1twHTfRjG3Vu5aUh0RnPPllfVUhs4qdWF5QYQ/640?wx_fmt=png&wxfrom=13 "")  
  
声明：  
文中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途给予盈利等目的，否则后果自行承担！  
  
如有侵权烦请告知，我会立即删除并致歉。谢谢  
！  
  
文章有疑问的，可以公众号发消息问我，或者留言。我每天都会看的。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/9zYJrD2VibHmqgf4y9Bqh9nDynW5fHvgbgkSGAfRboFPuCGjVoC3qMl6wlFucsx3Y3jt4gibQgZ6LxpoozE0Tdow/640?wx_fmt=png&wxfrom=13 "")  
  
  
  
   
  
> 字数 401，阅读大约需 3 分钟  
  
## 前言  
  
整个漏洞挖掘流程：  
  
FOFA 寻找子域名 ——》打开网站，是管理系统，需登录访问——》拼接 JS 当中的 Create Script，依次枚举 ——》HAE 中发现敏感信息，泄露第三方 token ——》从第三方获取密码，开发密码复用成功登录系统，拿下管理员权限  
  
**流程图：**  
  
![1367ae73af8d8f968e1c327b24b74563.png](https://mmbiz.qpic.cn/sz_mmbiz_png/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0SdHmwE7dovVLHy1FCB9UbXawia5upBdzOYTaCMKGLOlfTpiaIzn2zVMQEg/640?from=appmsg "")  
  
1367ae73af8d8f968e1c327b24b74563.png  
## 测试过程  
  
FOFA 寻找子域名 aaa.bbb.com  
  
可直接登录第三方系统  
  
JS 当中有  
![3c21279e3c3e39cffb14f3d6d0c5f30d.png](https://mmbiz.qpic.cn/sz_mmbiz_png/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0Sd0VXed0BqY1WVXNaXIfhpXGtCyBy7viaaubpNIkngo5W2s1NZ0cIjfkA/640?from=appmsg "")  
  
3c21279e3c3e39cffb14f3d6d0c5f30d.png  
  
是很典型的 Create Script。  
一个字典拼接令一个字典，具体连接哪个靠 key 来判断，将两个 value 拼接成完整路径  
```
xxxx-yyyy-index + . + 387c54 + .js
```  
  
走 intruder，获取所有 JS 信息  
  
然后在 HAE 查看所有信息  
![04280f025291614a261de747561c8b06.png](https://mmbiz.qpic.cn/sz_mmbiz_png/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0Sd0ZaIQCkgzlHN8KHqib3c0xc9vqO4HlkS5SQOyFMqYxrdY5havibPzSbA/640?from=appmsg "")  
  
04280f025291614a261de747561c8b06.png  
  
可以看到这么多的 JS 中有很多值得关注的信息  
![ff27a9b1f8f5890026ab033121b131ef.png](https://mmbiz.qpic.cn/sz_mmbiz_png/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0SdiaJziczqkficzV9mWzkicKkzvIqKkyKOWibr892pibcexabIn6vE0gFOL5ew/640?from=appmsg "")  
  
ff27a9b1f8f5890026ab033121b131ef.png  
  
在 mobile number 中，找到一处  
```
var phone = '187xxxxxxxx'
var urlpath = "https://第三方域名/login?token=xxxxx&phoneNo=" + phone;
```  
  
拼接 phone，访问，是第三方的一个系统，访问后 URL 后自动登录，不需要输入账号密码。  
  
但这是第三方的应用，和当前系统无关，交上去很容易被忽略【都是泪啊~】  
  
在第三方系统里查看信息，查看查看个人信息的 API 中返回了一个字段  
```
password : "32为字符串"
```  
  
一眼 md5 加密，cmd5.com 走起  
  
![71130c913e6e7d5a1322fe8d5b13afd3.png](https://mmbiz.qpic.cn/sz_mmbiz_png/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0Sd51xm0w7icCZ65FrnNsAHsiauwE1pdWeGCDSIn7gpiaic5wDicViaiaoIchTLg/640?from=appmsg "")  
  
71130c913e6e7d5a1322fe8d5b13afd3.png  
  
啧，要付费。  
  
购买获取明文，然后用泄露的手机号和得到的密码，登录 aaa.bbb.com  
  
和我想的一样，开发用的同一个密码，直接进入管理后台。  
## 总结  
  
不放过任何一个 JS 文件。  
  
   
  
  
  
欢迎加入知识星球  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/a1BOUvqnbrgVIlXSDyPdfcgG5ibNES0SdwibhZOwibrGX8AyLCMmGLE4MMeNkKI0dNHUL19ZVrauOuDX1E1xJypKw/640?wx_fmt=jpeg "")  
  
  
  
