#  金融SRC各场景漏洞挖掘技巧  
hyyrent  道一安全   2026-01-06 01:01  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWjKYvoSviaiaDUIGf1pH9H1bpSJRC3lIk08f5m6yibtkLDhFQwmCXicNMLFniaRrN0Xqvth9XWMQkn5bGQ/640?wx_fmt=gif "")  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**免责声明**  
  
道一安全（本公众号）的技术文章仅供参考，此文所提供的信息只为网络安全人员对自己所负责的网站、服务器等（包括但不限于）进行检测或维护参考，未经授权请勿利用文章中的技术资料对任何计算机系统进行入侵操作。利用此文所提供的信息而造成的直接或间接后果和损失，均由使用者本人负责。本文所提供的工具仅用于学习，禁止用于其他！！！  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**前言**  
  
挖掘金融类漏洞的核心不仅仅是技术点本身，更需要深入理解业务链路、资金流转规则、风控策略与账户体系，从而在“设计缺陷”中找到突破点。本文总结梳理常见的金融逻辑漏洞类型及关键节点的可利用点，帮助安全人员深入理解这些场景，快速定位高价值逻辑漏洞大大提升漏洞挖掘效率和准确度，减少资损信息泄露等高危问题的发生。  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**注册开户场景**  
  
首先我们来了解一下开户场景的大概流程  
  
![image-20251120165143882](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7qaJaRacPSKGRFXPgj7BpTicVsMETlpFQgwjcYo779t6pJklRqXvP23Q/640?wx_fmt=png&from=appmsg "")  
#### 绕过信息校验开户  
  
正常开户申请一般需要手机号+邮箱验证作为必填项，但往往只是在前端进行校验，符合正则格式则通过校验（手机号、邮箱、身份证号）  
  
![image-20251120163408862](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg73KDomLGKZE7j0JNfhFnvIiaViahiarV88RFVLHpibibfGObQzp4Bl2VgkYA/640?wx_fmt=png&from=appmsg "")  
  
由于传过来的字段存在可选填项（如推荐码等），后端往往不会硬性要求全部字段都取，后端只会拿传过来的部分，这时候去掉手机相关字段仍可以正常开户  
  
![image-20251120163517392](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7N2v22fFdCTkSrDFUSF3nmnsBfnU8nWUXMHYosQeLH4Jxf2BKhyR2Tg/640?wx_fmt=png&from=appmsg "")  
#### KYC信息复用伪造  
  
攻击者可利用已认证的同一套资料（如身份证正反面、人脸信息）去注册第二个账户  
  
![image-20251203160050564](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7ymfthShDwMPB4o7KTjt4ebmHeLNuygWfFTsyIInnicd8PBomr8ZOleQ/640?wx_fmt=png&from=appmsg "")  
  
正常上传证件信息，会对图片进行统一化格式处理，然后sdk加签存到数据库中，然后调用ocr对图片进行信息提取再去比对校验  
  
如下图为上传证件信息的api接口，会返回一个加密的filekey文件名  
  
![image-20251203155952891](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7ZStXSwoiaqM5q8nIicpRrJqo14HrX3tBnsOXoJzkjv1VvyZKx6G3vSDg/640?wx_fmt=png&from=appmsg "")  
  
很多时候后端为了节省资源开销，会优先调用缓存数据库结果，判断是否已经上传过，这时候就会导致一个证件信息可以开多个账户  
#### 申请状态查询越权  
  
我们渗透时候可以找到“申请/状态查询”相关接口或页面，修改post参数重放请求（id、orderNo、ticketId、userId等），观察响应是否返回不同用户的数据或敏感字段，然后利用自动化脚本/爆破可预测 ID  
  
  
![image.png](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7Nmzg236DxE0ZdkklOAlFvDgoVmhD5Vs9icWsmXiacTX2pN3zw8m4CQHQ/640?wx_fmt=png&from=appmsg "")  
  
如下面这个例子存在 request_id  
  
![image-20251204110252131](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7pKcZDWJ3PoUeJOPgedicK4gSJTU15ccV1fSrAt1yMP5cyxudmX401MA/640?wx_fmt=png&from=appmsg "")  
  
可通过遍历request_id  
获取其他用户手机号、身份证和银行卡信息  
  
![image-20251204110932960](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7licpMzC3sGmmsG6cqSJUkBNtQrm2EmQdfbZIRKh62rMkibRf31lu1pdQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**支付场景**  
#### 高并发下单/提现  
  
后端没有加分布式锁，会导致重复下单 / 重复提现，攻击者可使用burp一秒内提交多次订单，连续发起多笔提现  
  
![image-20251201114520985](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7XubfMLrHVRJSh0M3nnk8UX7ic6pwxiba6XBxYoNSaYn8scpvdlL8wvVw/640?wx_fmt=png&from=appmsg "")  
  
成功创建多笔提现订单  
  
![image.png](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7cO5HEcDwdos03LDNRIeHyONwAJJXDiaHyrjqjHEmH8dL7FtKRR0PclA/640?wx_fmt=png&from=appmsg "")  
#### 负值反冲  
  
在支付或退款接口中，如果传入的金额参数为负数，且后端没有严格校验金额必须为正，可能导致用户账户余额不减反增  
  
金融类转账严格要求不能为负数，当修改金额为负数时就会导致负值反冲  
  
![image-20251120110520581](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7qPqoUsNMuZsYjib4q6b4rbJkFMadwUic9icibibCBjzjgCUmp7l18Wzsf0Q/640?wx_fmt=png&from=appmsg "")  
#### int64 导致金额溢出  
  
金融系统常见操作：  
- 金额 × 精度（比如 ×10000 或 ×100000）  
  
- 金额求和（累计 risk amount）  
  
- transfer/settlement 金额偏大（企业/跨境/贷款/还款）  
  
如果攻击者传入一个极大的数值，可能导致系统计算时溢出，变成一个负数或一个很小的数  
```
repayAmount := precision.ConvertAmountByBase(    bo.RepayAmount.String(),    eaBo.MoneyMultiBase,)userName, email := f.getUserInfo(ctx, userInfo)now := time.Now()provisionExpireTime := f.getProvisionExpireTime(ctx, bo.ChannelId, bo.UserId, bo.PlatformUserId, now)paymentExpireTime := f.getPaymentExpireTime(ctx, bo.ChannelId, now)
```  
  
常见于后端使用 int64 处理金额，int64 的范围有限（只有 9.22e18），金额乘倍率（比如 ×100000）后很容易超过这个范围，一旦超过CPU 会直接把高位截掉，并且go不会自动检查溢出，传参过大会导致 price / amount 可被改为负数、0.01 等  
  
![image-20251118153559079](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7lUIqW6ubzAbmTCicJBpJgIrUer4sNIwQsv7eawWzG6JrutOTQEGibdvg/640?wx_fmt=png&from=appmsg "")  
  
![image-20251118153520380](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7ibSxaSB3ZYoVgNpHliaEiazYjJ32tQRO7YvicccQe2icKdSOu9Mna3fMwicA/640?wx_fmt=png&from=appmsg "")  
#### 篡改参数免手续费进行转账  
  
在涉及外币或虚拟币的交易中，攻击者尝试修改接口中传输参数，使最终结算金额显著降低  
  
![image-20251120110702048](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7zQvmSrsaoY33mXiaUicYpYb2TicvWwibVE5t9aj7v8QicrWzS0DauWo4tNw/640?wx_fmt=png&from=appmsg "")  
  
计算最终金额的时候，会有很多参数参与运算，如渠道、汇率等，这时候可以尝试纂改相关参数免除手续费  
  
![image-20251128150611909](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7rnId0XmHzVHAp0qqW2kH9CuwOJDbykOmRqUgSQTYUpQLT7N4ntyeiag/640?wx_fmt=png&from=appmsg "")  
  
转账时如果传入xx_id值为0的情况下，使后端 ampaign_result.code == 0 , 导致可以实现无手续费进行转账  
```
if remittanceTicketReq.GetFeeWaivedAmount() != 0 {if remittanceTicketReq.GetCampaignId() == "" {        logger.Error(ctx, format: "invalid_fee_xxx_id")return common.ErrWalletInvalidRequestParam    }}
```  
  
![image-20251201115257442](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7DvWVMNVSLCTqQRaOWzSVOCMLGDDc1y1K8ajqjIZQb5qgujvLib06aJw/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**优惠券场景**  
  
#### 优惠券码爆破  
  
优惠券码未采用加密，并且没配置网关限流策略，导致可以高并发遍历  
  
![image-20251201111453629](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7xTYRzzB3Ssia89WHsptxubjG22a3u4xicma1InRItskNObKfh1ibictHQQ/640?wx_fmt=png&from=appmsg "")  
  
通过对voucher_code进行遍历，根据回显字段长度不同判断优惠券是否存在，如下图存在为820、819，不存在为798  
  
![image-20251201111530424](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7J73ylGo5VJlOpxMInc1vXj5ibZCibVSyE7ZuqaQRheQ3HQ0vBheYs6kQ/640?wx_fmt=png&from=appmsg "")  
#### 优惠券无锁限制重复领取  
  
优惠券核销时，系统未严格校验该券的唯一使用记录或总使用次数  
  
加锁安全的情况  
```
func applyCouponWithLock(amount float64) float64 {    coupon.mu.Lock()         // 加锁    defer coupon.mu.Unlock() // 解锁    discounted := amount - coupon.Discountif discounted < 0 {        discounted = 0    }    coupon.UsageCount++return discounted}
```  
  
后端未加锁，导致可以重复领取  
```
func applyCoupon(amount float64) float64 {    discounted := amount - coupon.Discountif discounted < 0 {        discounted = 0    }    atomic.AddInt64(&coupon.UsageCount, 1)return discounted}
```  
#### 优惠券叠加使用  
  
系统允许用户在单笔订单中，同时使用多张原本设计为互斥或限用一张的优惠券  
  
![image-20251204105300720](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7YIicYrKGqcH74uyFVdP9cuOxbG4HxrVWVAnRc59Zm3SHe3ca8ul2TGQ/640?wx_fmt=png&from=appmsg "")  
  
正常单张使用  
  
![image-20251201113637619](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7abyf5iaLZnVpweBRhH5oCcMZ371A0Ec2xLfWznTgJ3cBZ3UKhSicibNcw/640?wx_fmt=png&from=appmsg "")  
  
多张优惠券叠加使用  
  
![image-20251201113756324](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7Uoic1JgmlXQLTcSnmnmpKaM8HZOsOhibkhIoq03ZiaQ9Tickyu1auqpf1Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**信息查询场景**  
#### 越权查询他人敏感信息  
  
金融类对越权漏洞是十分严格的，像一般的越权查看个人信息、订单，是渗透测试的必测项目  
  
![image-20251119165630215](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7EcGB2wLH6XOeQPUiaGZxngCIuVpoiaNONOdJuveO6gMN67dK4HiarnLsg/640?wx_fmt=png&from=appmsg "")  
  
例如用户 A 在查询自己的信息时，通过修改请求参数为用户 B 的订单 ID，成功查询到用户 B 的账单信息  
  
![image-20251204104318708](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7stz5zY3EvuoYlrEfhY0cFvUdicuAoicBvXEQaJcZYicgxCIAEdforHCCA/640?wx_fmt=png&from=appmsg "")  
  
![image-20251204104229848](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg75SuiaiaPrkicasDzY8yTgauU2keBP710eUKnXAJCNe2A3QXOD58rM1iagA/640?wx_fmt=png&from=appmsg "")  
  
稍微复杂一些的隐藏参数情况  
  
![image-20251203163346295](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7XSVjq5FYWMraVEx3Wyj7KF4hurbmAaOxAV2qibxXgzzN0ZVH34DUmvA/640?wx_fmt=png&from=appmsg "")  
  
函数实现是优先从post传参userid去查询的，然后再去解token提取uid进行查询  
  
但是正常调用的时候，post传入的是{}，因此走到第二优先级的解token提取uid进行查询，那我们该如何去挖掘这类的漏洞呢，我们可以通过搜集history返回包中的参数组成字典去fuzz，如下图返回包中有 userphone、uid、role  
等字段  
  
![image-20251203162838679](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7wCLqFz0HZngjjibkGvsHIiaibRf7gK5exPFlHRgZqSCsA4frvmiasZOWHQ/640?wx_fmt=png&from=appmsg "")  
  
通过fuzz发现uid可作为查询参数，越权查询他人的信息  
  
![image-20251203163817197](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7tTxCccfHsdA5xlIQ6y9M4SKTFK3xyu2wlrCgOW15cpsCWWKrUyRPIQ/640?wx_fmt=png&from=appmsg "")  
  
![image-20251203163851509](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7T394Bw1j8Fv3j70dMOOzk8eibkAvtFYQSHQbOlnTBdolZkFcDCh8noA/640?wx_fmt=png&from=appmsg "")  
#### Toc&ToB网关配置错误接口混用  
  
在 gRPC + Gateway 架构里，越权问题通常发生在普通用户能调用原本只允许管理员的 RPC 方法。  
- 生成的 HTTP 路由没有绑定权限检查，任何人都能访问  
  
- Gateway 没加角色校验，或者拦截器配置错误，token 可以被忽略或者被默认当作 admin  
  
假设admin管理端域名为a.com，toc用户端域名为b.com，通过burp history导出一份a.com  
  
![image-20241202145003839](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7VibcZ913tpZicxwAuXfhhMFltqkCvHsFtzOoVDHHG1oNP7SAb344Hu0w/640?wx_fmt=png&from=appmsg "")  
  
右键选中 save item，导出对应的xml文件  
  
![image-20241202145302261](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7aGGaDW8WEZic6F9shGyJu48IrF1Zd3icws4FO88zM2dAib73MzD2Imx8w/640?wx_fmt=png&from=appmsg "")  
  
导出文件格式还需要做进一步的处理：  
1. **Base64 解码**  
：  
  
1. 将 XML 中的 <request>  
 元素的内容进行 Base64 解码，解码得到完整的 HTTP 请求内容。  
  
1. **解析请求**  
：  
  
1. 根据 HTTP 请求格式，第一行包含了请求方法和路径，例如：POST /api/login HTTP/1.1，提取 /api/login  
  
1. **提取 POST 数据**  
：  
  
1. 通过检查是否遇到 HTTP 请求头结束标志 \r\n\r\n  
，来提取请求体  
  
1. **写入 CSV 文件**  
：  
  
1. 将提取到的请求方法、API 路径和 POST 数据写入到 CSV 文件中  
  
导出文件命名为 burp_history.xml  
，导出文件为 burp_history.csv  
  
脚本文件  
```
import osimport xml.etree.ElementTree as ETimport csvimport base64input_file = "burp_history.xml"output_file = "burp_history.csv"if os.path.exists(output_file):try:        os.rename(output_file, output_file)    except PermissionError:print(f"文件 {output_file} 正在被占用，请关闭相关程序后重试。")exit(1)tree = ET.parse(input_file)root = tree.getroot()try:    with open(output_file, mode="w", newline="", encoding="utf-8") as csvfile:        csv_writer = csv.writer(csvfile)        csv_writer.writerow(["Method", "API Endpoint", "POST Content"])for item in root.findall('./item'):            request_element = item.find('request')if request_element is not None and request_element.text:try:                    request_data = base64.b64decode(request_element.text).decode("utf-8", errors="ignore")                except Exceptionas e:print(f"Base64 解码失败: {e}")continue                header_body_split = request_data.split("\r\n\r\n", 1)if len(header_body_split) == 2:                    headers = header_body_split[0]                    body = header_body_split[1].strip()                    header_lines = headers.split("\r\n")if header_lines:                        first_line = header_lines[0].strip()                        parts = first_line.split(" ")if len(parts) >= 2:                            method = parts[0].upper()                            path = parts[1]else:                            method = "UNKNOWN"                            path = first_line                        post_content = body if method == "POST"else""                        csv_writer.writerow([method, path, post_content])else:                    csv_writer.writerow(["ERROR", "INVALID REQUEST", ""])else:                csv_writer.writerow(["ERROR", "EMPTY REQUEST", ""])print(f"已成功将数据整理为 {output_file}")except PermissionError:print(f"无法创建或写入文件 {output_file}，请检查权限。")except Exceptionas e:print(f"发生意外错误：{e}")
```  
  
处理效果  
  
![image-20251202154135953](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7yUQ0IBhicajGiajll5Qzs4bMCoAOfHdUJGJbBgn2Dl35h2Kib2PeFO9rg/640?wx_fmt=png&from=appmsg "")  
  
intruder测试选择 Pitchfork  
 模式进行发包  
  
![image-20241202152747911](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7DOpia89Z2N5BhzREmBDxkDiaTdo3o1BDTdKHW0DqWBiaBEN6FibLRCHnDg/640?wx_fmt=png&from=appmsg "")  
  
payload encoding  
 取消勾选  
  
![image-20241202153846692](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg73SGia3fyicE3QVIy19vdPbKFXuKk3SnhPQaqfpY67fWlfWklFcMwHlyA/640?wx_fmt=png&from=appmsg "")  
  
开始测试  
  
![image-20251202154105230](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7VXeuXibYdakW9VKjxRsuib4uM87ZJnutGIc55yOgucibFrFLUa3JlgVnQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**资源存储场景**  
  
#### 合同资料遍历获取  
  
合同、发票等敏感文件存储在一个可预测的路径下（如 /docs/contract/2025/ID_0001.pdf  
）  
  
![](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7vOUtWr6t01dwedIUg8R4eZBGlJ1quZXavn8cZrbMmlCKTvbN7icAIOQ/640?wx_fmt=png&from=appmsg "")  
  
攻击者通过爆破或递增 ID 的方式，可批量下载其他用户的合同文件  
  
![image.png](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7WsNna7Ej4OLFMJYiaAQXPQqFKStJnX6JSiabr2t9laH4gibDz2a5DVkXQ/640?wx_fmt=png&from=appmsg "")  
#### S3存储桶配置不当/泄露  
  
许多金融机构使用云存储服务（如 AWS S3 或阿里云 OSS）来存储文件。如果存储桶 Bucket 的权限配置错误（如设置为 Public Read），可能导致所有存储的资料对外公开。  
  
https://github.com/dark-kingA/cloudTools  
  
![image-20251118153909154](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7eoqQVs9icYvVw3Sn8icR99UbMlWVW35IGGeb5WpBGqgtK1EZUmmuL0Lw/640?wx_fmt=png&from=appmsg "")  
  
![image-20251118154002622](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7W3bqSbzskJC7rQ4DI7dv26B5TNV9jHPq3grXMJWBhDUPJUTX0cMNxw/640?wx_fmt=png&from=appmsg "")  
#### SSRF及绕过  
  
服务端存在可发起外部网络请求的接口（如图片加载、URL 抓取）。攻击者利用此接口，让金融服务器作为跳板，访问其内网敏感资源或未授权的云服务 API。  
  
![image-20251118182235376](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7dwIKN4dCazOQk5nw3Yr3kHbhRYFrmbLwlt7ribfBzcbsVozHLf9zWxg/640?wx_fmt=png&from=appmsg "")  
  
但通常企业内部会有白名单域名，当我们请求的url匹配则可以绕过检测  
  
![image-20251118184618667](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7yKibIEIs3UoicaCpls5gLJuTnwGpibMeIdvk72813iaXP82V1PWXL9w24A/640?wx_fmt=png&from=appmsg "")  
  
成功触发dnslog  
  
![image-20251201153958537](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7JDKtiaWwRoWkPFBz7OFYFK9tu2ZySJn9rWZ7qPVWGN8YtWXn2uiaGd9g/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**使用第三方厂商开发**  
#### JWT/密码硬编码未修改  
  
某盘系统中，系统初始化的工程中，定义了默认值的appid  
和jwttoken  
加密密钥  
  
![image-20251201160325500](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7ethNlZCibnNib99UrRcQ4ohWDY07Rbcoz8ZC9WdvBsMMsNVaMFviaJMNQ/640?wx_fmt=png&from=appmsg "")  
  
导致攻击者可以利用该密钥伪造任意用户的 JWT Token，从而绕过身份验证机制  
  
![image-20251201154518199](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg77QCRG3PSX1ywjaM6CmlyQdIR7AM7j5OaLluO2eWc8H4ACqToN4fhpA/640?wx_fmt=png&from=appmsg "")  
#### 运维后门  
  
第三方运维有时候为了方便更新维护，会在内部脚本、后端代码中直接引用外部 URL 来获取密码，例如：  
```
curl http://password.example.com/db-prod-passwget http://x.x.x.x/getKey?env=prod
```  
  
这种设计虽然便于统一管理，但等同于主动为攻击者开放后门接口  
  
![image-20251204104557490](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7r8hx3vA7buTkGkhDZAakqRc16JKQrL1254snJlicFqJjia1GUbqmsbhQ/640?wx_fmt=png&from=appmsg "")  
  
下面就是从SSO外部接口获取最新系统密码的实战例子  
  
![image-20251204104631703](https://mmbiz.qpic.cn/mmbiz_png/WAyrRuvrubEiccMI2f4ib2w2yIS6Q6fXg7KrIOB33v49Gibzich61p92KWrcPJH9tTBEflRAMpU1WAaYu37a3XDf3w/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWgJHGxRSfVlI02pBf15B0slPyoWRWfSP0mM3LqDQKhOhVwfvVKma68JRwQ7E2Oysib3Nsw5ny7uaSw/640?wx_fmt=gif "")  
  
**来源题**  
  
奇安信攻防社区：  
https://forum.butian.net/share/4671  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjK9Euejia8GQQAicPWkJI7HfpDplIlc3tPr73ZYKHIdg9kIHpWaJia2tGA/640?wx_fmt=gif "")  
  
**点分享**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjXjW9bUCoUia7g4iaVGGGm5AKWRMoDMQoFDdJuiceofhPJ8SJpKSGToZcw/640?wx_fmt=gif "")  
  
**点收藏**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjAEe2Bq3UgWlgxribzfYtnQ6EVkxkao5qmK0xpaoycfHyGVl7zFicPGibw/640?wx_fmt=gif "")  
  
**点在看**  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/Ljib4So7yuWiaHpokNh4uWxia9Vv2eYjfzjDia9eCL6sIvuL17F5uKHsjx0GNc6estct1jOfWh4EtOcVsvzynOar1Q/640?wx_fmt=gif "")  
  
**点点赞**  
  
  
