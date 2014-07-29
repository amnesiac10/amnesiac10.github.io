---
date: 2014-08-18 14:06:23+08:00
layout: post
title: 游戏中脚本外挂的检测及预防
categories: 甜点
tags: A_ScriptHwnd HWND PostMessage
---

导言：一些游戏在运行时能检测并屏蔽脚本外挂，原理是什么？有什么办法可以知道当前系统中开了多少脚本？包括使用默认托盘图标、自定义图标甚至隐藏了托盘图标的脚本。还有，能发现已编译成可执行文件的脚本吗？是否能控制这些脚本？？

## 检测在运行的脚本
获取系统中当前运行的所有 AutoHotkey 脚本的信息（下面的脚本获取脚本标题和教程路径）：

```autohotkey
DetectHiddenWindows, on
WinGet, AHKWinList, List, ahk_class AutoHotkey 
Loop, %AHKWinList%
{
  AHKWinHWND := AHKWinList%A_Index%
  WinGetTitle, AHKWinTitle, ahk_id %AHKWinHWND%
  WinGet, AHKWinProcessPath, ProcessPath, ahk_id %AHKWinHWND%
  MsgBox, % "脚本标题：" AHKWinTitle "`n脚本进程路径：" AHKWinProcessPath
}
return
```
由于可以获取到窗口句柄，所以可以直接通过窗口命令获取指定脚本的各种信息。其中脚本标题中包含了脚本文件名和执行脚本的 AutoHotkey.exe 版本号，例如：

> D:\Software\AutoHotkey\Scripts\test.ahk - AutoHotkey v1.1.15.00

当然，还可以使用其他方法。下面通过消息获取某脚本的进程 ID：

```autohotkey
AHKScriptName := "MyScript.ahk"
SetTitleMatchMode, 2
DetectHiddenWindows, on
SendMessage, 0x44, 0x405, 0, , %AHKScriptName% ahk_class AutoHotkey
MsgBox %ErrorLevel% is the process id.
```
想判断哪些是未编译脚本, 哪些是已编译脚本, 请看下图：
![运行的 AutoHotkey 脚本列表]({{ site.url }}/assets/images/20140819000.png)
图中显示当前系统运行了两个 AutoHotkey 脚本，我们对比这两个脚本的窗口名称：

* D:\Software\AutoHotkey\Scripts\test.ahk - AutoHotkey v1.1.15.00
* D:\Software\AutoHotkey\Scripts\test.exe

可以看出一个是未编译脚本，一个是已编译脚本，因此可以根据窗口名判断。注：从我使用这种方法开始到现在，窗口名称的规律没有发生变化，不过这里无法保证以后的所有版本都会遵从。


## 对这些脚本进行控制
在写脚本时，我们都知道可以很方便的对当前脚本进行控制，如：
```autohotkey
Pause::Pause
^!s::Suspend
^!r::Reload
^+q::ExitApp
!l::ListLines
!v::ListVars
!k::KeyHistory
```
提示：我以前写脚本时习惯加上许多这样的热键，包括暂停、重启、列出热键、列出执行行、列出变量、显示按键历史等，调试时很方便。
那么，对其他脚本我们可以实现这些功能吗？
```autohotkey
AHKScriptName := "MyScript.ahk"
DetectHiddenWindows On  ; 才可以检测到脚本的隐藏主窗口.
SetTitleMatchMode 2  ; 避免为下面的文件指定完整的路径.
WM_COMMAND := 0x111
ID_FILE_PAUSE := 65403
ID_FILE_SUSPEND := 65404
PostMessage, WM_COMMAND, ID_FILE_PAUSE,,, %AHKScriptName% ahk_class AutoHotkey
PostMessage, WM_COMMAND, ID_FILE_SUSPEND,,, %AHKScriptName% ahk_class AutoHotkey
WinClose, %AHKScriptName% ahk_class AutoHotkey ; 关闭脚本，也可以使用消息，不过这里使用窗口命令可能直观一些。
PostMessage, 0x111, 65305,,, %AHKScriptName% ahk_class AutoHotkey
PostMessage, 0x111, 65306,,, %AHKScriptName% ahk_class AutoHotkey
```

注意：当前我不清楚如何判断脚本当前处于哪种状态（当前脚本可使用 A_IsSuspended、A_IsPaused）。

还有哪些可用的消息？我提供几点思路供参考：

* 执行操作时通过工具截取消息（消息很多，这些属于 WM_COMMAND）
* 直接到官方论坛询问作者

我比较懒，下面是通过源代码提取的一些消息号（可能不完整，前面部分是消息号分配的说明）：

> 0: unused (possibly special in some contexts)  
> 1: IDOK  
> 2: IDCANCEL  
> 3 to 1002: GUI window control IDs (these IDs must be unique only within their parent, not across all GUI windows)  
> 1003 to 65299: User Defined Menu IDs  
> 65300 to 65399: Standard tray menu items.  
> 65400 to 65534: main menu items  

消息号 | 含义
-|-
65300 | ID_TRAY_OPEN
65400 | ID_FILE_RELOADSCRIPT, ID_TRAY_RELOADSCRIPT
65401 | ID_FILE_EDITSCRIPT, ID_TRAY_EDITSCRIPT
65402 | ID_FILE_WINDOWSPY, ID_TRAY_WINDOWSPY
65403 | ID_FILE_PAUSE, ID_TRAY_PAUSE
65404 | ID_FILE_SUSPEND, ID_TRAY_SUSPEND
65405 | ID_FILE_EXIT, ID_TRAY_EXIT
65406 | ID_VIEW_LINES
65407 | ID_VIEW_VARIABLES
65408 | ID_VIEW_HOTKEYS
65409 | ID_VIEW_KEYHISTORY
65410 | ID_VIEW_REFRESH
65411 | ID_HELP_USERMANUAL, ID_TRAY_HELP
65412 | ID_HELP_WEBSITE

注：一般而言在更新版本时消息号的用途不太可能发生变化，不过为了安全，在使用前最好明确其用途，否则可能发生意外情况。

## 实现的具体原理
在前面的脚本中我们应该注意到两点：

3. 开启了对隐藏窗口的检测
3. 使用 **AutoHotkey** 类名获取窗口

每个 AutoHotkey 运行时都有类名为 AutoHotkey 的主窗口（这个窗口为什么称为主窗口？请参阅 [A_ScriptHwnd](http://ahkcn.sourceforge.net/docs/Variables.htm#prop)），与是否创建自定义 GUI 窗口（自定义窗口的类名为 AutoHotkeyGUI）、是否隐藏托盘图标、是否打开调试窗口（这里的调试窗口即是主窗口，未打开时为隐藏状态）等无关。

说到这里，我想起了一个有趣的事情：为什么 AutoHotkey 被称为模拟多线程呢？从这里可以判断出，每个脚本运行时至少有两个线程：

* 主线程，运行主窗口，负责接受消息、缓冲热键及伪线程的调度（如中断和切换）等  
这里的描述可能不太准确和全面，不过大体上可以这么理解，其中的伪线程是指帮助中所说的[线程概念](http://ahkcn.sourceforge.net/docs/misc/Threads.htm)（例如 [Thread](http://ahkcn.sourceforge.net/docs/commands/Thread.htm) 中所描述的线程，注意帮助中除了 A_ScriptHwnd 外从未涉及到主线程）。
* 脚本线程，实际执行脚本的线程，这不用多说了，它执行的就是我们写的脚本。

从 Microsoft Spy++ 中可以看到，实际上每个脚本也只有两个线程（你多开几个热键，多用几个计时器并不会出现更多线程）。

## 小结
尽管未谈到游戏，不过主要内容前面都说完了。因此，要检测 AutoHotkey 脚本的外挂只需在脚本启动时及运行时定期执行：

```autohotkey
DetectHiddenWindows, on
While WinExist("ahk_class AutoHotkey")
	WinKill
```

游戏不大可能用 AutoHotkey 写，这里的演示大家能看懂它的用途吧？不过这种检测方法只是我猜测的，对于如 AutoIt、按键精灵这样的脚本语言应该具有一定的可行性（如果是 C 语言这种估计没那么容易预防了）。杀一儆百，正常的脚本如改键工具也无法例外了，有了解游戏开发的朋友能否爆个真实情况。

本文与游戏关系不大，最开始的标题为**脚本主窗口的妙用**，不过这个标题是否更好呢？
<!--
## 推荐的脚本管理工具

http://www.ahk8.com/thread-5250.html
Scriptcontrol 1.2
http://ahkscript.org/boards/viewtopic.php?f=6&t=3417
-->
