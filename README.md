AutoHotkey 特点介绍、编写经验及使用心得等。如果喜欢，请分享到微博、微信等。

## 博客相关说明

本项目派生自 [Yonsm.NET](https://github.com/Yonsm/NET)，在此基础上做了下列调整：<br />
最重要的一点是，派生本项目并拉到本地后无需安装 Jekyll 等诸多工具（在 Windows 中安装对普通用户较困难，此时无法本地预览），直接新建 md 文件编辑后上传即可（熟悉 md 语法后预览功能纯属多余）。

* 在主页添加“Fork me on the GitHub”彩带
* 在页面右侧增加百度分享
* 修改文章下的评论插件：从多说改为友言
* 修正了页面底部“Powered by Jekyll @ GitHub”中的绝对链接并提取为变量，可直接在配置中设置
* 增加了语法高亮插件 pygments 所需文件 pygments.css，不过对于 ahk 高亮样式有点丑
* 高亮语法为 {% highlight ahk %}、{% endhighlight %}。
* 调整源文件结构：[从命名文件夹改为命名 HTML 文件](http://jekyllcn.com/docs/pages/)
* 把[到内部资源（如图片或其他文件）的链接使用 {{ site.url }} 表示](http://jekyllcn.com/docs/posts/#section-3)，而不使用相对链接或具体网址的绝对链接
* [文章内链使用 post_url 变量表示](http://jekyllcn.com/docs/templates/#post-url)
* 把引用的谷歌网络字体库替换为国内的缓存，如将 Google 免费字体库的域名 http://fonts.googleapis.com 修改为：http://fonts.useso.com 即可

* [文章内链](http://jekyllcn.com/docs/templates/)（`2010-07-21-name-of-post` 不用包括扩展名），构建失败，加上 _posts/ 路径依然，为什么？目前仍使用 site.url 引用
	* THML：`{% post_url 2010-07-21-name-of-post %}`
	* MD：`[Name of link]({% post_url 2010-07-21-name-of-post %})`
* 图片：`![备用图片提示文本]({{ site.url }}/assets/images/20140720000.png)`
* 其他文件：`[备用资源提示文本]({{ site.url }}/assets/downloads/12345.pdf)`

<br />

## 一些注意事项

* 图片和下载的文件分别放在 `assets` 目录的 `images` 和 `downloads` 中
* 引用文件时使用站点根目录的绝对路径，如 `![有帮助的截图]({{ site.url }}/assets/screenshot.jpg)`
* 调整每篇文章下方的”上一篇“和”下一篇“到右边对齐
* 能否使用 page.id 代替 page.thread？page.thread 好像只是多说在使用，目前改为友言已无必要。
* 使用分页器变量重新设置文章底部的页面导航
* 每次编辑时，语法高亮最后设置，对于简单的编辑直接在网页中进行而不本地编辑
* 每次本地修改前首先 git pull 从服务器版本库获取更新，本地修改后上传（以防在线修改的丢失）

## 关于代码高亮
**注意：必须符合当前配置文件中的相关设置，具有相应的 js 和 css 文件并在相应文件中包含。**  
相关说明：[Jekyll高亮的另一个选择:JS高亮](http://blog.mangege.com/2012/10/18/jekyllgao-liang-de-ling-yi-ge-xuan-ze-jsgao-liang.html)  
若使用 rdiscount 引擎，虽可识别围栏格式的代码，但仅高亮 `highlight ahk` 格式代码（这里的识别和高亮都需在 md 转换为 html 时才能实现，所以下面的代码既无法被识别也无法高亮）：

{% highlight ahk %}
; Normal comment
/*
Block comment
*/

; Directives, keywords
#SingleInstance Force
#NoTrayIcon

; Command, literal text, escape sequence
MsgBox, Hello World `; This isn't a comment

; Operators
Bar = Foo  ; operators
Foo := Bar ; expression assignment operators

; String
Var := "This is a test"

; Number
Num = 40 + 4

; Dereferencing
Foo := %Bar%

; Flow of control, built-in-variables, BIV dereferencing
if true
	MsgBox, This will always be displayed
Loop, 3
	MsgBox Repetition #%A_Index%

; Built-in function call
MsgBox % SubStr("blaHello Worldbla", 4, 11)

if false
{
	; Keys and buttons
	Send, {F1}
	; Syntax errors
	MyVar = "This is a test
}

ExitApp

; Label, hotkey, hotstring
Label:
#v::MsgBox You pressed Win+V
::btw::by the way
{% endhighlight %}

若使用 redcarpet 引擎，可高亮两种形式的代码（高亮的效果相同），包括 fenced_code_blocks（这里的识别由 redcarpet 实现，所以下面的代码可识别，但高亮必须转换到 html 才能看到效果）：  
```autohotkey
; Normal comment
/*
Block comment
*/

; Directives, keywords
#SingleInstance Force
#NoTrayIcon

; Command, literal text, escape sequence
MsgBox, Hello World `; This isn't a comment

; Operators
Bar = Foo  ; operators
Foo := Bar ; expression assignment operators

; String
Var := "This is a test"

; Number
Num = 40 + 4

; Dereferencing
Foo := %Bar%

; Flow of control, built-in-variables, BIV dereferencing
if true
	MsgBox, This will always be displayed
Loop, 3
	MsgBox Repetition #%A_Index%

; Built-in function call
MsgBox % SubStr("blaHello Worldbla", 4, 11)

if false
{
	; Keys and buttons
	Send, {F1}
	; Syntax errors
	MyVar = "This is a test
}

ExitApp

; Label, hotkey, hotstring
Label:
#v::MsgBox You pressed Win+V
::btw::by the way
```
但目前只能使用 autohotkey 关键字（注意是全部小写），不能使用 ahk 或其他大小写形式（调整了高亮 js 中的代码，没成功）。

## 在 github 中使用 markdown 格式的注意事项

* 引用格式需在行末加两个空格才不会合并，但双层或多层引用则不需要  
* 列表格式前后需各空一行才能识别，内嵌的列表需用 tab 缩进
* 在高亮代码或 HTML 标记中的 md 标记无效

