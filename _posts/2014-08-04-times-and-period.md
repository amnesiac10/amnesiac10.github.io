---
date: 2014-08-04 06:10:54+08:00
layout: post
title: 次数与时长
thread: 18
categories: 热键
tags:
---
在热键方面，AutoHotkey 扩展了很多概念和适用范围，例如平常所说的热键一般指前一篇所说的组合键，但在 ahk 中鼠标键、滚轮或游戏操纵杆按钮都可以作为热键。换句话说，这里介绍的方法都应该根据实际需要灵活应用。（与热键相关的一个概念是加速键，常出现的图形用户界面的元素中加下划线显示并使用 Alt 触发。）

## 按次数（单击与多击）

以常见的双击为例，双击实际上为短时间内两次连续的单击，包含了两个必要条件：

* 两次单击的时间间隔必须在短时间内
* 两次单击必须是连续的

在这个示例中（来自[帮助](http://ahkcn.sourceforge.net/docs/)并做适当修改），使用了两个判断条件：

```ahk
intInterval := 500 ; 若两次连击在这个时间间隔中，则视为双击。
~RControl::
if (A_PriorHotkey <> "~RControl" or A_TimeSincePriorHotkey > intInterval)
{
    KeyWait, RControl
    return
}
MsgBox, 您双击了右边的 Ctrl 键。
return
```

在第一次按下右 Ctrl 键时，会被判断为不是双击，所以什么也没做。第二次则会显示提示框。在实际建立双击热键时，经常需要考虑两点：

* 减少对该键单击的影响
* 减少对该键其他组合键的影响

因此，对于 Control 这种本身作为修饰键（几乎不作为单键使用）配合 `~` 修饰符比较好，如果像字母键这样的普通键应首先考虑使用 [Send](http://ahkcn.sourceforge.net/docs/commands/Send.htm) 模拟（在这个脚本中则需在首次单击时发送，检测为双击后撤销前一次发送并执行双击操作）。

既然双击可行，那么三击或更多呢？在实际中不常用，不过思考还是挺有趣的。下面是多次连击的实用例子，设计的很精巧：

```ahk
CapsLock::
if (A_ThisHotkey <> A_PriorHotkey)
    return
intCount := (intCount = "" ? 1 : intCount + 1)
SetTimer, MultiPresses, -500
Return

; 这里根据连续按下次数判断执行的操作。
MultiPresses:
if (intCount = 1)
    MsgBox, 这是单击。
else if (intCount = 2)
    MsgBox, 这是双击。
else if (intCount = 3)
    MsgBox, 这是三次连击。

intCount := "" ; 重置记数器。
Return
```

注意到了吗？其中计时器的周期是负的。此外，这里直接屏蔽了系统原来的功能，参照前面双击的例子可以很容易恢复。再看帮助中一个例子（做了修改）：

```ahk
#c::
if (intCount > 0) ; SetTimer 已经启动，这里记录键击。
{
    intCount += 1
    return
}

intCount = 1
SetTimer, KeyWinC, 400 ; 在 400 毫秒内等待更多的键击。
return

KeyWinC:
SetTimer, KeyWinC, off
if (intCount = 1) ; 此键按下了一次。
{
    Run, m:\
}
else if (intCount = 2) ; 此键按下了两次。
{
    Run, m:\multimedia
}
else if (intCount > 2)
{
    MsgBox, 连击了三次或更多。
}
intCount = 0
return
```

和前一个完全一样吗？如果不去执行，可能不容易明白这两者的运行机制区别在哪里，这里给点提示：它们的计时方式不相同。比较而言，前一个较为符合我们对多击的理解，可能实用一些。

前两个例子尽管看起来长一些，实际上很好理解，但下面这个虽短却需好好思考及实践才会有点头绪：

```ahk
#MaxThreadsPerHotkey 5
#Space::  ; Win+Space 热键.
#MaxThreadsPerHotkey 1
intCount := (intCount ? (intCount + 1) : 1)
Sleep, 1000
if (intCount = 1)
    MsgBox, 这是单击。
else if (intCount = 2)
    MsgBox, 这是双击。
else if (intCount = 3)
    MsgBox, 这是三次连击。

intCount := "" ; 重置记数器。
Return
```

看不懂也没关系，用起来效果和第一个类似（使用指令时必须注意是全局还是局部的）。

这里举这么些例子（还有其他花样），不是为了看起来多厉害，而是在实际中经常必须有所选择。很多人不理解，为什么需要 SendRaw / SendInput / SendPlay / SendEvent 那么多类似的命令呢？在应用时实际情况比较复杂，而这些情况很难在程序中自动做出最优选择，所以需要平时多使用多实践才会找到感觉：什么情况下使用哪个会更有效（这是最让初学者困惑的问题之一）。

## 按时长

根据按住按键的时长执行不同的操作，可能实际中比上一种用法更少见。

```ahk
Esc:: ; 在按下时触发。
If StartTime
    return
StartTime := A_TickCount
return

Esc up:: ; 在弹起时触发。
TimeLength := A_TickCount - StartTime
if (TimeLength < 200)
{
    MsgBox, 您按住退出键不到 200 毫秒。
}
else if (TimeLength < 1500 and TimeLength >= 200) ; 后一条件实际是多余的，加上只是为了更清晰。
{
    MsgBox, 您按住退出键 1 秒左右。
}
StartTime := ""
return
```

代码中已经自行解释了，这里补充一点是触发的时间点，在写普通热键时经常需要考虑到这点（主要是修饰键和普通键的具体表现的区别）。另外，Esc:: 热键写成这样为什么不行呢？

```ahk
Esc::StartTime := A_TickCount
```

请看下面两图，可以看出长按一个按键时我们可能认为只会发送一次按下事件，但实际上它会发送连续的按下事件，且这些事件都触发了 Esc:: 热键：
![2014-07-01 105102.png](http://upload-images.jianshu.io/upload_images/19661-98ef0ae659f7cbbb.png)
![2014-07-01 105116.png](http://upload-images.jianshu.io/upload_images/19661-f4ff31991f6bd846.png)
因此，编写一个脚本时如果运行结果和预期不一致，调试是找出问题很好的方法。在实际中，有时可能用 [Hotkey 命令](http://ahkcn.sourceforge.net/docs/commands/Hotkey.htm)效果较好：

```ahk
Esc:: ; 在按下时触发。
If StartTime
    return
StartTime := A_TickCount
Hotkey, Esc up, EscUpSub, On
return

EscUpSub:
Hotkey, Esc up, EscUpSub, Off
TimeLength := A_TickCount - StartTime
if (TimeLength < 200)
{
    MsgBox, 您按住退出键不到 200 毫秒。
}
else if (TimeLength >= 200 and TimeLength < 1500)
{
    MsgBox, 您按住退出键 1 秒左右。
}
StartTime := ""
return
```



