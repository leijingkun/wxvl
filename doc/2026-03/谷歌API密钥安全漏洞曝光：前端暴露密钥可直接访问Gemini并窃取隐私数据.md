#  谷歌API密钥安全漏洞曝光：前端暴露密钥可直接访问Gemini并窃取隐私数据  
胡金鱼
                    胡金鱼  嘶吼专业版   2026-03-02 06:03  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/wpkib3J60o297rwgIksvLibPOwR24tqI8dGRUah80YoBLjTBJgws2n0ibdvfvv3CCm0MIOHTAgKicmOB4UHUJ1hH5g/640?wx_fmt=gif "")  
  
安全研究人员最新发现，嵌入在可访问前端代码中、用于地图等服务的谷歌API密钥，可被用于向Gemini AI助手进行身份认证，并访问私密数据。研究人员在扫描来自多个行业机构（甚至包括谷歌自身）的公开网页时，发现了近3000个此类密钥。   
  
该问题源于谷歌推出Gemini助手、开发者开始在项目中启用大语言模型API之后。在此之前，谷歌云API密钥不被视为敏感数据，即使公开暴露也普遍认为无安全风险。  
  
开发者通常使用API密钥为项目扩展功能，例如在网站中加载地图、嵌入YouTube视频、使用统计服务或Firebase相关功能。而随着Gemini的推出，谷歌云API密钥同时具备了谷歌AI助手的身份凭证权限。  
  
研究人员表示：攻击者可从网页源代码中复制API密钥，通过Gemini API服务访问私密数据。  
  
由于Gemini API并非免费使用，攻击者还可滥用该权限调用接口，为自身牟利。根据模型与上下文窗口不同，恶意攻击者若将API调用量刷满，单个受害者账户每天可能产生数千美元的费用。这些API密钥已在公开JavaScript代码中暴露多年，如今却在无人察觉的情况下突然获得了更高危的权限。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/fHEm7hZn9HJfHLibfFkfK1rWwyZOFvEp4a23rgD8ZBK5Okxeq8EBLv35rIeicWUMVZZggNPB5Mjl7dBIGflS5hCT7ehmsSVibOTKmW5TGYacPU/640?wx_fmt=png&from=appmsg "")  
  
研究人员对2025年11月的Common Crawl数据集进行分析，在代码中发现超过2800个正在使用、且公开暴露的谷歌API密钥。  
  
研究人员称，其中部分密钥被大型金融机构、安全公司和招聘公司使用。他们已向谷歌报告该问题，并提供了来自谷歌自身基础设施的相关样本。   
  
其中一个案例显示，某枚仅用作标识符的API密钥至少从2023年2月起就已部署，并嵌入在谷歌某产品的公开网站页面源代码中。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/fHEm7hZn9HKwLh7hj02icgTH7GUU5iaW7Y7RZWTiaTVCsyVuSwg288Rqa1yZjyAML7VdPUmqfkBwwTcuic2NcbE46cI67FeH1E244oVRHDT8QmM/640?wx_fmt=png&from=appmsg "")  
  
Google的暴露密钥  
  
安全研究人员使用该密钥调用Gemini API的/models接口，成功列出了可用模型。并于2025年11月21日向谷歌通报该问题。经过多轮沟通，谷歌在2026年1月13日将该漏洞归类为“单服务权限提升”。  
  
谷歌表示，目前已实施主动检测机制，拦截试图访问Gemini API的泄露API密钥。宣布将采取以下措施：  
  
·新的AI Studio密钥默认仅开放Gemini权限范围  
  
·泄露的API密钥将被禁止访问Gemini  
  
·检测到密钥泄露时将主动发送通知  
  
谷歌建议开发者检查项目中是否启用了Gemini（生成式语言API），审计环境中所有API密钥是否存在公开暴露，并立即轮换存在风险的密钥。  
  
参考及来源：  
https://www.bleepingcomputer.com/news/security/previously-harmless-google-api-keys-now-expose-gemini-ai-data/  
  
![](https://mmbiz.qpic.cn/mmbiz_png/fHEm7hZn9HJFn91ITyIQEpUUdfGC8Zf5QTqMibaicA9OlMRj5vGQkdJlY6b9Al0F2usnPuyPb3icR6BNgiansBlQjicYeiasZXV05kiaFGxmHXxzX0/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/fHEm7hZn9HL46sqMKXT8VQEHKhXJgNJXxD928dgsCXf0YsF7gFbwEVBhJVVOIQFrFmpPseS4H9IJynibovmSg0h1oDNPaTAexLCJX8egybE0/640?wx_fmt=png&from=appmsg "")  
  
  
