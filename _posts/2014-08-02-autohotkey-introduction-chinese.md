---
date: 2014-08-02 06:10:54+08:00
layout: post
title: 轻松学会弹指神功
thread: 16
categories: 指南
tags:
---
注：中文应用部分仍需更新。
转者按：本文属于**零基础入门**专题教程，原[发表于 Download!网络密技王第三期](http://www.pcuser.com.tw/bookdetail.aspx?Number=2AS503)，后[转载至简睿随笔](http://jdev.tw/blog/734/autohotkey-introduction-chinese)，作者简睿。本专题选取一些通俗易懂的基础入门教程，经适当调整（以反映目前 AutoHotkey 现状）后集中发表，以方便初次接触脚本的朋友入门（帮助中的[初学者向导](http://ahkcn.github.io/docs/Tutorial.htm)也是很好的入门教程）。注：原文为繁体中文，为方便简体用户，经 [OpenCC](http://code.google.com/p/opencc/) 转换为简体中文，某些用词（主要为计算机术语）可能转换有误，一般不影响理解。

## AutoHotkey 的高度定制功能，让你成为效率高手

如果你分析每天操作电脑时所重复执行的动作——相同的网址、相同的电子信箱、相同的文本输入等等，你会惊讶的发现重复频率之高令人咋舌。如果你想从诸多的重复动作里解脱而出，并且能有效提升操作电脑的效率，那么 AutoHotkey 是你不能错过的工具，善用 AutoHotkey 将让你轻而易举的成为效率高手。
![AutoHotkey_logo]({{ site.url }}/assets/images/AutoHotkey_logo.gif)
软件名称： AutoHotkey  
软件版本：1.1.15.01  
软件大小：2+MB  
软件授权：免费开源  
适用平台：Windows  
语言接口：英文  
官方网站：http://ahkscript.org/  
下载地址：[点击下载](http://ahkscript.org/download/ahk-install.exe)

## AutoHotkey：键盘与鼠标工具

AutoHotkey 顾名思义就是协助你将常用按键自动化的工具，而这些自动化的操作可以由用户依自身的需求来设置，随着设置的项目日渐扩充齐全，AutoHotkey 带给你的便利也日益增多。AutoHotkey 好处多多，只要几个简单步骤安装好，再建立一个写有 AutoHotkey 指令的文本文件（这个文件称为 AutoHotkey 的脚本档）就能开始享受它带来的速度感与便利性。「咦～还要写指令？会不会很困难，我只是会用电脑的用户而已呀！」别怕别怕，不要被「指令」这两个字吓到了，建立脚本档的过程就只是像是打篇文章而已，请耐心的看下去，本文会一步一步地把建立脚本档的步骤清楚、简单的介绍给你，读者们只要依样画葫芦马上就能感受到 AutoHotkey 的强大威力了！

在正式开始之前，先把 AutoHotkey 提供的功能以它本身的术语介绍给你：

* **热键（HotKey）**：
热键也可称为快捷键（Shortcut Key），意指某个按键能执行特定的功能。在 Windows 系统里，〔Win+E〕开启文件总管、〔Win+R〕开启执行窗口是几个常用的两个热键。而 AutoHotkey 的热键功能则让你自行建立专属你个人的快捷键。
虽然 HotKey 的对象似乎只限键盘，但事实上连鼠标的按钮、滚轮与摇杆也都能依你的需要来设置。
* **热字串（HotString）**：
热字串是比照热键而来的名词，有的系统会称呼为「缩写」，指的是输入较短的字串（缩写的关键字）而能自动扩展成较长的文本，例如只要输入「inet」四个英文本母就能自动变成「互联网」，而更令人兴奋的是：所有的热字串都是你自已设置的。
* **操作流程的判断与循环控制**：
如果 AutoHotkey 只具备了键盘与鼠标的自订功能，那它充其量也不过是个键盘工具罢了，但事实上 AutoHotkey 提供了许多指令用来判断诸多事项，具备程序控制能力，因而晋身为巨集（Macro）工具，能依需要再做更细部的处理与控制。我们可以简单地把巨集或脚本视为一种简易、好写的程序，虽说简单但功能可是一点也不马虎。
* **图形接口与脚本合体**：
AutoHotkey 也提供了许多窗口、按钮等图形接口的指令，能让我们很轻易的建立操作用的小窗口，从而提供了更方便与更优越的使用接口，而这些指令都能透过一支 SmartGUI 执行档用拖拉的方式来自动产生。

AutoHotkey 的基本功能介绍完毕，以下进入主题。首先说明 AutoHotkey 的安装步骤。

## AutoHotkey 下载与安装

3. 使用 IE 或 FireFox 等浏览器进入[下载页](http://ahkscript.org/download/)，点击「Installer」开始下载，当「文件下载」对话盒出现后，选取你常用的工具文件夹，把文件存入此文件夹。（注：该安装包已包含了后面几个链接中的文件，那些链接适合老用户。）在页面下方还有前往中文帮助的下载链接。
![AutoHotkey 下载页面]({{ site.url }}/assets/images/20140802000.png)
3. 下载后的「AutoHotkey111501_Install.exe」就是安装执行档，请双击此文件以执行安装步骤（若已安装过旧版本，则会提示直接更新或自定义更新）。
![AutoHotkey 安装界面1]({{ site.url }}/assets/images/20140801000.png)
 * 典型安装：使用默认选项安装到默认位置中，建议新用户选择，之后请直接跳到第六步
 * 自定义安装：可在后续对话框中选择各选项、安装位置等，适合老用户，点击后进入下一页
 * 你也可以把它装到 USB 随身碟里以增进可携性，即点击右下角“extract to...”并指定目标目录直接提取压缩文件。
3. 构建选择：
 * Unicode 32-bit：没有特殊情况都推荐使用。
 * Unicode 64-bit：在 64 位系统中性能更强，仅在 64 位系统中安装时才会出现。
 * ANSI 32-bit：与某些旧脚本（主要指 AutoHotkey Basic）的兼容性更好。
![AutoHotkey 安装界面2]({{ site.url }}/assets/images/20140802002.png)
3. 指定安装的文件夹，缺省是「C:\Program Files\AutoHotkey」，还可以选择不创建开始菜单中的快捷方式，最后按〔Next〕。
![AutoHotkey 安装界面3]({{ site.url }}/assets/images/20140802003.png)
3. 接着选取要安装的类型，如果想安装脚本编译器的话，则保持【Install script compiler】的勾选状态；想启动 .ahk 文件的拖拉功能的话，把【Enable drag & drop】选项勾选起来，最后按〔Install〕。
![AutoHotkey 安装界面4]({{ site.url }}/assets/images/20140802004.png)
3. 文件解压缩并复制后即告全部安装结束。此时可以打开帮助文档或看看本版本的更新内容或运行 AutoHotkey，也可以点击“Exit”直接退出。
![AutoHotkey 安装界面5]({{ site.url }}/assets/images/20140801001.png)

AutoHotkey 安装完成后不必重新启动电脑，尔后扩展名 .ahk 会自动关联到 AutoHotkey.exe，只要双击扩展名为 .ahk 的文件就能启动 AutoHotkey 来读取该文件的内容，再依脚本档内容来设置键盘与鼠标。AutoHotkey 安装文件夹里有几个重要文件要请大家注意：

文件名称 | 功能说明
-|-
AutoHotkey.exe | 用户所选构建的 AutoHotkey 主程序，AutoHotkeyA32.exe 为 ANSI 构建，AutoHotkeyU32.exe 为 Unicode 构建。
AutoHotkey.chm | 英文帮助，很好的学习参考文档，可[下载中文帮助](http://sourceforge.net/projects/ahkcn/files/latest/download)覆盖。
AU3_Spy.exe | Active Window Info，显示窗口控件信息的小工具，这些信息对高端的脚本撰写很有帮助。
Compiler\Ahk2Exe.exe 及相关文件 | 从脚本档生成执行档的图形工具，以方便没有安装 AutoHotkey 系统的环境能用执行档直接执行。
Installer.ahk | 安装程序的 ahk 脚本，结构工整、形式优美，可作为范本学习参考。

总之，我们只要把 AutoHotkey 的设置与指令写在扩展名为 .ahk 的文本档里，就能设置需要的动作。以下我们由浅入深、按部就班地展示 AutoHotkey 的各项功能。

## 由简单的范例开始使用 AutoHotkey 的热字串
请用【开始→程序集→附属应用程序→记事本】启动记事本（或使用你惯用的文本编辑程序），输入以下内容后保存成 test1.ahk。
![编写脚本]({{ site.url }}/assets/images/20140802006.png)
以上是常用网址与常用电子邮件的几个热字串范例，提示几个重点：

* 每行若以半角分号开头则表示此列是说明注解，不会被执行
* 热字串的关键字（或称缩写）必须用两个半角冒号夹住，再把要扩展的结果写在结尾的冒号后面，只能写一行（多行的写法请见后面的说明）
* 虽然范例中的关键字只有一个字母，实际运用上可任意组合多个字母与数字

双击 test1.ahk 后就能在 System Tray 里看到 AutoHotkey 的 H 图标，表示已执行并加载 test1.ahk。我们另行建立一个 test.txt 来测试，开启 test.txt 后，只要键入「y!」与一个触发符号（此符号可以是〔空白〕、〔Tab〕键或〔Enter〕键等，能透过指令定义），则关键字会替换成冒号后面的内容：

输入文本 | 触发符号 | 替换后的内容
-|-|-
y! | 空白 | http://tw.yahoo.com/
g! | Tab | http://www.google.com.tw
w! | Enter | http://www.wretch.cc
@g | 空白 | @gmail.com
@m | 空白 | @Your_Mail_Address.com.tw

触发符号要使用〔空白〕或〔Enter〕键悉听尊便，我个人是习惯用〔空白〕（更多触发符号请参阅[终止符](http://ahkcn.github.io/docs/Hotstrings.htm#EndChars)）。另外，为了避免在中文输入状态下使用到拆字按键而造成中文无法正常输入，**建议关键字以一个特殊字符开头或结尾**，例如范例中的惊叹号与 @ 符号，不过此二符号必须加按〔Shift〕键，不甚方便也影响输入速度，建议可使用单键符号，例如单引号、分号、斜线或逗点等来组成关键字，我个人常用的是单引号、斜线与逗点，最好是选用中文输入法未使用到的字符，以方便能在中文状态下也能输入。以下是修改成单引号与斜线后的范例：

输入文本 | 触发符号 | 替换后的内容
-|-|-
‘y | 空白 | http://tw.yahoo.com/
‘g | Tab | http://www.google.com.tw
‘w | Enter | http://www.wretch.cc
/g | 空白 | @gmail.com
/m | Tab | @Your_Mail_Address.com.tw

编辑修改 test1.ahk 后必须重新加载才能让变动生效，重新加载有两种方法：

1. 在右下角 System Tray 找到 AutoHotkey 的 H 图标后，按右键选【Exit】以结束目前的 AutoHotkey，再双击修改后的 test1.ahk 以重新启动 AutoHotkey
1. 第二个是较简便的方法，一样开启 System Tray 的 H 图标后，按右键选【Reload This Script】即可重新读入修改后的脚本指令
![从托盘图标重载脚本]({{ site.url }}/assets/images/20140802007.png)

## 常用的几种热字串范例

读者们可以自行汇总日常常用的字串，将之设置于 .ahk 文件内，再把 .ahk 文件存到启动文件夹里，如此便能自动重复使用了。笔者汇总几类常用的字串供各位做参考与当做你设置的启始内容，你可把下列表格的前两栏「关键字」与「替换后的内容」写入 test1.ahk 即可：

<pre>
<table>
<thead>
<tr>
<th width="89">关键字</th>
<th width="362">替换后的内容</th>
<th width="203">说明</th>
<th>分类</th>
</tr>
</thead>
<tbody>
<tr>
<td>::’g::</td>
<td>http://www.google.com.tw</td>
<td>Google网站</td>
<td rowspan="3">常用搜寻网站</td>
</tr>
<tr>
<td>::’y::</td>
<td>http://www.yahoo.com.tw</td>
<td>Yahoo! 网站</td>
</tr>
<tr>
<td>::’l::</td>
<td>http://www.live.com</td>
<td>微软 Live Search 网站</td>
</tr>
<tr>
<td>::’dic::</td>
<td>http://dictionary.yahoo.com.tw</td>
<td>Yahoo! 奇摩字典</td>
<td rowspan="2">字典网站</td>
</tr>
<tr>
<td>::’cdic::</td>
<td>http://140.111.34.46/newDict/dict/index.html</td>
<td>教育部重编国语辞典修订本网站</td>
</tr>
<tr>
<td>::@g::</td>
<td>@gmail.com</td>
<td></td>
<td rowspan="2">常用电子邮件</td>
</tr>
<tr>
<td>::@h::</td>
<td>@ms1.hinet.net</td>
<td></td>
</tr>
<tr>
<td>::’tk::</td>
<td>Thanks.</td>
<td>内容也可用「谢谢」，端视使用频率而定</td>
<td rowspan="4">常用邮件文本</td>
</tr>
<tr>
<td>::btw::</td>
<td>By the way,</td>
<td></td>
</tr>
<tr>
<td>::’br::</td>
<td>Best regards,</td>
<td></td>
</tr>
<tr>
<td>::’sy::</td>
<td>Sincerely yours,</td>
<td></td>
</tr>
<tr>
<td>::’me::</td>
<td>我的名字</td>
<td>你的姓名。</td>
<td>个人信息</td>
</tr>
</tbody>
</table>
</pre>

## 热字串配合使用 AutoHotkey 的按键字串

如果你在浏览器网址列输入范例内的热字串后，可能会想是否能让热字串能自动输出〔Enter〕键呢？如果可以的话，我们就可以少按一个〔Enter〕键了，这个需求只要在热字串里加上按键字串就能轻而易举的达成。AutoHotkey 按键的写法是在按键名称前后加上大括号，因此 `{enter}` 就代表〔Enter〕键，`{home}` 就代表〔Home〕键，以下枚举几个常用的按键（更多可用按键请参阅[按键和按钮列表](http://ahkcn.github.io/docs/KeyList.htm)）：

<pre>
<table>
<thead>
<tr>
<td>{Enter}</td>
<td>Enter 键</td>
<td>{Escape} 或 {Esc}</td>
<td>Escape 键</td>
<td>{Tab}</td>
<td>Tab 键</td>
</tr>
</thead>
<tbody>
<tr>
<td>{Backspace} 或 {BS}</td>
<td>倒退键</td>
<td>{Delete}</td>
<td>删除键</td>
<td>{Insert}</td>
<td>插入键</td>
</tr>
<tr>
<td>{Up}、{Down}、{Left}、{Right}</td>
<td>方向键</td>
<td>{PgUp}、{PgDn}</td>
<td>换页键</td>
<td>{CapsLock}</td>
<td>大写键</td>
</tr>
<tr>
<td>{NumLock}</td>
<td>数字锁定键</td>
<td>{Ctrl}、{LCtrl}、{RCtrl}</td>
<td>控制键与左、右控制键</td>
<td>{Alt}、{LAlt}、{RAlt}
<td>Alt 键与左、右 Alt 键</td>
</tr>
</tbody>
</table>
</pre>

上面的写法除了键盘之外，鼠标按钮也能用相同的格式来表示，例如：

写法 | 含义 | 写法 | 含义
-|-|-|-
{LButton}、{MButton}、{RButton} | 左、中、右钮 | {WheelDown}、{WheelUp} | 滚轮向下与向上

按键字串里若加上数字代表连续输出数个相同的按键，例如 `{Left 3}` 表示输出 3 个左键（务必只用半角字符，全角是无法使用的），等于 `{Left}{Left}{Left}`，按键字串和数字的间必须以至少一个半角空白分隔开。热字串加上这些按键的组合能够形成更多样化的功能，例如：

关键字 | 替换后的内容 | 触发符号 | 说明
-|-|-|-
::’y:: | http://tw.yahoo.com/{Enter} | 空白 | 输入 `’y` 会输出网址与〔Enter〕键
::/g:: | @gmail.com{Home} | 空白 | 输入 `/g` 会输出电子邮件并将光标移到开头位置（如同按下〔Home〕键）
::’img:: | {Left 2} | 空白 | 输入 `’img` 替换成标签，且将光标移到双引号里面，但因为 AutoHotkey 缺省会把触发符号也输出，造成光标左移到双引号里后又多输出当做触发符号的空白

AutoHotkey 热字串的替换依据不同的需求会有不同的选项，用户能很方便地设置不同的功能；热字串选项是写在开头两个冒号中间，格式是「**:选项:**」，举几个例子说明常用的选项（更多选项请参阅[热字串选项](http://ahkcn.github.io/docs/Hotstrings.htm#Options)）：

<pre>
<table>
<thead>
<tr>
<th>关键字</th>
<th>替换后的内容</th>
<th>触发符号</th>
<th>说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>:O:’img::</td>
<td>{left 2}</td>
<td>空白</td>
<td>必须按触发符号以替换文本，但<strong>不输出</strong>触发符号；只要把开头的两个冒号改成 :O: 即可。O 是 Omit（忽略）的意思，用来忽略触发符号</td>
</tr>
<tr>
<td>:*:@g::</td>
<td>test@gmail.com</td>
<td>无</td>
<td>:*: 表示<strong>不需要</strong>触发符号，键入 @ 和 g 两个字符后，立刻替换内容</td>
</tr>
<tr>
<td>:B0:::<p></p>
<p>（B零）</p></td>
<td>{Left 7}</td>
<td>空白</td>
<td>AutoHotkey 缺省会把关键字删掉（即触发后自动执行倒退以删掉关键字），此倒退功能可以使用 :B0: 选项将之取消，如此关键字在替换后仍会保留下来，再附加替换后内容。<br />
输出结果：<strong>|</strong>（|是光标位置），光标前会多出做为触发符号的一个空白</td>
</tr>
<tr>
<td>:*B0:::</td>
<td>{Left 7}</td>
<td>无</td>
<td>再多加一个星号就能不使用触发符号，因而不会有上列多出一个空白的问题输出结果：<strong>|</strong>（|是光标位置）</td>
</tr>
</tbody>
</table>
</pre>

## 热字串使用多列文本的方法

上面的例子每个关键字只能替换一列文本，若想输出多列文本应该要如何设置呢？其实 AutoHotkey 提供了简单的语法来达成这个功能：只要用各占一列的左右括号把多列文本夹起来就可以了。

{% highlight ahk %}
::long1::
(
  Dear xxx,
  Best regards,
  Your Name
)
{% endhighlight %}

## AutoHotkey 的热键设置方式

热键的设置也是很容易就能轻松完成，格式是「热键::执行的指令」，热键和要执行的指令间夹有两个半角冒号。热键有许多按键组合，以下是几个特殊的按键符号（更多前缀符号请参阅[热键修饰符](http://ahkcn.github.io/docs/Hotkeys.htm#Symbols)）：

<pre>
<table>
<thead>
<tr>
<th>按键符号</th>
<th>代表的按键与说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>^</td>
<td width="648">〔Ctrl〕键</td>
</tr>
<tr>
<td>!</td>
<td>〔Alt〕键</td>
</tr>
<tr>
<td>+</td>
<td>〔Shift〕键</td>
</tr>
<tr>
<td>#</td>
<td>〔Win〕键</td>
</tr>
<tr>
<td>&</td>
<td>用 & 符号把两个按键或按钮组合成为一个键，例如：LButton & a 表示按左钮不放，同时再按〔a〕键</td>
</tr>
<tr>
<td>~</td>
<td>加 ~ 符号表示<strong>不抑制</strong>该按键，使用在当我们想要把某个按键做额外输出的场合。例如：
<pre>; 不抑制原来的〔a〕键，〔a〕键替换成 aBBB
~a::Send BBB
; 按〔a〕键输出 BBB，抑制原来的按键输出
a::Send BBB
</pre>
</td>
</tr>
</tbody>
</table>
</pre>

指定好热键后，再接两个半角冒号，再用缺省的命令让 AutoHotkey 执行特定任务，用范例说明：

<pre>
<table>
<thead>
<tr>
<th>热键设置</th>
<th>指令说明</th>
</tr>
</thead>
<tbody>
<tr>
<td>#n::Run notepad</td>
<td>按〔Win+N〕键执行记事本程序（Notepad.exe）Run 命令表示执行后面指定的程序</td>
</tr>
<tr>
<td>^!F::run c:\program files\mozilla firefox2\firefox.exe</td>
<td>按〔Ctrl+Alt+F〕键执行 FireFox</td>
</tr>
<tr>
<td>~RButton::MsgBox 按了右键</td>
<td>按右键显示「按了右键」对话窗，若在记事本里操作，则原本的右键功能表会被「按了右键」对话窗取代了。<br>
MsgBox 是 Message Box 的意思，弹出显示消息对话窗</td>
</tr>
<tr>
<td>RButton::MsgBox 按了右键</td>
<td>弹出【按了右键】对话窗后继续显示记事本的右键功能表</td>
</tr>
<tr>
<td>~MButton & a::Send 送出消息</td>
<td>按鼠标中钮（两个按钮的鼠标，中钮就是滚轮）后用 MsgBox 函数显示对话窗</td>
</tr>
</tbody>
</table>
</pre>

多列式的热字串是用括号夹住文本，同样地热键也能执行多列命令：每个命令必须各占一行，开头的空白内缩只是为了阅读便利而已，没有空白或空白的数目多寡皆不会影响命令的执行，最后必须以 return 结束：

{% highlight ahk %}
; 一个〔Ctrl+Alt+F〕按键先后启动 FireFox 和记事本
^!F::
Run c:\program files\mozilla firefox2\firefox.exe
Run c:\Wndows\notepad.exe
return
{% endhighlight %}

设置好并重新加载后试一下是否成功了呢？咦？好像不行……很有可能是你电脑的系统目录和范例里的位置不同，比如操作系统的系统数据可能是 C:\Windows 或 C:\WINNT 或安装系统时自行指定的文件夹名称，因此若想让一个指令适用多个作业环境的话，就必须使用内置变量来替代固定的文件夹名称，以下是几个你可能会使用到的与文件夹相关的内置变量（更多的文件夹内置变量请参阅[操作系统和用户信息](http://ahkcn.github.io/docs/Hotkeys.htm#Symbols)）：

内置变量 | 用途 | 范例
-|-|-
A_WinDir | Windows 系统文件夹 | C:\Windows 或 C:\WINNT
A_ProgramFiles | 程序集文件夹名称 | C:\Program Files
A_AppData | 用户个人文件夹 | C:\Documents and Settings\用户\Application Data
A_Desktop | 用户桌面文件夹 | C:\Documents and Settings\用户\桌面

知道变量后再回头修改指令，在使用变量时其前后要夹上百分号 %。当然了，如果你的环境是固定的，直接写成固定文件夹也不会有问题，反而还更快速呢。下面是使用通用方式的写法：

{% highlight ahk %}
; 一个〔Ctrl+Alt+F〕按键先后启动 FireFox 和记事本
^!F::
Run %A_ProgramFiles%\mozilla firefox2\firefox.exe
Run %A_WinDir%\notepad.exe
return
{% endhighlight %}

还有几个和系统相关的内置变量，可以方便取出这些信息：

<pre>
<table>
<thead>
<tr>
<th>内置变量</th>
<th>用途</th>
<th>范例</th>
</tr>
</thead>
<tbody>
<tr>
<td>A_YYYY、A_MM、A_DD</td>
<td>
<p>分别传回系统日期的年度、月份、日子</p>
</td>
<td>
<pre>::'d::
d = %A_YYYY%/%A_MM%/%A_DD%
Send %d%
return</pre>
</td>
</tr>
<tr>
<td>A_Hour、A_Min、A_Sec</td>
<td>分别传回系统时间的小时、分钟、秒数</td>
<td>
<pre>::'t::
t = %A_Hour%:%A_Min%:%A_Sec%
Send %t%
return</pre>
</td>
</tr>
<tr>
<td>Clipboard</td>
<td>剪贴板内容</td>
<td>可以取用也可设值，在后面会有使用范例</td>
</tr>
</tbody>
</table>
</pre>

接着我列出几个我常用的按键设置当做大家使用的启始参考范例：

{% highlight ahk %}
; 按 Win+G 送出 @gmail.com 字串
#g::Send @gmail.com

; 按〔Win+H〕送出 @hotmail.com 字串
#h::Send @hotmail.com

; 按〔Win+2〕送出公司的电子信箱。用 2 的原因是因为 2 和 @ 是同一个按键，方便记忆
#2::Send @your_mail_address.com.tw

; 按〔Win+O〕开启 OpenOffice Writer
#o::Run %A_ProgramFiles%OpenOffice.org 2.4\programs\writer.exe

; 按〔Win+B〕启动缺省浏览器并加载指定网址
#b::Run http://www.google.com.tw

; 按〔Ctrl+Alt+C〕开启控制台窗口
^!c::Run %A_WinDir%\System32\control.exe

; 按左钮不放再按〔e〕启动记事本以编辑 AutoHotkey 脚本档
~LButton & e::Run %A_WinDir%notepad.exe C:\Program Files\AutoHotkey\test.ahk

; 按左钮不放再按〔r〕以重新加载（reload）脚本档，使修改过的内容能启用
~LButton &r::
reload
return
{% endhighlight %}

## AutoHotkey 的中文应用
（本段内容仍待整理）还有多少用户曾记得无法直接支持中文（准确说是仅支持 ANSI 字符）的热字串？当时要[实现替换为中文有两种方法](http://jdev.tw/blog/713/autohotkey-output-chinese)，一是使用剪贴板，二是利用 [Send 命令](http://ahkcn.github.io/docs/commands/Send.htm)的 `{ASC nnnnn}` 输出 Unicode 字符（某些程序可能不支持，包括其 Unicode 版本）。

Word 有提供把特殊符号指定给按键的用法，但只能在 Word 里使用，如果通过 AutoHotkey 设置，那么不管你操作的是那一种程序，统统都能适用。下面我们用一些热键设置来方便输入中文的标点符号。

{% highlight ahk %}
; 输入 ’addr 与触发符号后替换成地址。
:: 'addr::
current_clipboard = %Clipboard% ; 先把剪贴板目前内容存入 current_clipboard 变量
Clipboard = 台北市中山区民生东路二段 141 号 B1 ; 将电脑人公司的地址存入剪贴板
Send ^v
; 用「Ctrl+V」执行粘贴剪贴板内容
Clipboard = %current_clipboard% ; 再把剪贴板还原回原本的内容
return
 
; 按〔Ctrl+点〕送出句点
^.::
Clipboard = 。 ; 把句点存入剪贴板　　　　　
Send ^v
; 送出〔Ctrl+V〕粘贴句点
return
 
; 按〔Ctrl+半角逗点〕送出全角逗点
^,::
Clipboard = ，
Send ^v
return

; 按〔Ctrl+单引号〕送出顿点 
^'::
Clipboard = 、
Send ^v
return
 
; 按〔Alt+分号〕送出全角分号
!;::
Clipboard = ；
Send ^v
return
 
; 按〔Alt+1〕送出左箭头
!1::
Clipboard = ←
Send ^v
return
 
; 按〔Alt+1〕送出右箭头
!2::
Clipboard = →
Send ^v
return
{% endhighlight %}

接着我们再设置几个中文括号，先把要放在括号里的文本选取好，再按指定的按键就能把被选取文本夹在括号里。由于这些按键的处理指令大同小异，只有括号的符号不同而已，因此我们可以把指令集中到一个函数（send_bracket）里。

{% highlight ahk %}
![::
current_clipboard = %Clipboard% ; 把原有剪贴板内容存起来　　　
Clipboard = ; 把剪贴板清空
Send,
^c ; 把选取文本复制到剪贴板
ClipWait,1 ; 等待剪贴板保存动作完成
clipboard = 「%clipboard%」
; 在剪贴板前后加上全角括号
Send,
^v{left} ; 粘贴加了括号后的剪贴板内容
Clipboard = %current_clipboard% ; 剪贴板还原回原来内容
return
 
^[::
send_bracket("「", "」")
return
 
#[::
send_bracket("〔","〕")
return
 
^]::
send_bracket(“『“,”』“)
return
 
^![::
send_bracket("【","】")
return
 
^!]::
send_bracket(“《“,”》“)
return
 
send_bracket(start, end) {
  current_clipboard = %Clipboard%
  Clipboard =
  Send, ^c
  ClipWait,1
  clipboard = %start%%clipboard%%end%
  Send ^v{left}
  Clipboard = %current_clipboard%
  return
}
{% endhighlight %}

AutoHotkey_L 出现后，实现了对 Unicode 的全面支持，这种情况成为了历史。

* send_brackets 函数能将选取的文本或剪贴板内容的前后加上不同的中文括号，例如《书名号》、【中括号】等。
* send_html 则是输出常用的 HTML 标签，会判断剪贴板内容是否为网址而输出到不同的插入点。
* send 指令以 SendInput 取代，输出速度提升不少


{% highlight ahk %}
; 传回选取文本的内容
getSelectedText() {
  clpb_saved = %ClipboardAll% ; save clipboard
  Clipboard := "" ; clear
  Send, ^c ; simulate Ctrl+C (=selection in clipboard)
  selection = %Clipboard% ; save the content of the clipboard
  Clipboard = %clpb_saved% ; restore old content of the clipboard
  return selection
}

; 送出一组中文符号，如 send_brackets("「」")
send_brackets(symbol) {
  selection := getSelectedText()
  StringLen, length, selection
  if (length = 0) {
    selection = %clipboard%
  }
  ; msgBox %selection%
  left_str := Substr(symbol,1,1)
  right_str := Substr(symbol,2,1)
  sendInput %left_str%%selection%%right_str%
  StringLen, length, selection
  length := length + 1
  sendInput {left %length%}
  return
}

; 依剪贴板开头是否为“http:”来替换 $$ 或一般文本的 @@
; "@@"
send_html(tag, pos1, pos2) {
  selection := GetSelectedText()
  StringLen, length, selection
  if (length = 0) {
    selection := clipboard
  }
  leading := Substr(selection, 1,5)
  ;msgbox %leading%
  newstr = tag
  if (leading = "http:") {
    StringReplace, newstr, tag, $$, %selection%,
	StringLen, length, selection
	length := pos1
  } else {
    StringReplace, newstr, tag, @@, %selection%,
    StringLen, length, selection
	length := length + pos2
  }
  ;msgbox after=%newstr%
  sendInput %newstr%
  sendInput {left %length%}
  return
}

; 按 Ctrl+Comma 输出全角逗点，以此类推（下面这三个热键可能有误，需加 Send）
^,::，
^.::。
^;::；
^[:: send_brackets("「」")
^]:: send_brackets("『』")
^![:: send_brackets("【】")
^!]:: send_brackets("《》")
\#[:: send_brackets("〔〕")
^':: send_brackets("""""")

;===== HotString =====
::,a::
send_html("@@",6,8)
return

::,img::
send_html("@@",4,11)
return

::,aimg::
send_html("@@",8,8)
return
{% endhighlight %}

## AutoHotkey 的综合运用：标示字串与搜寻

当我们在某份文档或某网页上看到某个词句想要用搜寻引擎来查询时，大致会有下列四个步骤：

3. 把该词句存入剪贴板
3. 开启搜寻引擎网站
3. 粘贴剪贴板里的词句
3. 按搜寻

如果透过 AutoHotkey 我们可以把动作简化成两个步骤：

3. 选取要搜寻的词句
3. 按自订的一个按键，例如〔Alt+G〕


{% highlight ahk %}
; 选取文本后按〔Alt+G〕执行 Google 搜寻
!g::
current_clipboard = %Clipboard% ; 把目前的剪贴板内容存起来供后面还原
Clipboard = ; 先把剪贴板清空
Send ^c
; 把选取字串用〔Ctrl+C〕存入剪贴板
ClipWait, 1 ; 等待 1 秒让剪贴板执行存入动作
; 下行使用 Google 执行搜寻动作，要搜寻的字串就是剪贴板内容
Run http://www.google.com.tw/search?hl=zh-TW&q=%Clipboard%
Clipboard = %current_clipboard% ; 还原先前的剪贴板内容
return

; 选取文本后按〔Alt+Y〕执行 Yahoo! 搜寻
!y::
current_clipboard = %Clipboard%
Send ^c
ClipWait, 1
Run http://tw.search.yahoo.com/search?ei=UTF-8&p=%Clipboard%
Clipboard = %current_clipboard%
return

; 选取文本后按〔Alt+L〕执行微软 Live Search 搜寻
!l::
current_clipboard = %Clipboard%
Send ^c
ClipWait, 1
Run http://search.live.com/results.aspx?mkt=zh-tw&q=%Clipboard%
Clipboard = %current_clipboard%
return
{% endhighlight %}

## 提升效率的好帮手

在简要的介绍 AutoHotkey 的热键与热字串功能后，你是否也认为它确实能为你的电脑生活带来更好的效率呢？必须额外付出的学习成本事实上也是相当低
廉的，为了更快速、更便捷的电脑生活，AutoHotkey 是极佳的自我投资。

本文仅是基础的入门介绍，以下列出几个网站供想要更上层楼的读者们参考：

* [官方网站](http://ahkscript.org/)、[论坛](http://ahkscript.org/boards/)及[其中文子论坛](http://ahkscript.org/boards/viewforum.php?f=26)
* [中文爱好者论坛](http://ahk8.com/)
* 中文帮助：帮助是很好的参考手册，同时自带了很好的[初学者向导](http://ahkcn.github.io/docs/Tutorial.htm)
* 本博客中更多的 AutoHotkey 使用经验和技巧：[AutoHotkey 分类][1]、[AutoHotkey 标签][2]

[1]: http://jdev.tw/blog/category/software-and-tools/autohotkey-keyboard
[2]: http://jdev.tw/blog/tag/autohotkey
