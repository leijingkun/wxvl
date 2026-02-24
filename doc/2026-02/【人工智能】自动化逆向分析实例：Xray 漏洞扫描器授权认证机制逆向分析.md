#  【人工智能】自动化逆向分析实例：Xray 漏洞扫描器授权认证机制逆向分析  
利刃信安
                    利刃信安  利刃信安   2026-02-24 05:18  
  
   
  
# 自动化逆向分析实例：Xray 漏洞扫描器授权认证机制逆向分析  
> **声明**  
: 本文仅供安全研究和学习目的，请勿用于非法用途。未经授权绕过软件授权机制可能违反相关法律法规。  
  
## 1. 概述  
### 1.1 研究目标  
  
Xray 是一款由长亭科技开发的知名 Web 漏洞扫描工具，本文对其授权认证机制进行深入逆向分析，研究其安全设计并提出可能的绕过方案。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ZaibroIiatwe14EShDtMiaaLd9fN45kxdux0E9UCaiaibEibvgVL98l9p1WH3O3XG9uaAVSuUEmBsmQK83liaweO8xsHOXro7srZxYmnH1MlnkhoyQ/640?wx_fmt=png&from=appmsg "")  
### 1.2 分析环境  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">项目</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">配置</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">操作系统</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Windows 10/11 x64</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">分析工具</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">IDA Pro 7.7+</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">调试器</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">x64dbg</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">编程语言</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Python 3.x, Go 1.17</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">目标文件</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">xray_windows_amd64.exe v1.9.11</span></section></td></tr></tbody></table>  
## 2. 二进制文件基本信息  
### 2.1 文件属性  
```
┌────────────────────────────────────────────────────────────┐
│                    文件基本信息                              │
├────────────────────────────────────────────────────────────┤
│  文件名:     xray_windows_amd64.exe                        │
│  文件类型:   PE (Portable Executable for AMD64)            │
│  架构:       x64                                           │
│  基址:       0x400000                                      │
│  编程语言:   Go (Golang)                                    │
│  Go 版本:    go1.17                                        │
│  程序版本:   1.9.11                                        │
│  构建日期:   2023-05-18                                    │
│  保护机制:   UPX 加壳                                       │
└────────────────────────────────────────────────────────────┘
```  
### 2.2 UPX 脱壳  
  
原始文件经过 UPX 加壳处理，需要先脱壳才能进行深入分析：  
```
# 检查加壳状态
upx -t xray_windows_amd64.exe

# 脱壳处理
upx -d xray_windows_amd64.exe -o xray_unpacked.exe
```  
  
**脱壳前后对比:**  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">状态</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">文件大小</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">函数数量</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">字符串可见性</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">加壳前</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">~30 MB</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">24</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">混淆</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">脱壳后</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">~68 MB</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">100,000+</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">清晰</span></section></td></tr></tbody></table>  
## 3. 授权认证机制分析  
### 3.1 整体架构  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        Xray 授权认证系统架构                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                 │
│  │ 许可证文件   │    │  程序主体   │    │  功能模块   │                 │
│  │ .lic        │───▶│  验证逻辑   │───▶│  权限控制   │                 │
│  └─────────────┘    └─────────────┘    └─────────────┘                 │
│         │                  │                  │                         │
│         ▼                  ▼                  ▼                         │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐                 │
│  │ Base64签名  │    │ RSA验证     │    │ 功能开关    │                 │
│  │ AES加密数据 │    │ 时间校验    │    │ 版本限制    │                 │
│  └─────────────┘    └─────────────┘    └─────────────┘                 │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 3.2 验证流程时序图  
```
┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐
│  用户   │     │  程序   │     │许可证文件│     │ RSA验证 │     │ 功能层  │
└────┬────┘     └────┬────┘     └────┬────┘     └────┬────┘     └────┬────┘
     │               │               │               │               │
     │  启动程序     │               │               │               │
     │──────────────▶│               │               │               │
     │               │               │               │               │
     │               │  读取许可证   │               │               │
     │               │──────────────▶│               │               │
     │               │               │               │               │
     │               │  返回文件内容 │               │               │
     │               │◀──────────────│               │               │
     │               │               │               │               │
     │               │  解析注释行   │               │               │
     │               │──────────────┐│               │               │
     │               │              ││               │               │
     │               │  Base64解码  │               │               │
     │               │◀─────────────┘│               │               │
     │               │               │               │               │
     │               │  AES解密签名  │               │               │
     │               │──────────────┐│               │               │
     │               │              ││               │               │
     │               │  XOR 0x44    │               │               │
     │               │◀─────────────┘│               │               │
     │               │               │               │               │
     │               │  RSA-PSS验证  │               │               │
     │               │──────────────────────────────▶│               │
     │               │               │               │               │
     │               │               │     验证结果  │               │
     │               │◀──────────────────────────────│               │
     │               │               │               │               │
     │               │  时间有效性检查              │               │
     │               │──────────────┐│               │               │
     │               │              ││               │               │
     │               │  版本类型判断│               │               │
     │               │◀─────────────┘│               │               │
     │               │               │               │               │
     │               │  启用对应功能 │               │               │
     │               │──────────────────────────────────────────────▶│
     │               │               │               │               │
     │  程序就绪     │               │               │               │
     │◀──────────────│               │               │               │
     │               │               │               │               │
```  
## 4. 许可证文件结构  
### 4.1 文件格式  
  
许可证文件采用简单的文本格式，包含元数据和签名两部分：  
```
┌────────────────────────────────────────────────────────────┐
│                    许可证文件结构                           │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  ┌──────────────────────────────────────────────────────┐ │
│  │                    元数据区域                         │ │
│  │  # xray license                                       │ │
│  │  # user_name: Mannix                                  │ │
│  │  # user_id: 05401157dde901314d34ba8e848dc5b2         │ │
│  │  # license_id: 1d7a9cec586e3b266f5dae9c68fd787c       │ │
│  │  # distribution: COMMUNITY-ADVANCED                   │ │
│  │  # not_valid_before: 2021-08-03                       │ │
│  │  # not_valid_after: 2022-08-03                        │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                            │
│  ┌──────────────────────────────────────────────────────┐ │
│  │                    签名区域                           │ │
│  │  [空行]                                               │ │
│  │  AkTaFrP8/RnAYMjNKusPfREn2lGBVHf9gUkWnh/CR+UI...     │ │
│  │  (Base64 编码的签名数据)                              │ │
│  └──────────────────────────────────────────────────────┘ │
│                                                            │
└────────────────────────────────────────────────────────────┘
```  
### 4.2 字段说明  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">字段</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">类型</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">说明</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">示例</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">user_name</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">string</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">用户名</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Mannix</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">user_id</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">string</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">用户ID (MD5)</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">05401157dde901314d34ba8e848dc5b2</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">license_id</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">string</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">许可证ID (MD5)</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">1d7a9cec586e3b266f5dae9c68fd787c</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">distribution</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">string</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">版本类型</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">COMMUNITY-ADVANCED</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">not_valid_before</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">date</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">生效日期</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">2021-08-03</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">not_valid_after</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">date</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">失效日期</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">2022-08-03</span></section></td></tr></tbody></table>  
### 4.3 版本类型  
```
┌─────────────────────────────────────────────────────────────┐
│                     版本类型对比                             │
├─────────────────┬─────────────┬─────────────┬───────────────┤
│     功能        │  COMMUNITY  │  ADVANCED   │ PROFESSIONAL  │
├─────────────────┼─────────────┼─────────────┼───────────────┤
│ 基础漏洞扫描    │     ✅      │     ✅      │      ✅       │
│ 代理扫描        │     ✅      │     ✅      │      ✅       │
│ 反连平台        │     ✅      │     ✅      │      ✅       │
│ 子域名扫描      │     ❌      │     ✅      │      ✅       │
│ 高级POC插件     │     ❌      │     ✅      │      ✅       │
│ API安全测试     │     ❌      │     ❌      │      ✅       │
│ 优先技术支持    │     ❌      │     ❌      │      ✅       │
├─────────────────┼─────────────┼─────────────┼───────────────┤
│     价格        │    免费     │    付费     │     高价      │
└─────────────────┴─────────────┴─────────────┴───────────────┘
```  
## 5. 加密算法分析  
### 5.1 签名数据结构  
```
┌────────────────────────────────────────────────────────────┐
│                    签名数据结构                             │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  Base64 解码后:                                            │
│  ┌────────┬────────────────┬────────────────────────────┐ │
│  │ 版本号  │     AES IV     │    AES 加密的数据          │ │
│  │ 1 byte │   16 bytes     │        n bytes            │ │
│  │  0x02  │  随机生成      │  JSON + RSA签名            │ │
│  └────────┴────────────────┴────────────────────────────┘ │
│                                                            │
│  AES 解密后:                                               │
│  ┌────────────────┬────────────────┬────────────────────┐ │
│  │   长度前缀     │    JSON数据    │     RSA签名        │ │
│  │   2 bytes     │    变长        │     512 bytes      │ │
│  └────────────────┴────────────────┴────────────────────┘ │
│                                                            │
└────────────────────────────────────────────────────────────┘
```  
### 5.2 加密流程  
```
┌─────────────────────────────────────────────────────────────┐
│                      加密流程图                              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│  │ JSON数据 │───▶│ XOR 0x44 │───▶│AES-128   │              │
│  │          │    │  混淆    │    │CBC加密   │              │
│  └──────────┘    └──────────┘    └──────────┘              │
│                                        │                    │
│                                        ▼                    │
│  ┌──────────┐    ┌──────────┐    ┌──────────┐              │
│  │ 最终签名 │◀───│ Base64   │◀───│ 添加版本 │              │
│  │   数据   │    │  编码    │    │ 号和IV   │              │
│  └──────────┘    └──────────┘    └──────────┘              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```  
### 5.3 RSA 公钥  
  
程序中硬编码的 RSA 公钥：  
```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA2K+PRbp/yumhaVnN92JS
GuQiwj7df64jHAo8MvXLWjYxU/yvqB4LbGty8ymKQy33qaDNpu9jgE2s8cXrtftm
/UcvwDb8sTqWXpDhxYhcvJM30agxz3/8VwNJ4JOvhk9Gn+msYIUz+gXZMBuUFKhi
BOd6C2Pro03GYwVTNjfwH/Y9C5EfPKIKNU/5t2cYo+TuOBk5ooP+NTaDzB6rb7fd
E5uuNnF21x3rdiI9rZcKPbuU97/0OWNcIUh5wfxPNWwcmjYmFuZcxk/7dOUD65s4
pTplCoMLOelacB0l442dM4w2xNpn+Yg7i/ujmg37F+VguCZJWnoyImdhp/raccNG
+wIDAQAB
-----END PUBLIC KEY-----
```  
  
**公钥参数:**  
- • 算法: RSA  
  
- • 密钥长度: 2048 位  
  
- • 公钥指数: 65537 (0x10001)  
  
- • 签名方案: RSA-PSS with SHA-256  
  
### 5.4 算法汇总  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">算法</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">密钥长度</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">用途</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">RSA</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">2048 bit</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">签名验证</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">RSA-PSS</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">-</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">签名方案</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">SHA-256</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">256 bit</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">哈希计算</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">AES-128-CBC</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">128 bit</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">数据加密</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Base64</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">-</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">编码</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">XOR</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">8 bit</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">简单混淆</span></section></td></tr></tbody></table>  
## 6. 关键函数逆向  
### 6.1 函数调用关系  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        关键函数调用关系图                                │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  gunkit_core_assassin_license.LoadLicense (0x1327840)                  │
│         │                                                               │
│         ▼                                                               │
│  gunkit_core_assassin_utils_selfcrypto.LoadLicense (0x13097C0)         │
│         │                                                               │
│         ▼                                                               │
│  gunkit_core_assassin_utils_selfcrypto.parseLicenseContent (0x1308DE0) │
│         │                                                               │
│         ├──────────────────────────┐                                    │
│         ▼                          ▼                                    │
│  GetRSAPublicKey (0x13086E0)   parseLicenseContent.func1 (0x1309D60)   │
│         │                          │                                    │
│         ▼                          ▼                                    │
│  crypto/x509.ParsePKIXPublicKey  crypto/rsa.VerifyPSS                  │
│                                        │                                │
│                                        ▼                                │
│                              License.IsValid (0x1308840)                │
│                                        │                                │
│                                        ▼                                │
│                                   time.Now()                            │
│                                   时间比较验证                           │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 6.2 核心验证函数  
#### 6.2.1 IsValid 函数 (0x1308840)  
```
// 伪代码还原
func (l *License) IsValid() (bool, string) {
    now := time.Now()
    
    // 检查许可证是否已生效
    if l.NotValidBefore > now.Unix() {
        return false, fmt.Sprintf("license not valid until %s", 
            l.GetTimestampStr(l.NotValidBefore))
    }
    
    // 检查许可证是否已过期
    if l.NotValidAfter < now.Unix() {
        return false, fmt.Sprintf("license expired at %s", 
            l.GetTimestampStr(l.NotValidAfter))
    }
    
    // 检查许可证是否即将过期 (5年内)
    futureTime := now.AddDate(5, 0, 0)
    if l.NotValidAfter < futureTime.Unix() {
        // 特殊处理逻辑
        if regexp.MatchString(`[a-f0-9]{32}`, l.ID) {
            return false, "invalid license id"
        }
        return true, ""
    }
    
    return true, ""
}
```  
#### 6.2.2 RSA 签名验证函数 (0x1309D60)  
```
// 伪代码还原
func verifySignature(data []byte, signature []byte, publicKey *rsa.PublicKey) bool {
    // 计算 SHA256 哈希
    hash := sha256.Sum256(data)
    
    // RSA-PSS 签名验证
    err := rsa.VerifyPSS(publicKey, crypto.SHA256, hash[:], signature, nil)
    
    return err == nil
}
```  
### 6.3 关键函数地址表  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">函数名</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">地址</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">功能</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">GetRSAPublicKey</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x13086E0</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">获取硬编码的 RSA 公钥</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">License.IsValid</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x1308840</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">验证许可证有效性</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">License.String</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x1308B60</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">格式化许可证信息</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">parseLicenseContent</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x1308DE0</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">解析许可证内容</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">LoadLicense</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x13097C0</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">加载许可证文件</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">parseLicenseContent.func1</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x1309D60</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">RSA-PSS 签名验证</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">LoadLicense.func1</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><code style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-feature-settings: normal;font-variation-settings: normal;font-size: 12.6px;text-align: left;line-height: 1.75;color: rgb(221, 17, 68);background: rgba(27, 31, 35, 0.05);padding: 3px 5px;border-radius: 4px;"><span leaf="">0x1309E80</span></code></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">许可证加载回调</span></section></td></tr></tbody></table>  
## 7. 授权绕过方案  
### 7.1 方案总览  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        授权绕过方案总览                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      方案一: 二进制补丁                          │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │ IsValid函数 │  │ VerifyPSS   │  │ 时间检查    │             │   │
│  │  │ 直接返回true│  │ 跳过验证    │  │ 跳转修改    │             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      方案二: 公钥替换                            │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │ 生成新密钥对│  │ 替换程序公钥│  │ 签名新许可证│             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      方案三: 内存补丁                            │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │ DLL注入     │  │ Frida Hook  │  │ 调试器修改  │             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      方案四: 时间欺骗                            │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │ 系统时间修改│  │ Hook时间函数│  │ 虚拟机快照  │             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 7.2 方案一: 二进制补丁  
#### 7.2.1 IsValid 函数补丁  
  
**目标地址:**  
 0x1308840  
  
**原始汇编:**  
```
1308840  lea     r12, [rsp+var_60]
1308845  cmp     r12, [r14+10h]
1308849  jbe     loc_1308B35
130884f  sub     rsp, 0E0h
...
```  
  
**补丁方案:**  
```
; 方案 A: 直接返回成功
1308840  mov     eax, 1         ; B8 01 00 00 00
1308845  xor     ebx, ebx       ; 31 DB
1308847  xor     ecx, ecx       ; 31 C9
1308849  ret                    ; C3

; 方案 B: 跳过时间验证
13088D9  jmp     loc_1308A4A    ; E9 6C 01 00 00 (跳转到成功返回)
13088E6  jmp     loc_1308A4A    ; E9 5F 01 00 00
```  
  
**补丁字节码:**  
```
地址: 0x1308840
数据: B8 01 00 00 00 31 DB 31 C9 C3
说明: mov eax, 1; xor ebx, ebx; xor ecx, ecx; ret
```  
#### 7.2.2 VerifyPSS 函数补丁  
  
**目标地址:**  
 0x1309D60  
  
**原始汇编:**  
```
1309E0B  call    crypto_rsa_VerifyPSS
1309E10  test    rax, rax
1309E13  setz    al
```  
  
**补丁方案:**  
```
; 跳过签名验证，直接返回成功
1309E0B  xor     eax, eax       ; 31 C0
1309E0D  nop                    ; 90
1309E0E  nop                    ; 90
1309E0F  nop                    ; 90
1309E10  test    rax, rax       ; 保持原有逻辑
1309E13  setz    al             ; al = 1 (成功)
```  
  
**补丁字节码:**  
```
地址: 0x1309E0B
数据: 31 C0 90 90 90
说明: xor eax, eax; nop x3
```  
### 7.3 方案二: 公钥替换  
#### 7.3.1 实施步骤  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        公钥替换攻击流程                                  │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  步骤1: 生成新的 RSA 密钥对                                              │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  $ openssl genrsa -out private.pem 2048                         │   │
│  │  $ openssl rsa -in private.pem -pubout -out public.pem          │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                              │                                          │
│                              ▼                                          │
│  步骤2: 定位程序中的公钥位置                                             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  地址: 0x1DCC012                                                │   │
│  │  长度: 451 字节                                                  │   │
│  │  格式: PEM (-----BEGIN PUBLIC KEY----- ... -----END...)         │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                              │                                          │
│                              ▼                                          │
│  步骤3: 替换公钥并修复 PE 结构                                           │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  - 使用十六进制编辑器替换公钥                                    │   │
│  │  - 保持相同长度或填充空白                                        │   │
│  │  - 修复 PE 校验和                                                │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                              │                                          │
│                              ▼                                          │
│  步骤4: 使用私钥签名新许可证                                             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  - 构造许可证 JSON 数据                                          │   │
│  │  - 使用私钥进行 RSA-PSS 签名                                     │   │
│  │  - AES 加密 + Base64 编码                                        │   │
│  │  - 生成新的 .lic 文件                                            │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
#### 7.3.2 公钥位置信息  
```
┌────────────────────────────────────────────────────────────┐
│                    公钥存储位置                             │
├────────────────────────────────────────────────────────────┤
│  数据段地址: 0x1DCC012                                      │
│  引用位置:   off_43CBA90 (指针)                             │
│  长度变量:   qword_43CBA98 = 0x1C3 (451 字节)              │
│                                                            │
│  引用函数:                                                 │
│  - GetRSAPublicKey (0x13086E0)                             │
│  - parseLicenseContent (0x1308DE0)                         │
│  - verifyFile (0x131D42E)                                  │
└────────────────────────────────────────────────────────────┘
```  
### 7.4 方案三: 内存补丁  
#### 7.4.1 DLL 注入方案  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        DLL 注入内存补丁流程                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌──────────────┐     ┌──────────────┐     ┌──────────────┐           │
│  │  xray.exe    │     │  注入器      │     │  补丁DLL     │           │
│  │              │     │              │     │              │           │
│  └──────┬───────┘     └──────┬───────┘     └──────┬───────┘           │
│         │                    │                    │                    │
│         │   1.启动进程       │                    │                    │
│         │◀───────────────────│                    │                    │
│         │                    │                    │                    │
│         │   2.注入DLL        │                    │                    │
│         │◀───────────────────│                    │                    │
│         │                    │                    │                    │
│         │   3.DLL初始化      │                    │                    │
│         │────────────────────────────────────────▶│                    │
│         │                    │                    │                    │
│         │   4.修改内存       │                    │                    │
│         │◀────────────────────────────────────────│                    │
│         │                    │                    │                    │
│         │   5.正常运行       │                    │                    │
│         │   (已绕过验证)     │                    │                    │
│         │                    │                    │                    │
└─────────────────────────────────────────────────────────────────────────┘
```  
#### 7.4.2 Frida Hook 方案  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        Frida Hook 流程                                   │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  1. 启动 Frida 服务                                               │  │
│  │     $ frida-server &                                              │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                              │                                          │
│                              ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  2. 注入 Hook 脚本                                                │  │
│  │     $ frida -p <pid> -l hook.js                                   │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                              │                                          │
│                              ▼                                          │
│  ┌──────────────────────────────────────────────────────────────────┐  │
│  │  3. Hook 效果                                                     │  │
│  │     - 拦截 IsValid 函数调用                                       │  │
│  │     - 强制返回 true                                               │  │
│  │     - 程序正常运行                                                │  │
│  └──────────────────────────────────────────────────────────────────┘  │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 7.5 方案对比  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">方案</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">难度</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">可行性</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">持久性</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">检测风险</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">推荐度</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">二进制补丁 IsValid</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">✅ 高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">中等</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">中等</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐⭐</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">二进制补丁 VerifyPSS</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">✅ 高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">中等</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">中等</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">公钥替换</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⚠️ 中</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">低</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">DLL 注入</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">✅ 高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">低</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Frida Hook</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">✅ 高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">低</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">高</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">时间欺骗</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⚠️ 中</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">低</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">低</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐</span></section></td></tr></tbody></table>  
## 8. 绕过利用代码  
### 8.1 二进制补丁工具  
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""Xray License Bypass - Binary Patcher用于对 xray_windows_amd64.exe 进行二进制补丁"""

import os
import sys
import struct
import hashlib
from pathlib import Path

class XrayPatcher:
    """Xray 二进制补丁工具"""
    
    # 补丁配置
    PATCHES = {
        'IsValid': {
            'address': 0x1308840,
            'original': bytes([0x4C, 0x8D, 0x64, 0x24, 0x98, 0x49, 0x3B, 0x66, 0x10]),
            'patch': bytes([0xB8, 0x01, 0x00, 0x00, 0x00, 0x31, 0xDB, 0x31, 0xC9, 0xC3]),
            'description': 'IsValid: 直接返回 true'
        },
        'VerifyPSS': {
            'address': 0x1309E0B,
            'original': bytes([0xE8, 0x50, 0x2D, 0x0E, 0x00]),  # call 指令
            'patch': bytes([0x31, 0xC0, 0x90, 0x90, 0x90]),  # xor eax,eax; nop x3
            'description': 'VerifyPSS: 跳过签名验证'
        }
    }
    
    def __init__(self, filepath: str):
        self.filepath = Path(filepath)
        self.backup_path = self.filepath.with_suffix('.exe.bak')
        self.data = None
        
    def read_file(self) -> bool:
        """读取文件内容"""
        try:
            with open(self.filepath, 'rb') as f:
                self.data = bytearray(f.read())
            print(f"[+] 读取文件成功: {self.filepath}")
            print(f"[+] 文件大小: {len(self.data):,} 字节")
            return True
        except Exception as e:
            print(f"[-] 读取文件失败: {e}")
            return False
    
    def backup_file(self) -> bool:
        """备份原始文件"""
        try:
            with open(self.backup_path, 'wb') as f:
                f.write(self.data)
            print(f"[+] 备份文件成功: {self.backup_path}")
            return True
        except Exception as e:
            print(f"[-] 备份文件失败: {e}")
            return False
    
    def verify_original(self, patch_name: str) -> bool:
        """验证原始字节码"""
        patch_info = self.PATCHES[patch_name]
        addr = patch_info['address']
        original = patch_info['original']
        
        current_bytes = bytes(self.data[addr:addr + len(original)])
        
        if current_bytes == original:
            print(f"[+] {patch_name}: 原始字节码验证通过")
            return True
        else:
            print(f"[-] {patch_name}: 原始字节码不匹配")
            print(f"    期望: {original.hex()}")
            print(f"    实际: {current_bytes.hex()}")
            return False
    
    def apply_patch(self, patch_name: str) -> bool:
        """应用补丁"""
        patch_info = self.PATCHES[patch_name]
        addr = patch_info['address']
        patch = patch_info['patch']
        
        for i, byte in enumerate(patch):
            self.data[addr + i] = byte
        
        print(f"[+] {patch_name}: 补丁应用成功")
        print(f"    地址: 0x{addr:X}")
        print(f"    数据: {patch.hex()}")
        return True
    
    def write_file(self) -> bool:
        """写入修改后的文件"""
        try:
            with open(self.filepath, 'wb') as f:
                f.write(self.data)
            print(f"[+] 写入文件成功: {self.filepath}")
            return True
        except Exception as e:
            print(f"[-] 写入文件失败: {e}")
            return False
    
    def calculate_hash(self) -> str:
        """计算文件哈希"""
        return hashlib.sha256(self.data).hexdigest()[:16]
    
    def patch(self, patches: list = None) -> bool:
        """执行补丁流程"""
        if patches is None:
            patches = list(self.PATCHES.keys())
        
        print("\n" + "=" * 60)
        print("Xray License Bypass - Binary Patcher")
        print("=" * 60)
        
        # 读取文件
        if not self.read_file():
            return False
        
        original_hash = self.calculate_hash()
        print(f"[+] 原始文件哈希: {original_hash}")
        
        # 备份文件
        if not self.backup_file():
            return False
        
        # 应用补丁
        print("\n[*] 开始应用补丁...")
        for patch_name in patches:
            if patch_name not in self.PATCHES:
                print(f"[-] 未知补丁: {patch_name}")
                continue
            
            print(f"\n[*] 处理补丁: {patch_name}")
            print(f"    描述: {self.PATCHES[patch_name]['description']}")
            
            if self.verify_original(patch_name):
                self.apply_patch(patch_name)
        
        # 写入文件
        print("\n[*] 保存修改...")
        if not self.write_file():
            return False
        
        new_hash = self.calculate_hash()
        print(f"[+] 新文件哈希: {new_hash}")
        
        print("\n" + "=" * 60)
        print("[+] 补丁完成!")
        print("=" * 60)
        return True


def main():
    if len(sys.argv) < 2:
        print("用法: python patcher.py <xray_windows_amd64.exe>")
        print("示例: python patcher.py xray_windows_amd64.exe")
        sys.exit(1)
    
    filepath = sys.argv[1]
    
    if not os.path.exists(filepath):
        print(f"[-] 文件不存在: {filepath}")
        sys.exit(1)
    
    patcher = XrayPatcher(filepath)
    success = patcher.patch()
    
    sys.exit(0 if success else 1)


if __name__ == '__main__':
    main()
```  
### 8.2 DLL 注入补丁  
```
/* * Xray License Bypass - DLL Injection * 编译: x86_64-w64-mingw32-gcc -shared -o patch.dll patch.c */

#include <windows.h>
#include <stdio.h>

// 补丁配置
#define ISVALID_ADDR    0x1308840
#define VERIFYPSS_ADDR  0x1309E0B

// 原始字节码
static BYTE g_originalIsValid[] = { 0x4C, 0x8D, 0x64, 0x24, 0x98 };
static BYTE g_originalVerifyPSS[] = { 0xE8, 0x50, 0x2D, 0x0E, 0x00 };

// 补丁字节码
static BYTE g_patchIsValid[] = { 0xB8, 0x01, 0x00, 0x00, 0x00, 0x31, 0xDB, 0x31, 0xC9, 0xC3 };
static BYTE g_patchVerifyPSS[] = { 0x31, 0xC0, 0x90, 0x90, 0x90 };

// 应用补丁
BOOL ApplyPatch(LPVOID address, LPBYTE patch, SIZE_T size) {
    DWORD oldProtect;
    
    // 修改内存保护
    if (!VirtualProtect(address, size, PAGE_EXECUTE_READWRITE, &oldProtect)) {
        printf("[-] VirtualProtect failed: %lu\n", GetLastError());
        return FALSE;
    }
    
    // 写入补丁
    memcpy(address, patch, size);
    
    // 恢复内存保护
    VirtualProtect(address, size, oldProtect, &oldProtect);
    
    return TRUE;
}

// DLL 入口点
BOOL APIENTRY DllMain(HMODULE hModule, DWORD reason, LPVOID lpReserved) {
    switch (reason) {
        case DLL_PROCESS_ATTACH:
            // 禁用线程通知
            DisableThreadLibraryCalls(hModule);
            
            printf("[*] Xray License Bypass DLL Loaded\n");
            printf("[*] Process ID: %lu\n", GetCurrentProcessId());
            
            // 获取模块基址
            HMODULE hModule = GetModuleHandleA(NULL);
            if (hModule == NULL) {
                printf("[-] GetModuleHandle failed\n");
                return FALSE;
            }
            
            LPVOID baseAddr = (LPVOID)hModule;
            printf("[+] Module base: %p\n", baseAddr);
            
            // 计算实际地址
            LPVOID isValidAddr = (LPVOID)((DWORD_PTR)baseAddr + ISVALID_ADDR - 0x400000);
            LPVOID verifyPSSAddr = (LPVOID)((DWORD_PTR)baseAddr + VERIFYPSS_ADDR - 0x400000);
            
            printf("[+] IsValid address: %p\n", isValidAddr);
            printf("[+] VerifyPSS address: %p\n", verifyPSSAddr);
            
            // 应用 IsValid 补丁
            printf("\n[*] Patching IsValid...\n");
            if (ApplyPatch(isValidAddr, g_patchIsValid, sizeof(g_patchIsValid))) {
                printf("[+] IsValid patched successfully\n");
            }
            
            // 应用 VerifyPSS 补丁
            printf("\n[*] Patching VerifyPSS...\n");
            if (ApplyPatch(verifyPSSAddr, g_patchVerifyPSS, sizeof(g_patchVerifyPSS))) {
                printf("[+] VerifyPSS patched successfully\n");
            }
            
            printf("\n[+] All patches applied!\n");
            break;
            
        case DLL_PROCESS_DETACH:
            printf("[*] DLL unloaded\n");
            break;
    }
    
    return TRUE;
}
```  
### 8.3 Frida Hook 脚本  
```
/** * Xray License Bypass - Frida Hook Script * 用法: frida -p <pid> -l hook.js */

// 配置
const CONFIG = {
    moduleName: 'xray_windows_amd64.exe',
    baseOffset: 0x400000,
    functions: {
        IsValid: 0x1308840,
        VerifyPSS: 0x1309D60,
        LoadLicense: 0x13097C0
    }
};

// 获取模块基址
function getBaseAddress() {
    const module = Process.findModuleByName(CONFIG.moduleName);
    if (module) {
        console.log(`[+] Module: ${module.name}`);
        console.log(`[+] Base: ${module.base}`);
        console.log(`[+] Size: ${module.size}`);
        return module.base;
    }
    return null;
}

// 计算实际地址
function getRealAddress(base, offset) {
    return base.add(offset - CONFIG.baseOffset);
}

// Hook IsValid 函数
function hookIsValid(base) {
    const addr = getRealAddress(base, CONFIG.functions.IsValid);
    console.log(`\n[*] Hooking IsValid at ${addr}`);
    
    Interceptor.attach(addr, {
        onEnter: function(args) {
            console.log('\n[IsValid] Called');
            // 可以在这里打印许可证信息
        },
        onLeave: function(retval) {
            console.log(`[IsValid] Original return: ${retval}`);
            // 强制返回 true
            retval.replace(1);
            console.log(`[IsValid] Modified return: ${retval}`);
        }
    });
}

// Hook VerifyPSS 函数
function hookVerifyPSS(base) {
    const addr = getRealAddress(base, CONFIG.functions.VerifyPSS);
    console.log(`\n[*] Hooking VerifyPSS at ${addr}`);
    
    Interceptor.attach(addr, {
        onEnter: function(args) {
            console.log('\n[VerifyPSS] Called');
            // 保存上下文
            this.context = this.context;
        },
        onLeave: function(retval) {
            console.log(`[VerifyPSS] Original return: ${retval}`);
            // 强制返回 true (验证成功)
            retval.replace(1);
            console.log(`[VerifyPSS] Modified return: ${retval}`);
        }
    });
}

// 内存补丁 - 直接修改函数
function patchIsValid(base) {
    const addr = getRealAddress(base, CONFIG.functions.IsValid);
    console.log(`\n[*] Patching IsValid at ${addr}`);
    
    // 补丁代码: mov eax, 1; ret
    const patch = [0xB8, 0x01, 0x00, 0x00, 0x00, 0xC3];
    
    Memory.protect(addr, patch.length, 'rwx');
    Memory.writeByteArray(addr, patch);
    
    console.log('[+] IsValid patched: always returns true');
}

// 主函数
function main() {
    console.log('='.repeat(60));
    console.log('Xray License Bypass - Frida Hook');
    console.log('='.repeat(60));
    
    const base = getBaseAddress();
    if (!base) {
        console.log('[-] Failed to find module');
        return;
    }
    
    // 选择方案
    console.log('\n[*] Available bypass methods:');
    console.log('    1. Hook functions (runtime)');
    console.log('    2. Memory patch (permanent)');
    
    // 方案1: Hook 函数
    hookIsValid(base);
    hookVerifyPSS(base);
    
    // 方案2: 内存补丁 (取消注释使用)
    // patchIsValid(base);
    
    console.log('\n[+] Hooks installed successfully!');
    console.log('[*] Waiting for license verification...');
}

// 执行
Java.perform(main);
```  
### 8.4 许可证生成器 (需要私钥)  
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""Xray License Generator需要拥有 RSA 私钥才能生成有效许可证"""

import json
import base64
import hashlib
from datetime import datetime, timedelta
from cryptography.hazmat.primitives import hashes, serialization
from cryptography.hazmat.primitives.asymmetric import rsa, padding
from cryptography.hazmat.backends import default_backend
from Crypto.Cipher import AES
import struct

class LicenseGenerator:
    """Xray 许可证生成器"""
    
    def __init__(self, private_key_pem: str):
        """初始化，加载私钥"""
        self.private_key = serialization.load_pem_private_key(
            private_key_pem.encode(),
            password=None,
            backend=default_backend()
        )
        self.aes_key = self._derive_aes_key()
    
    def _derive_aes_key(self) -> bytes:
        """派生 AES 密钥 (需要逆向分析获取具体算法)"""
        # 这里需要分析程序中的 AES 密钥生成逻辑
        # 示例: 使用固定密钥或某种派生算法
        return b'\x00' * 16  # 占位符
    
    def generate_license(self,                          user_name: str,                         distribution: str = "ADVANCED",                         valid_days: int = 365) -> str:
        """生成许可证"""
        
        # 生成 ID
        user_id = hashlib.md5(user_name.encode()).hexdigest()
        license_id = hashlib.md5(f"{user_name}{datetime.now()}".encode()).hexdigest()
        
        # 计算有效期
        now = datetime.now()
        not_valid_before = now.strftime("%Y-%m-%d")
        not_valid_after = (now + timedelta(days=valid_days)).strftime("%Y-%m-%d")
        
        # 构造许可证数据
        license_data = {
            "user_name": user_name,
            "user_id": user_id,
            "license_id": license_id,
            "distribution": distribution,
            "not_valid_before": not_valid_before,
            "not_valid_after": not_valid_after
        }
        
        # JSON 序列化
        json_data = json.dumps(license_data, separators=(',', ':'))
        
        # RSA-PSS 签名
        signature = self.private_key.sign(
            json_data.encode(),
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        
        # 组合数据: JSON + 签名
        combined = json_data.encode() + signature
        
        # XOR 混淆
        xored = bytes([b ^ 0x44 for b in combined])
        
        # AES 加密
        iv = os.urandom(16)
        cipher = AES.new(self.aes_key, AES.MODE_CBC, iv)
        # PKCS7 填充
        pad_len = 16 - (len(xored) % 16)
        padded = xored + bytes([pad_len] * pad_len)
        encrypted = cipher.encrypt(padded)
        
        # 组合最终数据: 版本(1) + IV(16) + 加密数据
        final_data = bytes([0x02]) + iv + encrypted
        
        # Base64 编码
        signature_b64 = base64.b64encode(final_data).decode()
        
        # 构造许可证文件
        license_content = f"""# xray license# user_name: {user_name}# user_id: {user_id}# license_id: {license_id}# distribution: {distribution}# not_valid_before: {not_valid_before}# not_valid_after: {not_valid_after}{signature_b64}"""
        
        return license_content
    
    def save_license(self, filepath: str, **kwargs):
        """保存许可证到文件"""
        content = self.generate_license(**kwargs)
        with open(filepath, 'w') as f:
            f.write(content)
        print(f"[+] License saved to: {filepath}")


def main():
    print("=" * 60)
    print("Xray License Generator")
    print("=" * 60)
    print("\n[!] 需要 RSA 私钥才能生成有效许可证")
    print("[!] 私钥由长亭科技保管，无法获取")
    print("\n[*] 此工具仅供学习研究目的")
    
    # 示例用法 (需要私钥)
    # with open('private.pem', 'r') as f:
    #     private_key = f.read()
    # 
    # generator = LicenseGenerator(private_key)
    # generator.save_license(
    #     'xray-license.lic',
    #     user_name='TestUser',
    #     distribution='ADVANCED',
    #     valid_days=365
    # )


if __name__ == '__main__':
    main()
```  
### 8.5 完整利用工具包  
```
xray_bypass_toolkit/
├── README.md
├── requirements.txt
├── patcher.py           # 二进制补丁工具
├── injector.py          # DLL 注入器
├── patch.dll            # 补丁 DLL
├── hook.js              # Frida Hook 脚本
├── license_generator.py # 许可证生成器
└── utils/
    ├── pe_parser.py     # PE 文件解析
    ├── crypto_utils.py  # 加密工具
    └── logger.py        # 日志模块
```  
## 9. 总结与防护建议  
### 9.1 分析总结  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        安全分析总结                                      │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     发现的安全问题                               │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │  1. 验证逻辑过于集中                                             │   │
│  │     - 单一 IsValid 函数控制所有验证                              │   │
│  │     - 补丁一点即可绕过全部验证                                   │   │
│  │                                                                  │   │
│  │  2. 缺乏完整性校验                                               │   │
│  │     - 程序未对自身进行完整性检查                                 │   │
│  │     - 可被轻易修改二进制代码                                     │   │
│  │                                                                  │   │
│  │  3. 公钥硬编码                                                   │   │
│  │     - RSA 公钥直接嵌入程序                                       │   │
│  │     - 可被替换进行公钥攻击                                       │   │
│  │                                                                  │   │
│  │  4. 时间验证可预测                                               │   │
│  │     - 使用系统时间进行验证                                       │   │
│  │     - 可通过时间欺骗绕过                                         │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     安全设计优点                                 │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │  1. 使用 RSA-PSS 签名方案                                        │   │
│  │     - 安全的签名算法                                             │   │
│  │     - 无法伪造签名                                               │   │
│  │                                                                  │   │
│  │  2. 多层加密保护                                                 │   │
│  │     - AES + XOR 混淆                                            │   │
│  │     - 增加逆向难度                                               │   │
│  │                                                                  │   │
│  │  3. UPX 加壳                                                     │   │
│  │     - 压缩代码                                                   │   │
│  │     - 增加静态分析难度                                           │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 9.2 防护建议  
```
┌─────────────────────────────────────────────────────────────────────────┐
│                        防护建议                                          │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     代码层面防护                                 │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │                                                                  │   │
│  │  1. 多点验证                                                     │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  在多个关键功能点进行许可证验证                        │    │   │
│  │     │  - 扫描开始前验证                                      │    │   │
│  │     │  - POC 加载时验证                                      │    │   │
│  │     │  - 报告生成时验证                                      │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  │  2. 完整性校验                                                   │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  - 启动时校验程序哈希                                  │    │   │
│  │     │  - 使用数字签名验证                                    │    │   │
│  │     │  - 检测调试器和注入                                    │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  │  3. 代码混淆                                                     │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  - 使用更强的混淆工具                                  │    │   │
│  │     │  - 虚拟化保护关键代码                                  │    │   │
│  │     │  - 反调试、反注入技术                                  │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                     架构层面防护                                 │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │                                                                  │   │
│  │  1. 在线验证                                                     │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  - 定期向服务器验证许可证                              │    │   │
│  │     │  - 服务器控制功能开关                                  │    │   │
│  │     │  - 实时吊销过期许可证                                  │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  │  2. 硬件绑定                                                     │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  - 许可证绑定机器特征                                  │    │   │
│  │     │  - CPU ID, MAC 地址等                                  │    │   │
│  │     │  - 防止许可证共享                                      │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  │  3. 云端功能                                                     │   │
│  │     ┌──────────────────────────────────────────────────────┐    │   │
│  │     │  - 核心功能放在云端                                    │    │   │
│  │     │  - 本地程序仅作为客户端                                │    │   │
│  │     │  - 无法离线使用高级功能                                │    │   │
│  │     └──────────────────────────────────────────────────────┘    │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```  
### 9.3 安全评分  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">评估项目</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">得分</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">说明</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">签名算法安全性</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">RSA-PSS + SHA256，非常安全</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">数据加密保护</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">AES + XOR，多层保护</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">代码保护强度</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">UPX 加壳，一般水平</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">验证逻辑安全性</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">过于集中，易被绕过</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">完整性保护</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">⭐</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">缺乏完整性校验</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><strong style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-weight: bold;text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: inherit;color: rgb(0, 152, 116);"><span leaf="">综合评分</span></strong></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><strong style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-weight: bold;text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: inherit;color: rgb(0, 152, 116);"><span leaf="">⭐⭐⭐</span></strong></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><strong style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);font-weight: bold;text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: inherit;color: rgb(0, 152, 116);"><span leaf="">中等安全水平</span></strong></td></tr></tbody></table>  
## 附录  
### A. 参考链接  
- •   
IDA Pro 官方文档[1]  
  
- •   
Go 语言逆向分析[2]  
  
- •   
RSA-PSS 签名方案[3]  
  
- •   
Frida 官方文档[4]  
  
### B. 工具下载  
  
<table><thead><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">工具</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">用途</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">下载链接</span></section></td></tr></thead><tbody><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">IDA Pro</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">逆向分析</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">https://hex-rays.com/</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">x64dbg</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">动态调试</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">https://x64dbg.com/</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">Frida</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">动态插桩</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">https://frida.re/</span></section></td></tr><tr style="box-sizing: border-box;border-width: 0px;border-style: solid;border-color: rgb(229, 229, 229);"><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">UPX</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">加壳/脱壳</span></section></td><td style="box-sizing: border-box;border: 1px solid rgb(223, 223, 223);text-align: left;line-height: 1.75;font-family: Optima-Regular, Optima, PingFangSC-light, PingFangTC-light, &#34;PingFang SC&#34;, Cambria, Cochin, Georgia, Times, &#34;Times New Roman&#34;, serif;font-size: 14px;padding: 0.5em 1em;color: rgb(63, 63, 63);word-break: keep-all;"><section><span leaf="">https://upx.github.io/</span></section></td></tr></tbody></table>  
### C. 免责声明  
  
本文所述内容仅供安全研究和学习目的。未经授权绕过软件授权机制可能违反软件许可协议和相关法律法规。作者不对任何滥用行为承担责任。请确保在合法授权范围内使用软件。  
  
