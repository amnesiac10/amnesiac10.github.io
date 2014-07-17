---
date: 2014-07-31 06:10:54+08:00
layout: post
title: 自动化操作轻松入门
thread: 14
categories: 指南
tags:
---
转者按：本文属于**零基础入门**专题教程，原[发表于博客中国](http://yonken.blogcn.com/diarypage.shtml?sortsid=100075373)（已失效），我曾[转载到中文论坛](http://ahk8.com/thread-3111-post-17835.html)，作者 yonken（[此处是他现在的博客](http://www.cnblogs.com/yonken/)，但原来的很多 AutoHotkey 内容丢失了）。本专题选取一些通俗易懂的基础入门教程，经适当整理（以反映目前 AutoHotkey 现状）后集中发表，以方便初次接触脚本的朋友入门（帮助中的[初学者向导](http://ahkcn.sourceforge.net/docs/Tutorial.htm)也是很好的入门教程）。

前言：据我了解需要编写 AutoHotkey/AutoIt 脚本来实现自动化操作的用户很多都是网管，其它则可能是一些个人用户，他们一般都具有相当的技术水平，而且都希望能借助脚本来完成某些以往需要人工操作的重复​性劳动，但限于语言条件上的限制可能对官方的帮助文档有较难理解之处。为方便读者，我将从最简单的说起，每个示例尽可能同时给出相应的 AHK 和 AU3 版本代码。本文将尽可​能用较通俗的语言描述，但并不打算讲解语法基础，所以不一定适合新手阅读。
文中涉及到的 AHK/AU3 版本（注：这个是最初编写时的版本）：

* AutoHotkey 1.0.44 .08
* AutoIt 3.1.1

## 关于脚本
### 什么是脚本？
这是个非常“流行”的术语了，通俗而言脚本（Script）一般都是指根据某种语法规则编写的具有特定格式的文本文件。可能大家已经听说过很多种脚本：VBScript、​JScript、PHP、ASP、JSP、CGI、CS 脚本，甚至游戏工具脚本。
这些脚本文件都是可执行文件，可执行相应的操作。

* AHK 脚本文件扩展名：\*.ahk
* AU3 脚本文件扩展名：\*.au3

### 脚本和程序的不同？
严格来说，所谓“程序”就是指以各种编程语言（比如说 C/C++/C#/Delphi）编写、由编译器编译好后的二进制文件，一般就是机器代码，可由系统执行。而脚本则是只是些纯文本文件，包含了各种定义好的命令，这一点很像批处理文件。这样​，我们得出一个简单的结论，那就是用户一般无法获得“程序”的源代码，我们只能进行反汇编把它逆向还原为汇编语言代码（或其它），当然，也有些“程序”是可以获得源代码的​（比如 Java）；脚本则是用户可直接查看的代码文件，而 AHK/AU3 则提供了把脚本文件“转换”成可执行文件的方法。

### 脚本如何运行？
脚本是“解释性”的语言，它 的运行依赖一个“解释器”，由这个解释器来“翻译并解释”脚本的每条命令（或者说代码），然后执行相应操作。如果不严格定义的话，HTML 和 Java 都可以认为是解释性语​言。AHK/AU3 的主程序（分别是 AutoHotkey.exe 和 AutoIt3.exe）就是它们的“解释器”，上面提到脚本可“转换”成可脱离相应的解释器而独立运行的可​执行文件，而我们还可以使用相应的工具把它们“还原”成脚本文件，由此我们完全可以这么理解：脚本代码是被 “压缩”到这个可执行文件中，解释器也是在里面，在运行可执行文件时实际上是先“解压”脚本代码然后运行解释器并解释该脚本。

### 如何创建脚本？
使用资源管理器的右键菜单即可创建相应脚本文件，或者新建一个文本文件后改扩展名即可。

### 稍微介绍一点语法规则？
A）对 AHK 而言，每个内建的功能是以“命令”的形式提供：
`Command, param1, param2,…`
而 AU3 则以“函数”的形式提供：
`Function(param1, param2, …)`
命令或函数中被符号“[”和“]”围住的参数是可选参数，表示在使用这些命令或函数时可省略它们（不给出具体数值）。
若某个参数含有空格，则最好使用双引号围住该参数。

B）解释器自上而下（从第一行到最后一行）“解释”脚本中的每行语句，除非遇到“Return”、“Goto”、“Gosub”、“Exit”等语句、函数、热键或其它能使​脚本“跳”到某个标识符的条件成立。

C）关键字和标识符（包括变量名、命令名、函数名等）都不区分大小写。

## 运行程序或打开文件
### 运行程序
Run 命令或者函数用来运行外部可执行文件，AHK还可利用它来直接打开文件。
AHK：`Run, 目标文件 [, 工作目录, Max|Min|Hide|UseErrorLevel, 输出PID变量]`
AU3：`Run ( "文件名" [, "工作目录" [, 标志]] )`

【示例 2.1.1 】
AHK：`Run, Notepad.exe`
AU3：`Run("Notepad.exe")`
上面的示例中都没有给出程序“Notepad.exe”的路径，为什么仍能执行？这是因为它们都会自动在脚本所在目录下搜寻目标文件，如有则运行，否则就到系统文件夹（%​PATH%）中搜寻。

注意：
A）某些程序必须给定“工作目录”才能成功运行！
B）给出完整的文件路径有助于轻微提高程序的可靠性。
C）AHK 的 Run 命令可以用来运行程序和直接打开文件，而 AU3 的 Run 函数则只能用来运行程序（可执行文件）或传递参数让某个程序打开目标文件。

当然，运行程序的功能还不仅仅是这么简单，我们还可以指定运行程序的初始状态，比如让运行的记事本窗口以最大化状态显示（或者最小化、隐藏）：

【示例 2.1.2 】
AHK：`Run, Notepad.exe, , Max`
AU3：`Run("Notepad.exe", "", @SW_MAXIMIZE)`

### 打开文件
前面已经提到，AHK 的 Run 命令可以直接打开文件，而 AU3 的 Run 函数则只能用来运行程序，因此在打开文件的方式上有点不同：AHK 脚本中可直接给出目标文件，而 AH​K 将自动运行该文件的关联程序来打开它；而 AU3 则必须由用户自己传递参数让某个程序打开目标文件。

【示例 2.2.1 】
AHK：
`Run, MyFile.txt`
`Run, Notepad.exe MyFile.txt`
AU3：`Run("Notepad.exe MyFile.txt")`

### 以命令行形式运行程序
可以考虑运行系统的命令行解释器（cmd.exe/command.com），然后指定要执行的命令并传递参数。
假设我们要执行命令“dir C:\WINDOWS\system 32” ，用以列出指定目录的所有文件及子目录。

【示例 2.3.1 】
AHK：`Run, %ComSpec% /k dir C:\WINDOWS\system32`
AU3：`Run(@ComSpec & " /k dir C:\WINDOWS\system32")`

注意：
A）%ComSpec% 是脚本内建的用以指示命令行解释器位置的变量或宏。
B）/k 参数表示“执行字符串指定的命令但保留”，若改为 /c 则表示“执行字符串指定的命令然后终断”。对此比较直观的解释是 /k 将在执行完命令后保留命令提示窗口，而 /c 则将在执行完命令之后关闭命令提示窗口。
C）符号“&”是 AU3 定义的字符串连接符。

### 特殊应用
A）打开网页
【示例 2.4.1 】
AHK：
`Run, http://ahkscript.org/`
`Run, %A_ProgramFiles%\Internet Explorer\IEXPLORE.EXE http://ahkscript.org/`
AU3：`Run(@ProgramFilesDir & "\Internet Explorer\IEXPLORE.EXE http://ahkscript.org/")`

B）打开特殊文件夹
系统的某些特殊文件夹被定义了相应的 CLSID（请查看 [CLSID 列表](http://ahkcn.sourceforge.net/docs/misc/CLSID-List.htm)），我们可利用它来打开相应的文件夹，比如打开回收站：
【示例 2.4.2 】
AHK：`Run ::{645ff040-5081-101b -9f 08-00aa 002f 954e}`
AU3：不适用！

C）运行控制面板工具
微软已经为我们提供了通过命令行打开控制面板某个工具或项目的方式，比如打开系统属性窗口：
【示例 2.4.3 】
AHK：`Run control sysdm.cpl`
AU3：`Run("control sysdm.cpl")`
关于访问控制面板项目的详细介绍请查看此文：文章地址（原网址已丢失，请自行搜索`命令行方式访问控制面板项目的方法`查找其他转载）。

D）指定搜索位置并打开搜索窗口
假设我们要打开一个搜索窗口，而且要指定搜索位置，比如 C:\：
【示例 2.4.4 】
AHK：`Run, find C:\`
AU3：不适用！

E）显示指定文件的属性窗口
假设我们要打开文件“MyFile.txt”的属性窗口，则使用关键字 properties 然后接上目标文件即可：
【示例 2.4.5 】
AHK：`Run, properties MyFile.txt`
AU3：不适用！
注意：AHK 在退出前将自动关闭打开的属性窗口！

F）用“资源管理器”打开指定文件夹
我们知道使用 `Run, explorer C:` 或 `Run("explorer C:")` 即可打开指定的文件夹，可是有时候我们需要在资源管理器中打开它，这时可使用关键字 explore（注：前面的 explorer 实际上是资源管理器 explorer.exe 省略扩展名的写法，而 explore 是 Run 的关键字，为动词，类似于 find/print）：

【示例 2.4.6 】
AHK：`Run, explore C:`
AU3：`run("explorer.exe /e,C:\")`

G）打印指定文件
要打印指定文件，可使用关键字 print：
【示例 2.4.7 】
AHK：`Run, print MyFile.txt`
AU3：不适用！

## 窗口操作
注意：窗口标题和窗口文本参数总是对大小写敏感的。

### 等待窗口系列命令/函数
AHK 和 AU3 都提供了用法类似的一组窗口等待命令/函数：WinWait/WinWaitActive/WinWaitClose。
它们分别用于等待窗口出现、等待窗口被激活、等待窗口被关闭。由于这些命令/函数的参数类似，现仅以WinWait为例说明。

AHK：`WinWait [, 窗口标题, 窗口文本, 超时时间, 排除标题, 排除文本]`
AU3：`WinWait ( "窗口标题" [, "窗口文本" [, 超时时间]] )`
WinWait 的作用是在目标窗口出现之前不再执行后面的所有语句。

假设我们要运行记事本程序，并在其窗口出现时提示用户：
【示例 3.1.1 】
AHK：

```ahk
Run Notepad
WinWait, 无标题 - 记事本
MsgBox 记事本窗口已被打开！
```

AU3：

```au3
Run("Notepad")
WinWait("无标题 - 记事本")
MsgBox(0, "", "记事本窗口已被打开！")
```

### 激活窗口相关命令/函数
让目标窗口成为活动窗口的办法就是激活它，可用的命令/函数是 WinActivate：

AHK：`WinActivate [,窗口标题, 窗口文本, 排除标题, 排除文本]`
AU3：`WinActivate ( "窗口标题" [, "窗口文本"] )`

### 关闭窗口
关闭窗口有两种方式，一种是正常的关闭窗口（WinClose），另一种则是强行关闭窗口（WinKill）：

AHK：`WinClose/WinKill [,窗口标题, 窗口文本, 超时时间,, 排除标题, 排除文本]`
AU3：`WinClose/WinKill ( "窗口标题" [, "窗口文本"] )`

现在我们已经可以实现一个比较简单的功能了，比如我们可以打开系统属性窗口并等待其出现，窗口出现后激活它，接着等待 3 秒再关闭它：
【示例 3.1.2 】
AHK：

```ahk
Run, Sysdm.cpl
WinWait, 系统属性
WinActivate, 系统属性
WinWaitActive, 系统属性
Sleep, 3000
WinClose, 系统属性
WinWaitClose, 系统属性
```

AU3：

```au3
Run("Control Sysdm.cpl")
WinWait("系统属性")
WinActivate("系统属性")
WinWaitActive("系统属性")
Sleep(3000)
WinClose("系统属性")
WinWaitClose("系统属性")
```

建议：如果程序中频繁地出现要用到这些窗口标题的地方，会带来一个问题：脚本的可读性，也许你会想，这不是很直观吗？可问题是如果这个重复出现的窗口标题是个很长的字符串​呢？这将严重影响整个代码的排版美观。而且我们也无从了解这些窗口标题的“来头”，不知道这个窗口标题究竟是怎么来的。而如果我们定义一个变量（假设变量名是“AppWi​ndow1”）保存这个窗口标题，我们就能在命令/函数中用变量来表示它，这样就达到了让代码用意更清晰一点的目的。另外，就算目标软件因某些原因（比如升级）而改变了它的窗口标题，我们也能很方便地作出修改。

### 更准确的标识窗口的方法（主要针对 AHK 脚本）
程序在运行时起码会有一个进程，如果能获得这个进程 ID 就能在一定程度上保证对窗口的准确标识。另外，每个窗口都有定义窗口类名（Class，比如说记事本窗口的类名就是 Notepad），所以我们可以以此排除与目标窗口不同的其它窗口类。其实，我们还有一个更准确的方法：
每个窗口（包括控件在内）都被Windows指派了一个可区别于其它窗口的唯一的标识符（ID），我们称之为窗口句柄（HWND）。
直接给定窗口标题来表示窗口的一个缺点就是无法保证在脚本运行的过程中始终以该窗口为操作目标，因为在这个过程中很有可能会有其它“同名”窗口（或者说满足匹配条件的窗口​）出现，而如果我们使用这个标识符来表示窗口自然就能保证命令/函数的操作窗口总是同一个窗口了。

我们先来了解一下获得窗口句柄的命令/函数：
AHK：`WinGet[, 输出变量, ID, 窗口标题, 窗口文本, 排除标题, 排除文本]`
AU3：`WinGetHandle ( "窗口标题" [, "窗口文本"] )`
其中 WinGet 获得的窗口 ID 将通过“输出变量”返回，而 WinGetHandle 的返回值就是获得的窗口 ID。

我 们在进行自动化操作时是要先运行某个程序，如何获得这个程序成功运行后显示的窗口句柄？一个比较保险的办法是先获得这个程序的进程 ID，然后根据这个进程 ID获得它的窗口句柄，AHK 支持使用进程 ID 作为窗口标题使用；但 AU3 不支持这样使用，只能先获得该窗口的类名再根据该类名来获得窗口句柄（不够保险）：
【示例 3.1.3 】
AHK：

```ahk
Run, NotePad, , , ThisPID
WinWait, ahk_pid %ThisPID% ;这里的ahk_pid表明跟在后面的变量是进程ID
WinGet, ThisID, ID, ahk_pid %ThisPID% ;ThisID将保存获得的窗口句柄
```

AU3：

```au3
Opt("WinTitleMatchMode", 4)
Run("Notepad")
$handle = WinGetHandle("classname=Notepad")
```

现在暂且先忘记了 AU3 吧，因为它的窗口函数一般都不支持使用窗口句柄作为（窗口标题）参数。
至于如何在 AHK 中使用窗口句柄，简单的说，凡是有“窗口标题”参数的命令就可以用窗口句柄来代替，比如：
【示例 3.1.4 】
AHK：

```ahk
Run, Notepad, , , ThisPID ;先获得运行的记事本程序的进程ID
WinWait, 无标题 - 记事本 ahk_pid %ThisPID% ;等待该进程窗口的出现
WinGet, ThisHWND, ID, 无标题 - 记事本 ahk_pid %ThisPID% ;获得窗口句柄
WinActivate, ahk_id %ThisHWND% ;这里的 ahk_id 表明跟在后面的变量是窗口句柄
WinWaitActive, ahk_id %ThisHWND%
Sleep, 3000
WinClose, ahk_id %ThisHWND%
WinWaitClose, ahk_id %ThisHWND%
```

## 模拟键击和鼠标点击
### 模拟鼠标点击（按钮等）控件
既然是模拟用户操作，自然就包括了模拟鼠标点击在内。
适用命令/函数：Click/MouseClick/ControlClick
其中 Click/MouseClick 用来模拟用户的物理操作（点击），把鼠标点击事件发送到指定坐标位置（相对当前窗口或绝对位置）上，但这种方法并不能保证 100% 的准确性，屏幕分辨​率、用户干扰和系统环境等都会影响到它们的执行结果，而 ControlClick 则直接把鼠标点击事件发送到目标窗口的目标控件上，因而更准确，一般我们不考虑使用坐标位​置方式的点击，下面仅以 ControlClick 为例说明：
AHK：`ControlClick [, 目标控件或坐标位置, 窗口标题, 窗口文本, 鼠标按钮, 点击次数, 选项,排除标题, 排除文本]`
AU3：`ControlClick ( "窗口标题", "窗口文本", 控件ID [, 按钮] [, 点击次数]] )`

对 AHK 而言，“目标控件”参数是指要点击的控件的类别名（ClassNN）或控件文本，另外还可以使用控件句柄（若用的是控件句柄则第一个参数需留空，并在第二个参数中​使用 ahk_id %控件句柄%）。

Q：用什么工具来获得目标控件的这些信息呢？
A：AHK 用户请使用 Window Spy，AU3 用户则请使用 AutoIt Window Info，你可以在相应的开始菜单项目里找到它们，或者到安装目录下寻找。
Q：如何使用这两个工具？
A：先打开你要进行操作的目标窗口，然后运行 Window Spy 或 AutoIt Window Info，接下来就是把鼠标移到目标控件上（比如某个按钮）：

Window Spy 使用演示截图：
![2014-07-05 210804.png](http://upload-images.jianshu.io/upload_images/19661-c2b1e31b44145f7d.png)
AutoIt Window Info 使用演示截图：
![2014-07-06 061224.png](http://upload-images.jianshu.io/upload_images/19661-602dab3da12acff2.png)

现在我们假设已打开并激活了“系统属性”窗口，而任务是点击它的“确定”按钮，则可用以下几种方法：
【示例4.1.1】
AHK：

```ahk
ControlClick, 确定, 系统属性
ControlClick, Button2, 系统属性
```

AU3：

```au3
ControlClick("系统属性", "", 1)
ControlClick("系统属性", "", "Button2")
ControlClick("系统属性", "", "确定")
```

提醒：即使目标窗口或控件是隐藏状态，ControlClick 命令还是可以“点击”目标控件，但不能保证成功率。

### 模拟键盘操作
键盘也是我们在操作窗口时会用到的工具，比如说在安装软件的时候经典的“一路回车大法”。下面简单介绍一下模拟键盘操作的方法。
Send 是最直接的方法，就是模拟用户按键行为，直接发送键击命令，用法请参考官方文档，在此不予说明。
最简单的应用――按回车：
AHK：

```ahk
Run, Control Sysdm.cpl
WinWait, 系统属性
Send, {Enter}
```

AU3：

```au3
Run("Control Sysdm.cpl")
WinWait("系统属性")
Send("{Enter}")
```

常见的组合键――Alt+X / Ctrl+N 等等，在安装软件的时候经常会有提供一个按钮“下一步(N)”，表示按下 Alt+N 即可触发等同于点击该按钮的效果，其它的可触类旁通。以打开记事本窗口的“​文件”菜单为例：
AHK：

```ahk
Run, Notepad
WinWait, 无标题 - 记事本
WinActivate, 无标题 - 记事本
WinWaitActive, 无标题 - 记事本
Send, !f
```

AU3：

```au3
Run("Notepad")
WinWait("无标题 - 记事本")
WinActivate("无标题 - 记事本")
WinWaitActive("无标题 - 记事本")
Send("!f")
```

## 控件操作
然而，在真正实现自动化时仅靠上面的技术往往难以达到预期目的。下面开始进入最为重要的控件操作。
### 设置文本
在安装软件的过程中用户往往需要提供一些必需信息，比如安装目录。很多用户并不喜欢把软件安装到默认的 C 盘而更愿意把它们安装到别的地方，那么脚本究竟提供了什么方法能让​我们修改如下图所示的路径呢？
![2014-07-02 174402.png](http://upload-images.jianshu.io/upload_images/19661-e349bc97f65baf18.png)
我们先用上文中提到的 Window Spy 或 AutoIt Window Info 来获得这个路径的编辑框的信息，假设这个窗口的标题为 Setup foobar，该路径编辑框的类名是 Edit1，而我们需要把它改成“D:\foobar2000”，接下来就可以使用下列命令/函数来设置它的文本了：
AHK：`ControlSetText [, 目标控件, 新文本, 窗口标题, 窗口文本, 排除标题, 排除文本]`
AU3：`ControlSetText ( "窗口标题", "窗口文本", 控件ID, "新文本")`

具体用法如下：
【示例5.1.1】
AHK：`ControlSetText, Edit1, D:\foobar2000, Setup foobar`
AU3：`ControlSetText("Setup foobar", "", "Edit1", "D:\foobar2000")`

### 选中和取消选中单选框和复选框项目
有时程序为了满足用户的个性化设置而需要用户提供更多的信息，我们经常会遇到这样的情况：
![2014-07-02 194152.png](http://upload-images.jianshu.io/upload_images/19661-1b7e95d3afe9e54a.png)
如何保证选中所需项目并取消某些项目呢？
下面先来介绍 AHK 和 AU3 中用来对控件进行各种属性设置的命令/函数：
AHK：`Control [, 命令, 值, 目标控件, 窗口标题, 窗口文本, 排除标题, 排除文本]`
AU3：`ControlCommand ( "窗口标题", "窗口文本", 控件ID, "命令", "选项")`
其中，“命令”就是让我们指定要进行何种设置的参数。对这些单选框/复选框按钮来说，适用的命令是“Check”和“UnCheck”。

假设这个窗口的标题是为 Setup foobar，我们打算进行下来操作：
选中它的“桌面”复选框（Button5）、取消选中“快速启动栏”复选框（Button7）；
选中“0.7x”单选框（Button14）。
那么具体的用法示例如下：
【示例5.1.2】
AHK：

```ahk
Control, Check, , Button5, foobar
Control, UnCheck, , Button7, foobar
Control, Check, , Button14, foobar
```

AU3：

```au3
ControlCommand("foobar", "", "Button5", "Check", "")
ControlCommand("foobar", "", "Button7", "UnCheck", "")
ControlCommand("foobar", "", "Button14", "Check", "")
```

### 选择下拉列表的项目
相信你肯定遇到过下面这种情况：
![2014-07-02 174414.png](http://upload-images.jianshu.io/upload_images/19661-1577c38144700e20.png)
问题又来了：如何选中自己需要的项目？
答案仍是使用上面提到的命令/函数。对这种控件而言，AHK 适用的命令是“Choose, N”和“ChooseString, String”，分别表示选中第 N 个项目和选中与字符串 String 匹配的项目；而 AU3 适用的命令则是“SetCurrentSelection, N”和“SelectString, String”，分别表示选中第 N+1（注意是从零开始表示！）个项目和选中与字符串 String 匹配的项目。

假设我们要选中第五个项目“简体中文”，那么具体的用法示例如下：
【示例5.1.3】
AHK：

```ahk
Control, Choose, 5, ComboBox1, Installer
Control, ChooseString, 简体中文, ComboBox1, Installer
```

AU3：

```au3
ControlCommand("Installer", "", "ComboBox1", "SetCurrentSelection", 4)
ControlCommand("Installer", "", "ComboBox1", "SelectString", "简体中文")
```
