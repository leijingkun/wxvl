#  漏洞预警 | jsPDF路径遍历漏洞  
浅安  浅安安全   2026-01-15 00:00  
  
**0x00 漏洞编号**  
- CVE-  
2025-68428  
  
**0x01 危险等级**  
- 高危  
  
**0x02 漏洞概述**  
  
jsPDF是一个用于在客户端生成PDF文件的开源JavaScript库。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/7stTqD182SWpnDM2ib33Iic1oeiaW08az6S30NWOTZsrpMVBal3O0m6ibrT4hcXPqkJuiayjNPr9mc0XW1WK7X8GNGg/640?wx_fmt=png&from=appmsg "")  
  
**0x03 漏洞详情**  
###   
  
**CVE-2025-68428**  
  
**漏洞类型：**  
**路径遍历**  
  
**影响：**  
越权访问  
  
**简述：jsPDF中的loadFile方法存在本地文件包含路径遍历漏洞。攻击者可以通过向该方法传递未经清理的文件路径，导致加载并包含任意本地文件的内容，生成的PDF文件中将包含该文件的内容，从而访问到运行Node进程的文件系统中的敏感信息。**  
  
**0x04 影响版本**  
- jsPDF <= 3.0.4  
  
**0x05****POC状态**  
- 未公开  
  
**0x06****修复建议**  
  
******目前官方已发布漏洞修复版本，建议用户升级到安全版本****：******  
  
https://github.com/parallax/jsPDF  
  
  
  
