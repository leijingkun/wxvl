#  2644 万美元被盗背后：Truebit Protocol 合约漏洞分析  
原创 慢雾安全团队  慢雾科技   2026-01-12 07:51  
  
作者：enze & Lisa  
  
编辑：77  
  
背景2026 年 1 月 8 日，去中心化离线计算协议 Truebit Protocol 遭受攻击，攻击者利用合约漏洞获利约 8,535 ETH（约合 2,644 万美元）。以下为慢雾安全团队对本次攻击事件的详细分析。  
  
根本原因Truebit Protocol 的 Purchase 合约在计算铸造 TRU 代币所需 ETH 数量时，由于整数加法运算缺乏溢出保护，导致价格计算结果异常归零，攻击者得以近乎零成本铸造大量代币并套取合约储备金。  
  
前置知识  
  
Truebit Protocol 是一个去中心化的链下计算市场，旨在将复杂计算任务从区块链主网转移至链下执行，同时通过经济激励机制保障计算结果的正确性。协议引入了原生代币 TRU，TRU 代币采用算法化的弹性供应机制，TRU 的实时价格由合约内 ETH 储备量与 TRU 流通供应量的比率函数动态决定，其铸造与销毁完全由链上智能合约自动管理：铸造：用户向 Purchase 合约存入 ETH，按算法价格铸造 TRU销毁：用户销毁持有的 TRU，按算法价格从合约提取 ETH攻击分析  
  
攻击者地址：0x6C8EC8f14bE7C01672d31CFa5f2CEfeAB2562b50攻击合约：0x764C64b2A09b09Acb100B80d8c505Aa6a0302EF2相关攻击交易：0xcd4755645595094a8ab984d0db7e3b4aabde72a5c87c4f176a030629c47fb0141. 攻击者调用 Purchase 合约的 getPurchasePrice 函数，查询铸造 240,442,509,453,545,333,947,284,131 枚 TRU 代币所需的 ETH 数量。由于该数值经过精心构造，导致价格计算过程中发生整数溢出，函数返回值为 0。2. 攻击者调用 Purchase 合约的 0xa0296215 函数（铸造函数），传入上述代币数量。由于所需 ETH 价格被计算为 0，攻击者无需支付任何 ETH，成功铸造了 240,442,509,453,545,333,947,284,131 枚 TRU 代币。3. 攻击者立即调用 Purchase 合约的 0xc471b10b 函数（销毁函数），将刚铸造的全部 TRU 代币销毁，从合约储备中兑换出 5,105.069 ETH。4. 攻击者重复执行上述「铸造 → 销毁」流程。随着 TRU 代币供应量 (S) 增大，后续铸造需支付少量 ETH，但铸造所得代币的价值仍远超支付成本，套利空间依然可观。攻击者持续操作直至耗尽合约的 ETH 储备。攻击原理剖析  
  
1. 价格计算公式  
  
通过对 Purchase 合约进行反编译分析，我们定位到核心的价格计算函数：该函数用于计算铸造指定数量代币所需支付的 ETH 数量，其计算公式如下：Price = (100 * A² * R + 200 * A * R * S) / ((100 - T) * S²)其中：A (AmountIn)：用户请求铸造的代币数量R (Reserve)：合约当前的 ETH 储备量S (Supply)：代币当前的总供应量T (THETA)：合约参数，固定值为 752. 漏洞原因  
  
该合约采用 Solidity ^0.6.10 版本编译。在 Solidity 0.8.0 之前的版本中，算术运算符（+、-、*）不包含内置的溢出检查。当运算结果超过 uint256 的最大值（2²⁵⁶ - 1）时，会发生静默溢出(Silent Overflow)，结果将回绕至 0 附近的小数值。在价格计算的关键代码中：然而乘法运算使用了 SafeMath 库进行溢出检查，但分子的加法运算 v12 + v9 直接使用了原生 + 运算符，未进行溢出保护。这构成了本次攻击的核心漏洞点。  
  
3. 攻击数值分析  
  
以攻击者首次铸造交易为例：计算过程：溢出判定：由于 v12 + v9 的结果超过了 uint256 的最大值，发生溢出回绕。溢出后的分子值变为一个极小的数，经过整数除法后，最终计算出的 Price = 0。4. 攻击影响  
  
攻击者通过精心构造 AmountIn 参数，使得：1.乘法运算均通过 SafeMath 检查（不触发 revert）2.加法运算发生溢出，分子回绕为极小值3.整数除法结果为 0最终，攻击者无需支付任何 ETH，即可铸造大量代币。MistTrack 分析据链上追踪 & 反洗钱工具 MistTrack 分析，攻击者在本次事件中获利约 8,535 ETH（约合 2,644 万美元）。被盗的 8,535 ETH 首先转移到三个新地址，最终均转入 Tornado Cash。从链上看，攻击者地址曾分别在 2025/11/20、2025/12/06、2025/12/27 有过交易记录，主要行为如下：2025/11/20：在 Avalanche 上通过 Drain 获得资金，并通过 Rhino.fi 跨链到 BNB Chain2025/12/06：在 BNB Chain 上将收到的资金通过 Rhino.fi 跨链到 Ethereum2025/12/27：在 Ethereum 上通过 RUN 获得 4.98 ETH，疑似为攻击者之前发起的另一个攻击，共 5 ETH 转入 Tornado Cash目前 MistTrack 已对相关地址进行标记。  
  
结论  
  
本次攻击的根本原因是 Purchase 合约在计算铸造价格时，分子项的加法运算未使用 SafeMath 库进行溢出保护。由于合约采用 Solidity 0.6.10 版本编译，原生 + 运算符不具备溢出检查。攻击者通过构造特定的铸造数量，使得加法运算结果超过 uint256 最大值并发生溢出回绕，导致价格计算结果为 0，从而实现近乎零成本的代币铸造和套利。慢雾安全团队建议对于使用 Solidity 0.8.0 以下版本的合约，开发者应确保所有算术运算均使用 SafeMath 库进行保护，避免因整数溢出导致的逻辑漏洞。  
  
**往期回顾**  
  
[慢雾 CISO 23pds 受邀参与「Web3 领袖项目」公开课分享](https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA==&mid=2247504157&idx=1&sn=1979e1ad45483ed613dce71f8fa52a19&scene=21#wechat_redirect)  
  
  
[慢雾出品 | 2025 区块链安全与反洗钱年度报告](https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA==&mid=2247504119&idx=1&sn=9e6afef4262207a8da1f16a66368ced6&scene=21#wechat_redirect)  
  
  
[慢雾 Q4 追踪实录：协助被盗客户冻结/追回百万美元资金](https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA==&mid=2247504087&idx=1&sn=5325391cb456ec020da8f3b5b47a8178&scene=21#wechat_redirect)  
  
  
[圣诞劫 | Trust Wallet 扩展钱包被黑分析](https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA==&mid=2247504059&idx=1&sn=ea64fa6ef53f86b2dd2508d3dfc42f77&scene=21#wechat_redirect)  
  
  
[慢雾：去中心化永续合约安全审计指南](https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA==&mid=2247504034&idx=1&sn=c4174518c7087011675ce1c0a165bc5c&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_png/qsQ2ibEw5pLbEP8f4tadFenoLauzHpicWdWbVap3aia38LUGPflBho9ibDHXjoG5fecGJSaYa4S4zYdoicXibSmjv9tg/640?wx_fmt=png&from=appmsg "")  
  
**慢雾导航**  
  
  
**慢雾科技官网**  
  
https://www.slowmist.com/  
  
  
**慢雾区官网**  
  
https://slowmist.io/  
  
  
**慢雾 GitHub**  
  
https://github.com/slowmist  
  
  
**Telegram**  
  
https://t.me/slowmistteam  
  
  
**Twitter**  
  
https://twitter.com/@slowmist_team  
  
  
**Medium**  
  
https://medium.com/@slowmist  
  
  
**知识星球**  
  
https://t.zsxq.com/Q3zNvvF  
  
