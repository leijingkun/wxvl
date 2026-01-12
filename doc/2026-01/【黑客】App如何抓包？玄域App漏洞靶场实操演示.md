#  【黑客】App如何抓包？玄域App漏洞靶场实操演示  
 赤弋安全团队   2026-01-12 00:46  
  
前置准备  
  
玄域靶场地址：www.shangsec.com  
  
  
1、用到的工具和软件：burpsuite、小黄鸟、MuMu模拟器、玄域App漏洞靶场、MT管理器  
  
  
2、大致步骤：环境配置、抓包、数据转发到burp  
  
  
3、视频版在小破站，全网同名：无名的安全小屋  
  
**0x01 MuMu和小黄鸟安装**  
  
直接百度搜索MuMu模拟器和小黄鸟抓包工具，直接下载安装即可，这里就不做过多的赘述。  
  
**0x02 玄域App漏洞靶场安装**  
  
访问玄域靶场地址：www.shangsec.com  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkrgehwp2spLOdLA4qZm5olNkibPuMXMNehshWEfowNfVtKexqN93mXPLqPLvD62Fjgn1LfyTBHqnb4A/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkrgehwp2spLOdLA4qZm5olNkGrL2kUNLVYkkFNqPSvTROXWCI6W0ibj2Q9ljDEZA0MBB0cwbKkOM3tQ/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURcyK8716Azu1fqPOQR7aEebeEAKdmlkWzUgdfUlqrZVoOTniaK71RtQw/640?wx_fmt=png "")  
  
然后直接下载即可  
  
**0x03 环境配置**  
  
MuMu模拟器配置：  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURFy8PH9MmefPeG2BicBUL58Ij430BYF9zIKbfkmRAxLWR1l1XLAJt3YA/640?wx_fmt=png "")  
  
新建设备后进行设置  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURpyck27kLsCu8HyEiba4KN7YcHJq8rWNPMzJLvD5NElAPXXIFHA3JNVQ/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdUR6z7gS5Uia1RxyU3TUpo2vFnhmicbht6s1JZZicjCbnunkSiaDffUnqMUbA/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURiaJia2g5Obn6DbLLpvB5BgyYdKD2r0IP4APZnfwAT0asMfyDN0Gf5atQ/640?wx_fmt=png "")  
  
然后小黄鸟导出证书  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURUE1BngFrPv0a5NKxOhZAMm6DambMWnFF18I6W3avz2lXPlRz1IHiaYQ/640?wx_fmt=png "")  
  
启动模拟器，打开MT管理器，把证书拖入进去  
  
把左边的证书文件，长按复制到右边的指定目录（/system/etc/security/cacerts）  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURHIEz0SOIban2RlOS559HaasX4xQhEfTibFBkqjx50Ut8UiamCwciakY2g/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURHIEz0SOIban2RlOS559HaasX4xQhEfTibFBkqjx50Ut8UiamCwciakY2g/640?wx_fmt=png "")  
  
配置代理，来到wifi高级设置，填写小黄鸟的信息即可  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURjaEOgXyXottYPhZN8WJrURNo03aDZo0WG0e8uviaDMIfXKoXbxZczQw/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURX690LDkM0Nfzs6EwQHdsts4zjbgIGDkUEsIU9QWAicwYV6RQtvXutcA/640?wx_fmt=png "")  
  
  
**0x04 抓包并转发数据包**  
  
设置二级代理，把数据包转发给burpsuite  
  
二级代理的信息填写burp的代理地址即可  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURIORbviamDOyg2sGIiarlwaw7sjeTGfqeAtpiaFSTmNtO7VqvOA8fTv4bA/640?wx_fmt=png "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURkrblBFGLMf0EXOhVdibneyWib9cooVl3K3sXmOib8MbSqIOGbcjlZZ8AQ/640?wx_fmt=png "")  
  
  
**0x05 愉快的测试**  
  
然后就可以愉快的测试了  
  
![](https://mmbiz.qpic.cn/mmbiz_png/BZbILNBpkriaT5iaxPOrT4WDfGUggIEdURjv1YlPxcmTKvHAvwIeBVq8vFXMicdbMD8GS1bYfuGe7uiaAbWVAUpUpQ/640?wx_fmt=png "")  
  
  
