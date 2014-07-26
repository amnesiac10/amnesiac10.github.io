---
date: 2014-08-13 06:10:53+08:00
layout: post
title: amnesiac 的 Everything 热键
categories: 即用
tags: 热键 Everything TotalCommander
---

amnesiac 是我的 ID，以后不再赘述。

[Everything](http://www.voidtools.com/) 是个不错的软件，许多人应该都将它列为开机启动的程序之一。它本身提供了热键功能，包括新建窗口、显示窗口和切换窗口，通过切换窗口用热键控制激活和隐藏 Everything 在实际中足够了（如下图），所以下面这个脚本并不很必要。不过如果用类似的方式控制 [Total Commander](http://www.ghisler.com/) 效果就出来了，它较小众所以这里不作为例子。
![Everything 选项对话框]({{ site.url }}/assets/images/20140813000.png)

在启动 Everything 后，用一个热键可激活其搜索窗口（当最小化或隐藏时）、隐藏其窗口（当活跃时）。

## 脚本

{% highlight ahk %}
; 环境：WIN_XP; AutoHotkey 1.1.15.00 Unicode; Everything V1.3.3.658b (x86)

EverythingExe := "d:\Software\Everything\Everything-1.3.3.658b.x86.exe"

F1::    ; 打开/最小化/激活 Everything
IfWinActive, ahk_class EVERYTHING ; 窗口当前活跃，关闭（隐藏到后台了）。
{
    WinClose
    return
}
DetectHiddenWindows, On
IfWinNotExist, ahk_class EVERYTHING_TASKBAR_NOTIFICATION ; 未启动。
{
    Run, %EverythingExe%,, Max
    WinWait, ahk_class EVERYTHING_TASKBAR_NOTIFICATION,, 2
    if (ErrorLevel = 1)
    {
        MsgBox, 4112, 错误, Everything启动失败。
        return
    }
}
IfWinNotExist, ahk_class EVERYTHING ; 已启动但不存在窗口，说明在后台。
{
    PostMessage, 0x312, 0, 0x700000,, ahk_class EVERYTHING_TASKBAR_NOTIFICATION
    WinWait, ahk_class EVERYTHING,, 1
}
IfWinNotActive, ahk_class EVERYTHING ; 窗口不活跃，激活。
    WinActivate
return
{% endhighlight %}

## 分析

由于 Everything 可能未启动、启动了未激活（即最小化或隐藏）或激活状态，所以先理顺思路：

* 若 Everything 窗口活跃，则退出（或隐藏）；
* 若 Everything 窗口不存在，有两种情况；

当 Everything 窗口不存在时，有可能：

* Everything 未启动，则启动并激活；
* Everything 已启动，则激活窗口；

这里，不论是否已启动都需要激活窗口，所以合并到一起。接着重点说说 EVERYTHING_TASKBAR_NOTIFICATION 这个窗口，怎么来的呢？
![Microsoft Spy++ 进程列表]({{ site.url }}/assets/images/20140813001.png)

打开 Microsoft Spy++，在 Everything 进程中可以看到这个隐藏窗口（应该在选项中选中**后台运行**才会存在，若不选则关闭时会退出）。

前面已经设置“切换窗口热键”为 F1，这里监视这个隐藏窗口的消息后按下 F1：
![Microsoft Spy++ 消息窗口]({{ site.url }}/assets/images/20140813002.png)

Everything 的窗口出现了，同时消息窗口中也显示了隐藏窗口处理的消息，很幸运第一条即是我们需要的。其中 P 表示 [PostMessage](http://ahkcn.github.io/docs/commands/PostMessage.htm)，后面包含了参数（属性窗口中查看更方便，需要注意这些值都是十六进制，作为命令参数时需加“0x”），因此得到这条命令：

{% highlight ahk %}
PostMessage, 0x312, 0, 0x700000,, ahk_class EVERYTHING_TASKBAR_NOTIFICATION ; 必须开启隐藏窗口检测才有效。
{% endhighlight %}

0x312 表示 WM_HOTKEY，相关说明请参考 [WM_HOTKEY 资料页](http://msdn.microsoft.com/en-us/library/aa931321.aspx)。

刚才关于消息的来源介绍时只是蜻蜓点水，详细说明需要在另一篇专门文章了，若有兴趣可先参考帮助中的[消息指南](http://ahkcn.github.io/docs/misc/SendMessage.htm)。此外，后台运行的程序都会有隐藏窗口，有了窗口才能与系统、用户或其他进程交互（可能说法不够严谨，请专业人士斧正）。AutoHotkey 脚本运行时也有（即使不包含图形界面），这个隐藏窗口的妙用有机会再聊。

## 其他

### 我的实际脚本

我实际使用的脚本与上面有些差异：

* 有了 Listary 后，Everything 不经常启动了（所以不随系统启动）；
* 在启动前要设置 Everything.ini 以在 TC 中打开文件/文件夹路径；
* 定制了一些仅用于 Everything 的热键，用于快速执行常用过滤等；

由于各人习惯有异、实现的方法都比较简单且适用性不大，这里不发了。

### Total Commander 热键

尽管通过前面的内容应该能自己写出来，想想还是放在这里吧（这个更简单）。

{% highlight ahk %}
TotalCommanderExe := "d:\Software\totalcmd\TOTALCMD.EXE"
Return

F1::    ; 打开/最小化/激活TC
IfWinNotExist, ahk_class TTOTAL_CMD
{
    Run, %TotalCommanderExe%,, Max
}
Else
{
    IfWinNotActive
    {
        WinMaximize
        WinActivate
    }
    else
    {
        WinGet, TcWinState, MinMax, A
        If (TcWinState = 1)
            WinMinimize
        Else
            WinMaximize
    }
}
Return 
{% endhighlight %}
