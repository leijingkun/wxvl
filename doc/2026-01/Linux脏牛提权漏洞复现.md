#  Linux脏牛提权漏洞复现  
dmd5安全  dmd5安全   2026-01-09 02:07  
  
# 简述  
  
脏牛（DirtyCow）是Linux中的一个提权漏洞。主要产生的原因是Linux系统的内核中Copy-on-Write（COW）机制产生的竞争条件问题导致，攻击者可以破坏私有只读内存映射，并提升为本地管理员权限。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETic8icGeYy0spwpeiaOtlWIW1UMf7lfGibkf3KaGIicfV385fmaT3BG3BE9g/640?wx_fmt=png&from=appmsg "")  
#   
# 前期准备  
  
攻击机：Kali 192.168.230.128  
  
靶机：vulnhub——Lampiao 192.168.230.217  
  
![屏幕截图 2024-03-18 092013.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyET73M5vt1GlPyG333QcDN0jFB3B30AL2qpEjibI0pFFD4C5mIiblXjhGTg/640?wx_fmt=jpeg&from=appmsg "")  
#   
# 复现过程  
  
1、对 192.168.230.0/24 这一个网段进行扫描  
```
nmap -sS 192.168.230.0/24
```  
  
![屏幕截图 2024-03-18 092047.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyET8AVq6HhMtQfLKqTPrOUHG3oKuYFK7DKFiaxDIDVl9M8H9IXjGdfOPYQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
2、扫到靶机的IP为 192.168.230.217，接着对其进行深度的端口扫描  
```
nmap -p 1-65535 -sV 192.168.230.217
```  
  
![屏幕截图 2024-03-18 092445.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETyytpgdPkIQAcacyg74pSXEU44J8reFu96L1j1NJtnnJt9KLQ2LHiceQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
3、发现三个开放端口，分别为 22、80、1898 ，访问80端口页面无反应，于是访问1898端口：http://192.168.230.217:1898  
  
![屏幕截图 2024-03-18 092536.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETvMIk1jpzIQze2ndNrEWKR4RyEBAibLQUMXoPwv8FhvW0mI19h8DZOsg/640?wx_fmt=jpeg&from=appmsg "")  
  
成功访问到一个登录页面  
  
  
4、在该页面最下方有一行 Powered by Drupal  
  
![屏幕截图 2024-03-18 092945.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETp2oJ7VsfPnM2vN8H661Z8IZqk8KxQ0V4NXcCr7x1mkQ8A6wib5ibNbcA/640?wx_fmt=jpeg&from=appmsg "")  
  
尝试搜索，发现Drupal是一套使用PHP语言开发的CMS  
  
![屏幕截图 2024-03-18 093034.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETxNJ8vfPI1PkclUfyMAN0YE9uNicQ3uYU8OrmKwyBZjItQzcbfIUXpZw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
5、通过阿里云漏洞库能搜索到对应的历史漏洞  
  
![屏幕截图 2024-03-18 093243.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyET0AQ3rqP30KEAEyHfLQhzeJOc0za2qHUkkrDqBlUseGrkMAha0cB8Og/640?wx_fmt=jpeg&from=appmsg "")  
  
其中也存在过远程代码执行漏洞 CVE-2018-7600 ，具体内容如下所示：  
  
![屏幕截图 2024-03-18 093336.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETedYIfOWwjZvhrt6tibShovrYmJ90xgbo9x55yVC3nYhsWvrdayTzz7Q/640?wx_fmt=jpeg&from=appmsg "")  
  
  
6、在msf里进行搜索对应的exp  
  
![屏幕截图 2024-03-18 093642.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETFGzS3WMFEEYblXasvKbtDNGRtfFd3MsHfO14NhhnvoNTbM5Sud3SMg/640?wx_fmt=jpeg&from=appmsg "")  
  
  
7、选择编号为1的，配置好部分参数，包括rhost，lhost，lport等等  
  
![屏幕截图 2024-03-18 094110.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETUmHkAqeSRsVib3AnomdeM3IVdADpS4V41reFYPKsym689ibIQiclP2c1w/640?wx_fmt=jpeg&from=appmsg "")  
  
  
8、输入命令run执行，成功反弹，但getuid得到的结果显示只有一个网站的权限，需要进一步提权到root  
  
![屏幕截图 2024-03-18 094532.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETFibbIbJGhlB7lkgia7BBUl0fv7XdUeZG16arVGHa6y7opUrIHtWiapRuQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
9、新建一个终端，使用python建立一个http服务器，并在对应目录下存放 linux-exploit-suggester.sh，然后在靶机这边通过wget命令下载  
  
![屏幕截图 2024-03-18 151711.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETCLic3CianIX2vlk3mVJyYHMw9AJjzxPPU1C9eTb6q59p6hAWict5ibfkmQ/640?wx_fmt=jpeg&from=appmsg "")  
  
  
10、chmod +x 增加执行权限之后，执行该脚本文件  
  
![屏幕截图 2024-03-18 151906.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyET4b6ad6nn70UAV9TmeAVCvW3nZ1E4zsiaxneBFBXu5tOcSaW6ibpBWwnA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
11、通过脚本测试出来的结果，发现存在脏牛提权漏洞  
  
![屏幕截图 2024-03-18 151936.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyET6jgGibMdXoiacOC7uKsdPVwkxicbrGANA89k3j5FU6ygdfJFJuV7bUgicA/640?wx_fmt=jpeg&from=appmsg "")  
  
  
12、在Github上找对应的exp  
  
![屏幕截图 2024-03-18 153144.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETnbMg9TnfCNnicsTmTRCPfPiaiaZ2UPt1TQQicaDtY26mXaBib8JJQ8YmKxw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
13、使用同样的方式传输到靶机上面  
  
![屏幕截图 2024-03-18 153935.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETiblVODeArpDx6Tb0V0QMHiamia4Dic8YCjQibdcB6yD399Hr9s5CZFNrPOw/640?wx_fmt=jpeg&from=appmsg "")  
  
  
14、解压后进入到exp对应的目录下，需要先对.cpp文件进行编译（使用make命令），之后才能执行  
  
![屏幕截图 2024-03-18 155507.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETd8WMxraWI7AR3PqWMJibtuSohNlU2RESj3qz3Yib4XVDpXbibqUP5482A/640?wx_fmt=jpeg&from=appmsg "")  
  
执行结果成功得到root用户的密码：dirtyCowFun  
  
![屏幕截图 2024-03-18 160301.jpg](https://mmbiz.qpic.cn/sz_mmbiz_jpg/ZQgABwTC9I5YbN4Fv6HqKut3rJOQibyETqlgVYsia24eeVV3FVQczOhr1MO7nftUfmmYsyDr38DDxdBoeVuWbbrg/640?wx_fmt=jpeg&from=appmsg "")  
  
提权成功！目前用户为root  
  
