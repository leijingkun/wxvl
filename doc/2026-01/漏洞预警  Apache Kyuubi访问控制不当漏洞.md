#  漏洞预警 | Apache Kyuubi访问控制不当漏洞  
浅安
                    浅安  浅安安全   2026-01-19 00:01  
  
**0x00 漏洞编号**  
- CVE-  
2025-66518  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
Apache Kyuubi是Apache基金会旗下的分布式SQL网关与多租户计算服务平台，主要面向Apache Spark、Flink等大数据计算引擎。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SUXMjseYbjJkzOZuEAZYkLXcgeMH3GWSWTwwoWODpibrXiarXFA34aibJibdlmvPEiaBlYaqicLpPbK03OQ/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-66518**  
  
**漏洞类型：**  
访问控制不当  
  
**影响：**  
泄露敏感信息  
  
**简述：**  
Apache Kyuubi Server存在访问控制不当漏洞，由于服务器端在处理本地路径时缺乏必要的路径规范化校验，攻击者只要能够通过Kyuubi前端协议访问服务，即可绕过kyuubi.session.local.dir.allow.list配置限制，访问或使用未被允许列表包含的本地文件资源。  
  
**0x04 影响版本**  
- 1.6.0 <= Apache Kyuubi <= 1.10.2  
  
**0x05****POC状态**  
- 未公开  
  
**0x06****修复建议**  
  
******目前官方已发布漏洞修复版本，建议用户升级到安全版本****：******  
  
https://kyuubi.apache.org/  
  
  
  
