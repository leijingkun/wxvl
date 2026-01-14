#  Nessus插件开发实战：从零开始编写高效漏洞扫描脚本  
原创 奥村燐  弥天安全实验室   2026-01-14 10:27  
  
   
  
网安引领时代，弥天点亮未来 ****  
  
  
  
  
  
****  
  
****  
  
**0x00前言描述**  
  
  
  
****  
**需要寻找一个能长期维护自己编写POC的框架，要求具备端口扫描、服务识别、指纹探测和POC扫描功能。尝试过不少免费框架，但都不太理想：要么存在较多BUG(挺多奇怪的问题)，要么担心后续停止更新会带来麻烦(太监了)。综合考虑后，选择了Nessus作为解决方案，其稳定性和持续更新能力令人满意。 0x01操作步骤1.第一部分主要设置插件一些对外显示，没啥好说的2.第二部分主要设置执行顺序、分类、数据获取来源**  
3.script_category  
具体顺序如下图  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4kLHBFvroljIniaYplR0iaicVe6MokkzveqpYrZO0LdzCIgDhACTiaGwFxg/640?wx_fmt=png&from=appmsg "")  
  
  
4.script_family设置了插件列表的所属分类，在插件列表显示  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4ibYuxQZLFIZl5gHmiaJthzlxiapwmic1tBkH0hJicibyCFXGgFsQzhDDF6aQ/640?wx_fmt=png&from=appmsg "")  
  
  
5.  
script_dependencies和  
script_require_keys代表插件的数据来源，代表从服务识别插件获取所有的www(也就是web端口)  
```
script_dependencies("find_service.nasl");
script_require_keys("Services/www");
```  
  
  
  
6.后面就是脚本的执行逻辑了，也没啥好说的  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4yAuQz1u7Z2xxGOQicFgQKUiaXZCniaWqGYktY9UsibN9c29tHXr2jm7tpw/640?wx_fmt=png&from=appmsg "")  
  
  
7.测试效果  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae47Ipx3krC8X34ODoFVlQjEUKNib528WPLg50l7FoEYmCcSkA96IUQkVA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4LPIjYjNKYeeibOIhAVpGdDaibZp5aZoqjCCPjOmUrjapVb45WxWqicpzQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4OWsh4mWdeIW2LfibGJXMrrxQLFmC8icPhibOpXB3GiaLfia6TkKVqwCVyJA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_png/MjmKb3ap0hCiabr7wibibQtaucibTrpQpiae4plmibbAZpovtQOtD1ibWBibvRZ7x56QHsH9Hok5WbcaGkMfpZRBu9aAAg/640?wx_fmt=png&from=appmsg "")  
  
  
  
7.具体扫描效果可以看下面视频  
  
(  
演示过程只加载了服务识别和标题探测的插件  
)  
  
  
  
****  
  
  
知识分享完了  
  
喜欢别忘了关注我们哦~****  
  
学海浩茫，  
  
予以风动，  
  
必降弥天之润！  
  
  
**弥  天**  
  
**安全实验室**  
  
****  
  
  
