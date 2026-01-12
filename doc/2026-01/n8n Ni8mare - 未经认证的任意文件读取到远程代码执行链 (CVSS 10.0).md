#  n8n Ni8mare - 未经认证的任意文件读取到远程代码执行链 (CVSS 10.0)  
 TtTeam   2026-01-12 16:08  
  
未经身份验证的任意文件读取 → 管理员令牌伪造 → 沙箱绕过 → 远程代码执行  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0HlywncJbB3dkFhiaTXyRwG7A7NIUiaRHCDMDc5EpSezMFQOFewt9neAhYHSwAG7cD0ia4ZR42x3NtQHqn9QM88xw/640?wx_fmt=png&from=appmsg "")  
  
n8n 上的完整未经认证的远程代码执行链：  
- CVE-2026-21858 - 内容类型混淆 → 任意文件读取  
  
- 读取配置 + 数据库 → Forge 管理员 JWT  
  
- CVE-2025-68613 - 表达式注入 → 沙箱绕过 → 远程代码执行  
  
攻击链  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0HlywncJbB3dkFhiaTXyRwG7A7NIUiaRHCnLe8XicOYo1rGMnKETOlKRoxuW4keHJDia8LjqoOLNOSVj6z9GrJnWhQ/640?wx_fmt=png&from=appmsg "")  
  
https://github.com/Chocapikk/CVE-2026-21858  
  
