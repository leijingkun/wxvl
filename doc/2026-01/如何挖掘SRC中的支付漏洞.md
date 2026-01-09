#  如何挖掘SRC中的支付漏洞  
 黑白防线   2026-01-09 01:00  
  
点击上方蓝字关注“公众号”  
  
前言  
  
  
我们在进行漏洞挖掘的时候，要多熟悉目标的功能点，并且还得心细，认真分析每一个可能的数据包  
  
正文  
  
  
这次的测试目标是一个app  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ22qMTUtdtUicQYUTtz5icwYSDOpJia0okolfK712D5K3gq9dbpN4qaMnqg/640?wx_fmt=png&from=appmsg "")  
  
  
  
我们目前的积分是12万  
  
点击进去充值中心  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ20ugCJGGkXV7EYOkicQ5erOEh03OzFfOEJsY99lMxolT1YUibjTBUHPxw/640?wx_fmt=png&from=appmsg "")  
  
  
  
可以看到原价是8888  
  
我们点击支付，然后抓包  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2wgSBiafvz9RJyCud55gL6lkS9PY5ia3dFgRia8hqicibrSUR71nNOfnyjMg/640?wx_fmt=png&from=appmsg "")  
  
  
  
account字段就是我们的金额，直接改成0.01，发包  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2QCHY3mniasOIwtAJNFnkIUHELpRHOGDu2q8ppEhcOhM1J2FnDodabfA/640?wx_fmt=png&from=appmsg "")  
  
  
  
支付0.01  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2HEdGWdkoU2XdOqKMBJ7QqDcEMaCfILnticGOwErPsa8kdIW571dnICw/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2QFcTQbJjeNmgQFrRMJxSB62UN8OVwiaXvg9J1DoGriaKEtgB9t78jFIQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
显示支付成功  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2JibdJCYvVS2KJRJk9icCuicKwia6RXX77sroj4kibxvqNpm8vR38j0Ctic2g/640?wx_fmt=png&from=appmsg "")  
  
  
  
积分也变了  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/NksHPtAWibIbHjw867VibCXcCh7ibznWgZ2BVDaR3EuPYLg1ZJxjuiafZJrvmAUz2AKf1EGnIXagFPmFVsQqA6Wp3w/640?wx_fmt=png&from=appmsg "")  
  
  
  
实现用0.01购买至尊卡  
  
备注  
  
  
以上漏洞已修复，测试只为证明漏洞存在  
  
  
免责声明  
  
```
本公众号“代码防线”致力于传播网络安全领域的技术知识，所有内容仅限用于合法的学习交流与学术研究目的。
本公众号坚决反对任何形式的非法或恶意网络行为。
本公众号所发布的所有信息均基于作者的个人理解与实践经验，仅代表作者自身的观点。这些内容仅供读者参考，不构成任何具有约束力的专业承诺或保证。
任何人士因使用或参考本公众号提供的信息、工具或技术所引发的任何直接或间接损失，本公众号概不负责。
本公众号介绍的技术、工具及方法，严格限定在合法合规的框架内使用，严禁用于任何非法用途。任何违反国家法律法规的行为，均与本公众号的立场完全相悖，并需自行承担由此产生的一切法律后果。
使用者必须充分知悉并独立承担运用本公众号内容所可能带来的全部风险，并对其自身行为负完全责任。
本公众号保留在不事先通知的情况下，随时更新或调整此免责声明的权利。
本公众号所发文章相关漏洞已修复，仅供学习
```  
  
  
