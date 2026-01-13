#  低成本玩转SDN：实测OpenDaylight资源消耗仅1.1GB，完美纳管华为设备，开源方案真香！  
原创 衡水铁头哥  铁军哥   2026-01-13 01:03  
  
对于前面介绍的Guard路由  
（[H3C这个冷门功能Guard路由有什么用？我搭了一套实验环境，发现了它的秘密](https://mp.weixin.qq.com/s?__biz=MzI4NjAzMTk3MA==&mid=2458863089&idx=1&sn=6665f631a110d61d2b2cfa76d2943fcc&scene=21#wechat_redirect)  
  
），还有稍微复杂的SR-MPLS  
（[验证成功！翼航仿真实训平台完美支持SR-MPLS L3VPN等前沿技术，网络学习利器](https://mp.weixin.qq.com/s?__biz=MzI4NjAzMTk3MA==&mid=2458863151&idx=1&sn=1ccbf3bd4f684ab5ed25b3ee3b6c3fed&scene=21#wechat_redirect)  
  
），再到时髦的SRv6  
（[超越SR-MPLS！SRv6实测：基于纯IPv6数据面承载IPv4 VPN业务，体验协议简化之美](https://mp.weixin.qq.com/s?__biz=MzI4NjAzMTk3MA==&mid=2458863205&idx=1&sn=91a7936ffb23ea3f42a2058396fa52ac&scene=21#wechat_redirect)  
  
），实际使用中都是有正规控制器对接的。但是做实验肯定要控制成本，那就考虑一下开源控制器，主流的应该是ODL和ONOS，用到最多的，貌似应该是ODL。  
  
  
目前OpenDaylight（ODL）的最新版本是0.22.1，发布于2025年10月19日，同日发布的，还有一个0.21.3版本。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/5fL4uXAOMM40OB544uAC8b63WdXkwlVwxjScgicqMiauY2C5GGROiaY0A8mvGgMC0RVZYIRhicjTpGfZDYCKc44wHA/640?wx_fmt=png "")  
  
既然是测试使用，我们直接上最新的0.22.1版本。在下载页面，有两个下载版：一个是tar.gz压缩包，一个是zip压缩包。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/5fL4uXAOMM40OB544uAC8b63WdXkwlVwgL4hPsVSicOWFWx9fCmpt13d0luXWmf5szwudI3jmDiabn56ZMtVhyVA/640?wx_fmt=png "")  
  
