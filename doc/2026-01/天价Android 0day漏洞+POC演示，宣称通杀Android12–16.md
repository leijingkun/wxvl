#  天价Android 0day漏洞+POC演示，宣称通杀Android12–16  
原创 网空闲话
                    网空闲话  网空闲话plus   2026-01-15 22:52  
  
2026年1月16日暗网监测发现，一则关于Android 0day漏洞及完整exploit chain的售卖信息最早出现在Telegram的某个频道中。相关内容随后被转发至地下技术论坛及其他Telegram渠道，并配合一段漏洞利用的PoC演示视频，用于展示其宣称的攻击能力。  
完整Android exploit源代码的标价为4.5Million美元；单独出售的exploit shellcode标价为400k美元，可谓天价。另有目标植入服务，价格也是相当不菲。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/0KRmt3K30icUKZ49noRPrk93zQCsMmJKLNwrxeXWYGFibBI3K7ro5UASVAjDTVNudWBpDx3ygicAicKa5CKKv20Bqg/640?wx_fmt=png&from=appmsg "")  
  
最早来源与传播路径  
  
根据目前可见的信息，该售卖消息最初发布于Telegram渠道，发布时间为1月16日。在Telegram渠道中，内容以完整售卖帖形式出现，包含漏洞名称、影响范围、利用能力、价格信息及联系方式。此后，相同或高度相似的内容被发布至地下论坛Breachstars，形成跨平台传播。  
  
在不同平台中，发布者反复使用同一Session ID作为身份标识，并引导潜在买家通过Telegram私聊获取PoC、价格细节及进一步说明。  
  
漏洞与影响范围描述  
  
根据售卖信息中的公开描述，该漏洞被命名为“OZDMessage Parser”，漏洞类型被归类为内存破坏（Memory Corruption）。信息中称该漏洞影响Android12至Android16多个系统版本，覆盖ARM 32位与64位架构。  
  
漏洞被描述为与“消息解析（Message Parser）”相关，但相关内容未披露具体系统组件、代码路径或厂商修复状态。  
  
Exploit Chain能力说明  
  
售卖信息明确指出，该漏洞并非单一利用点，而是构成一条完整的exploit chain。按照描述，该链路可实现远程代码执行（RCE），并进一步完成沙箱逃逸与权限提升，最终在目标设备上获得Root权限（uid=0）。  
  
在能力描述中，售卖方将该exploit链路整体定位为可直接对目标Android设备实现系统级控制。  
  
利用特征与运行表现  
  
在对利用方式的说明中，信息强调该exploit具备较强的隐蔽性特征。其描述称，漏洞利用过程不需要任何用户交互，并且在执行过程中不会触发系统崩溃界面或异常UI提示。  
  
这些特征与随附的PoC演示视频一并用于展示利用过程的运行状态。  
  
售卖内容与价格信息  
  
从公开内容来看，售卖方将相关能力拆分为不同层级进行定价。其中，完整Android exploit源代码的标价为4.5Million美元；单独出售的exploit shellcode标价为400k美元；另有一档100k美元的shellcode，注明仅适用于有限设备范围。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/0KRmt3K30icUKZ49noRPrk93zQCsMmJKLicCHcJ4kyB2GR4FGHVdmoOYEUABIQxOgFibS4vG2KRhPIKC36TUz471w/640?wx_fmt=png&from=appmsg "")  
  
除直接出售漏洞相关代码外，信息中还提供按设备计费的“Phone Exploitation services”。根据描述，单台设备的利用服务费用为50,000美元；若一次性购买10台设备，价格则降至20,000美元/台。相关服务被描述为同时支持Android与iOS平台。  
  
版本支持与演示说明  
  
售卖信息称，购买方可在Android12–16范围内对exploit进行测试，并在确认效果后完成交易。此外，信息中特别提到，在Android 16更新覆盖至各厂商机型后，可提供对应版本的演示视频作为补充展示。  
  
目前流传的PoC视频即被用于展示漏洞触发、利用执行以及权限获取的整体过程。  
  
  
  
配套控制框架披露  
  
除漏洞和exploit本体外，售卖信息还提及可提供完整的C2（Commandand Control）框架。相关描述指出，该框架以GUI控制面板形式呈现，可用于集中管理和控制已被入侵的设备。  
  
售卖方特别说明，该GUI控制面板本身并非exploit，而是用于操作和管理被攻陷终端的控制系统。  
  
联系方式与身份标识  
  
在Telegram渠道及论坛帖子中，发布者多次重复同一Session ID，并提供Telegram用户名@evolvedbunnyberserker以及频道链接shinychannels，用于跨平台识别身份及后续沟通。  
  
结语  
  
截至目前，该Android 0day漏洞售卖信息仍以Telegram渠道首发、论坛与即时通讯平台扩散的形式存在，并通过PoC演示视频配合说明其宣称的攻击能力、适用版本及交易方式。相关内容尚未披露进一步的技术细节或官方信息。  
  
参考来源：TG频道  
  
  
