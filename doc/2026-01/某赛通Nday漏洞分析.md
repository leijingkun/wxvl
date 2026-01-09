#  某赛通Nday漏洞分析  
原创 redcat  虹猫少侠的江湖路   2026-01-09 09:58  
  
免责声明  
  
![图片](https://mmbiz.qpic.cn/mmbiz_gif/QtaE6uFmibPlPBODMGcLpNt9QBAWypThbpd3jzvdXXROdjXCddIaUR6icvQfhjUVDAXNPbM4SuibSU8GwCr9t5VvA/640?wx_fmt=gif&from=appmsg&wxfrom=5&wx_lazy=1&tp=webp#imgIndex=0 "")  
  
  
由于传播、利用本公众号  
虹猫少侠的江湖路  
所提供的信息而造成的任何直接或者间接的后果及损失，均由  
使用者本人负责，公众号  
虹猫少侠的江湖路  
及作者不为此承担任何责任，一旦造成后果请自行承担！如有侵权烦请告知，会立即删除并致歉。  
  
最近没项目闲的有点蛋疼，看到某公众号发了关于某安全文档系统的前台任意文件上传漏洞，并且明确标注了受影响的版本号，该系统在攻防中内外网还是存在一定占比的，于是打算分析下exp备用。  
  
直接下载对应的两个版本源码进行对比，整个过程比较简单：  
  
过程：  
  
下载  
Beyond Compare 对比工具进行差异对比：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIo0Tw9G60jLmTgsLxr2kXEWIicibF5mTnDxWtxpElD1kHAFc4CSKsiaA1Q/640?wx_fmt=png&from=appmsg "")  
  
该工具默认不会反编译class文件的，需要下载对应的插件，直接询问ai：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIiaU6XLjTODyF7AD48DxvvicG2yYjIUTSZHJ5tv8R6WkpSBfBgvcwSz2Q/640?wx_fmt=png&from=appmsg "")  
  
安装这个插件之后就可以正常查看class文件了：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIDrwCRbysiajO8jRyLOrj4RAl8rv6Ld4HTGUPLfS4PoTj2Rt3dxAXXZA/640?wx_fmt=png&from=appmsg "")  
  
看到某方法中增加了一个baseDir的代码，猜测大概率就是禁止往上跨目录的修复，并且这个方法后续还有文件操作，所以大概率就是这里了：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIUMibPld8gCrVxUZz1HZ185wkBgZWKjRa5ictseKrNbufibHNvLfH6coYw/640?wx_fmt=png&from=appmsg "")  
  
然后用idea看了下这个控制器的方法，这里就是存在跨目录的任意文件上传操作，后续就是过一些验证了：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQITPmwBzFrn1r9XpdvZTsvS9PTl31d8PS5GVTvMl32EibTTJPGdRLgjzQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIYEC2Qpg99s11vJphVIs9IwNugX3RNRnvxxLZSAUSibmEicicjtATAtRAw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIicHEYicLTk5cQbpGtgXZic4rNFosZnsWAiaSvrBkbbKdG5CQG3V85Nl9sw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQICAibJjJ9H6yibNSFd83KbEkM0XmBaiaGykia2LYKBztzG4mgF9rdfiaYVrA/640?wx_fmt=png&from=appmsg "")  
  
本地没有下载链接sqlite数据库的工具，这里直接让ai搞：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQI4CrGHn2XUFBcWT80ib7ZANXXWicVWF53SxTGO2F7c0l4Pcp4RRVzorWw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIczOhCicDL1DcmvwWMhjGhQoTUe2E9C33GgJaLdq3JictcicsPkiaGJ8jxw/640?wx_fmt=png&from=appmsg "")  
  
最后也是成功的复现了某通告中的内容：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/RzCEZQRRgcdOhOibZl9YveMiaqyz987ibQIB0cYoabMbCb3xxSpcuVqymfQw0Wl5TRchr0EQCpfVf6iaxkv3IAibkIg/640?wx_fmt=png&from=appmsg "")  
  
提示：这漏洞不是默认的对外的web端口上的应用，同时这个数据库中的密码也是有一部分系统搭建之后改了的，所以影响一般，在内网遇到或许可以试试，漏洞的特征倒是不明显，没有../../ 这类敏感字符。  
  
  
