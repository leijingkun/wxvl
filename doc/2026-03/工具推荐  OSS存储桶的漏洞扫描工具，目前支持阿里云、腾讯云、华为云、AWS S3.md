#  工具推荐 | OSS存储桶的漏洞扫描工具，目前支持阿里云、腾讯云、华为云、AWS S3  
bitboy-sys
                    bitboy-sys  星落安全团队   2026-03-03 16:01  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/spc4mP9cfo75FXwfFhKxbGU93Z4H0tgt4O9libYH9mKfZdHgvke0CeibvXDtNcdaqamRk3dEEcRQiaWbGiacZ2waVw/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=0 "")  
  
点击上方  
蓝字  
关注我们  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/WN0ZdfFXY80dA2Z4y8cq7zy2dicHmWOIib5sIn8xAxRIzJibo2fwVZ3aicVBM8RnAqRPH5Libr4f02Zs5YnMLBcREnA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=1 "")  
  
  
现在只对常读和星标的公众号才展示大图推送，建议大家能把  
**星落安全团队**  
“  
**设为星标**  
”，  
否则可能就看不到了啦  
！  
  
【  
声明  
】本文所涉及的技术、思路和工具仅用于安全测试和防御研究，切勿将其用于非法入侵或攻击他人系统以及盈利等目的，一切后果由操作者自行承担！！！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/g1pTgnczBiaGsLLysCuVDXCVxrtArQSiaFPEzfCiapAUL5qic6japlSeRXgUib7gpndjBC13pDB0actuRiaTVdCLOyicqNiaQboc7MALaQqlPJtSPeo/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&watermark=1&tp=webp#imgIndex=2 "")  
  
**背景介绍**  
  
OSS_Scanner 是一款多厂商 OSS 存储桶漏洞扫描工具，支持阿里云、腾讯云、华为云、AWS S3 等主流对象存储服务，可检测敏感文件泄露、匿名上传 / 删除、CORS 配置过宽等 10 + 类安全风险，帮助安全测试人员快速发现存储桶配置漏洞。  
  
![](https://mmbiz.qpic.cn/mmbiz_png/rlSBJ0flllmMCxSLGMUlOHVRibS71HAicVpNOxtNCDe7R4WiajB6E6DibuxmeLPDc1UdZpx8V82ia5fZE0hqkPRss9g/640?wx_fmt=png&from=appmsg "")  
  
功能特性  
  
功能模块检测能力敏感文件扫描密钥文件（.env、id_rsa）、数据库备份（.sql、.bak）、配置文件（wp-config.php）等权限漏洞检测公开可列目录、匿名 PUT 上传、匿名 POST 表单上传、匿名 DELETE 权限配置风险检测CORS 配置过宽（允许任意域名跨域）、敏感 HTTP 头泄露（X-OSS-Meta-*）日志与版本风险访问日志泄露（/logs/、/accesslog/）、版本控制文件泄露（S3/OSS 特有）路径穿越检测目录遍历漏洞（../../etc/passwd 等敏感路径访问）报告生成支持 Text/JSON/HTML 三种输出格式，HTML 报告可视化展示风险等级多线程扫描可配置扫描线程数，提升批量 Bucket 检测效率兼容性支持 Windows/macOS/Linux，自动适配不同厂商存储桶 URL 格式## 基础命令（扫描阿里云 Bucket）  
  
```
# 扫描阿里云Bucket，生成HTML报告
python main.py \
  --cloud aliyun \
  --region ap-southeast-1 \
  --bucket 你的测试Bucket名称 \
  --output html \
  --output-file ./reports/aliyun_scan.html \
  --progress
```  
```
# 输入阿里云hostid-url，自动识别bucket和region，并生成HTML报告
python main.py --cloud aliyun --hostid-url http://bucket.oss-region.aliyuns.com --output html --output-file ./reports/aliyun_scan.html
```  
  
### 批量扫描（从文件读取 Bucket 列表）  
  
```
# 从bucket_list.txt读取Bucket，多线程扫描（10线程）
python main.py \
  --cloud tencent \
  --region ap-guangzhou \
  --bucket-file ./bucket_list.txt \
  --thread 10 \
  --output json \
  --output-file ./reports/tencent_batch_scan.json
```  
  
漏洞检测范围  
<table><thead><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">漏洞类型</span></section></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">风险等级</span></section></th><th style="box-sizing: border-box;padding: 6px 13px;font-weight: 600;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">检测逻辑</span></section></th></tr></thead><tbody><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">敏感密钥文件泄露</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">严重</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">扫描 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">.env</span></code><span leaf="">/</span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">id_rsa</span></code><span leaf="">/</span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">access_key.txt</span></code><span leaf=""> 等路径，判断是否可匿名访问</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">数据库备份泄露</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">严重</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">检测 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">backup/*.sql</span></code><span leaf="">/</span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">db.bak</span></code><span leaf=""> 等备份文件，判断是否可下载</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">匿名 PUT 上传</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">严重</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">尝试上传随机测试文件，返回 200/201 则判定漏洞存在（上传后自动清理）</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">匿名 DELETE 权限</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">严重</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">先上传测试文件，再尝试删除，返回 200/204 则判定漏洞存在</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">公开可列目录</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">高危</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">访问 Bucket 根目录，判断是否返回 XML 格式的文件列表</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">访问日志泄露</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">高危</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">检测 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">/logs/</span></code><span leaf="">/</span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">/accesslog/</span></code><span leaf=""> 等常见日志路径，判断是否可访问</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">CORS 配置过宽</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">中危</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">发送 OPTIONS 请求，判断是否允许 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">Origin: *</span></code><span leaf=""> 且支持 PUT/POST 方法</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(246, 248, 250);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">目录遍历</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">中危</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">尝试访问 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">../../etc/passwd</span></code><span leaf=""> 等路径，判断是否返回敏感文件内容</span></section></td></tr><tr style="box-sizing: border-box;background-color: rgb(255, 255, 255);border-top: 0.727273px solid rgba(209, 217, 224, 0.7);"><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">敏感 HTTP 头泄露</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">低危</span></section></td><td style="box-sizing: border-box;padding: 6px 13px;border-color: rgb(209, 217, 224);border-style: solid;border-width: 0.727273px;border-image: none 100% / 1 / 0 stretch;"><section><span leaf="">检查响应头是否包含 </span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">X-OSS-Meta-*</span></code><span leaf="">/</span><code style="box-sizing: border-box;font-family: &#34;Monaspace Neon&#34;, ui-monospace, SFMono-Regular, &#34;SF Mono&#34;, Menlo, Consolas, &#34;Liberation Mono&#34;, monospace;font-size: 13.6px;tab-size: 4;padding: 0.2em 0.4em;margin: 0px;white-space: break-spaces;background-color: rgba(129, 139, 152, 0.12);border-radius: 6px;"><span leaf="">X-Amz-Storage-Class</span></code><span leaf=""> 等敏感信息</span></section></td></tr></tbody></table>  
  
**相关地址**  
  
**关注微信公众号后台回复“入群”，即可进入星落安全交流群！**  
  
关注微信公众号后台回复“  
20260304  
**”，即可获取项目下载地址！**  
  
****  
  
****  
**圈子介绍**  
  
博主介绍  
：  
  
  
目前工作在某安全公司攻防实验室，一线攻击队选手。自2022-2024年总计参加过30+次省/市级攻防演练，擅长工具开发、免杀、代码审计、信息收集、内网渗透等安全技术。  
  
  
目前已经更新的免杀内容：  
- 部分免杀项目源代码  
  
- 星落安全内部免杀工具箱1.2.1  
  
- GoCobaltStrike星落专版2.0  
  
- 一键击溃windows defender  
  
- 一键击溃火绒进程  
  
-    
CobaltStrike免杀加载器  
  
- 数据库直连工具免杀版  
  
- aspx文件自动上线cobaltbrike  
  
- jsp文件  
自动上线cobaltbrike  
  
- 哥斯拉免杀工具   
XlByPassGodzilla  
  
- 冰蝎免杀工具 XlByPassBehinder  
  
- 冰蝎星落专版 xlbehinder  
  
- 正向代理工具 xleoreg  
  
- 反向代理工具xlfrc  
  
- 内网扫描工具 xlscan  
  
- Todesk/向日葵密码读取工具  
  
- 导出lsass内存工具 xlrls  
  
- 绕过WAF免杀工具 ByPassWAF  
  
- 等等...  
  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/DWntM1sE7icZvkNdicBYEs6uicWp0yXACpt25KZIiciaY7ceKVwuzibYLSoup8ib3Aghm4KviaLyknWsYwTHv3euItxyCQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=9 "")  
  
  
目前星球已满1100人，价格由208元  
调整为  
218元(  
交个朋友啦  
)，1200名以后涨价至268元。  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/rlSBJ0flllk2esLaDRsI4yjB0HkCibHzialJBFBfcyeib4RRsQTOiamqSvAfZogia7pIcSY9lvfTicWXuTcCgtu3NP1w/640?wx_fmt=jpeg "")  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/MuoJjD4x9x3siaaGcOb598S56dSGAkNBwpF7IKjfj1vFmfagbF6iaiceKY4RGibdwBzJyeLS59NlowRF39EPwSCbeQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=11 "")  
  
     
往期推荐  
     
  
  
1.[加量不加价 | 星落免杀第二期，助你打造专属免杀武器库](https://mp.weixin.qq.com/s?__biz=MzkwNjczOTQwOA==&mid=2247495969&idx=1&sn=d3379e8f69c2cefb6d0564299e13d579&scene=21#wechat_redirect)  
  
  
  
2.[【干货】你不得不学习的内网渗透手法](https://mp.weixin.qq.com/s?__biz=MzkwNjczOTQwOA==&mid=2247489483&idx=1&sn=0cbeb449e56db1ae48abfb924ffd0b43&scene=21#wechat_redirect)  
  
  
  
3.[新增全新Web UI版本，操作与管理全面升级 | GoCobalt Strike 2.0正式发布！](https://mp.weixin.qq.com/s?__biz=MzkwNjczOTQwOA==&mid=2247497899&idx=1&sn=018f02ef4064930cbcb40d6b0495e136&scene=21#wechat_redirect)  
  
  
  
4.[【免杀】原来SQL注入也可以绕过杀软执行shellcode上线CoblatStrike](http://mp.weixin.qq.com/s?__biz=MzkwNjczOTQwOA==&mid=2247489950&idx=1&sn=a54e05e31a2970950ad47800606c80ff&chksm=c0e2b221f7953b37b5d7b1a8e259a440c1ee7127d535b2c24a5c6c2f2e773ac2a4df43a55696&scene=21&token=458856676&lang=zh_CN#wechat_redirect)  
  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/DWntM1sE7icZvkNdicBYEs6uicWp0yXACpt25KZIiciaY7ceKVwuzibYLSoup8ib3Aghm4KviaLyknWsYwTHv3euItxyCQ/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp#imgIndex=12 "")  
  
  
