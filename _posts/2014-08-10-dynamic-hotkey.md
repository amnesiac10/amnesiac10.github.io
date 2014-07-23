---
date: 2014-08-01 10:16:23+08:00
layout: post
title: 伪数组与关联数组
thread: 24
categories: 热键
tags: hotkey input A_PriorKey 
---

引子：有些文章只看它的标题，您就看不出它的内容，则很可能错过它的精彩，本文即是其中一例。

使用双冒号语法可以快速创建热键，简单、直接，非常方便。有时我们需要经常修改一些热键或把脚本给别人使用， 通常必须考虑把热键放在专门的配置中以方便修改，此时就需要动态实现热键了。

## 使用 hotkey 命令

这个命令本身的用法简单，这里结合常见的具体场景介绍：

{% highlight ahk %}
; 为了简便这里直接赋值变量（实际情况中可从配置文件读取）：
MyHotkey := "F1"

; 下面这个热键仅在记事本中有效：
Hotkey, IfWinActive, ahk_class Notepad
Hotkey, %MyHotkey%, MyLabel_1
; 下面这个热键为全局热键：
Hotkey, If
Hotkey, %MyHotkey%, MyLabel_2
; 下面这个热键在记事本或写字板窗口活动时有效：
Hotkey, If, WinActive("ahk_class Notepad") || WinActive("ahk_class WordPadClass")
Hotkey, %MyHotkey%, MyLabel_3
Return

; 这里使用不同的标签，是便于调试具体生效的是哪个变体：
MyLabel_1:
MyLabel_2:
MyLabel_3:
MsgBox, 您按下了 %A_ThisHotkey% 热键（%A_ThisLabel%）。
Return

; 上面的第三个热键需要该指令才起作用（注：这里的 `||` 替换为 `or` 无效）：
\#If, WinActive("ahk_class Notepad") || WinActive("ahk_class WordPadClass")
{% endhighlight %}

下面进行简单的分析：

{% highlight ahk %}
Hotkey, IfWinActive, ahk_class Notepad
{% endhighlight %}

设置热键生效的窗口条件，可使用窗口存在/不存在/活动/不活动（IfWinActive/IfWinNotActive/IfWinExist/IfWinNotExist），效果等同于对应的系列指令（不过作用对象有区别，一个是双冒号热键，一个是 hotkey 热键）。

{% highlight ahk %}
Hotkey, IfWinActive
{% endhighlight %}

取消热键条件，其中的 IfWinActive 可替换为 IfWinExist/IfWinNotActive/IfWinNotExist/If（注：这里还包括 If）。

{% highlight ahk %}
Hotkey, IfWinActive, ahk_class Notepad
Hotkey, IfWinActive, ahk_class WordPadClass
Hotkey, %MyHotkey%, MyLabel_1
{% endhighlight %}

这种条件与对应指令类似，效果是互斥的，所以上面这个热键不会在记事本和写字板中都生效。

{% highlight ahk %}
Hotkey, If, WinActive("ahk_class Notepad") || WinActive("ahk_class WordPadClass")
{% endhighlight %}

尽管这里的条件仍与窗口有关，不过实际上可以任意表达式，只需满足存在相应的 #If 指令且它们包含的表达式完全一致。

{% highlight ahk %}
\#If, WinActive("ahk_class Notepad") || WinActive("ahk_class WordPadClass")
{% endhighlight %}

原本 #If 指令是与位置有关，但由于这个脚本中没有双冒号热键和热字串，所以这里放在什么位置关系不大。

关于**热键优先级**：一般而言，钩子热键优先级最高，最近启用的优先级更高，局部变体优先级高于全局变体（多个局部变体都有效时最先启用的优先级更高）。前面的有些结论可能仅适用于同一脚本而言，不同脚本及与其他程序之间实际情况比较复杂。

热键的优先级与启用顺序和作用范围有关，但先创建的并不总是高优先级。

双冒号热键和 Hotkey 创建的热键是分别管理的，后者是通过该命令的选项管理自身创建的热键，具体请参阅帮助。Hotkey 命令不能直接启用或禁用脚本中不是它创建的热键，但在大多数情况下它可以通过创建或启用相同的热键来覆盖它们。

## 使用 Input 命令

> 请用 AutoHotkey 实现按任意键继续的功能。

请思考，您会如何实现？很容易想到命令提示符中的 pause 命令，在批处理中执行某些操作前先提示用户，随意按某个键后继续执行，AutoHotkey 中应如何实现相同功能？也许实际中不一定需要，但思考可以锻炼思维。先看看我的实现：

{% highlight ahk %}
; 建立所有按键列表，尽管可能有些键未包含在其中，这里用于演示。
AllKeyList := "a|b|c|d|e|f|g|h|i|j|k|l|m|n|o|p|q|r|s|t|u|v|w|x|y|z|{LControl}|{RControl}|{LAlt}|{RAlt}|{LShift}|{RShift}|{LWin}|{RWin}|{AppsKey}|{F1}|{F2}|{F3}|{F4}|{F5}|{F6}|{F7}|{F8}|{F9}|{F10}|{F11}|{F12}|{Left}|{Right}|{Up}|{Down}|{Home}|{End}|{PgUp}|{PgDn}|{Del}|{Ins}|{BS}|{Capslock}|{Numlock}|{PrintScreen}|{Pause}"
Loop, Parse, AllKeyList, |
{
    ; 把 Send 参数中发送模拟按键的格式转成热键参数中的按键格式：
    OneKey :=  Trim(A_LoopField, "{}")
    Hotkey, ~%OneKey%, Operation
}
Return

Operation:
MsgBox, 您按下了 %A_ThisHotkey% 键。
Return
{% endhighlight %}

按任意键，就建立“任意键”的热键，思路很明确。现在要转换成“提示用户一些信息并按任意键继续”就简单了，加上一对 ToolTip（在该片段前加带参数的 ToolTip 而 Operation 子程序中加不带参数的）或使用类似的方法。

上面虽然实现了功能，不过从编写效率或移植角度看这种实现不好（如果都用双冒号会更糟糕），热键功能是 AutoHotkey 的特色，有更好的实现吗（其他脚本中蹩脚是正常的）？

{% highlight ahk %}
; 来自帮助（一行命令，干净简洁）：
Input, SingleKey, L1, {LControl}{RControl}{LAlt}{RAlt}{LShift}{RShift}{LWin}{RWin}{AppsKey}{F1}{F2}{F3}{F4}{F5}{F6}{F7}{F8}{F9}{F10}{F11}{F12}{Left}{Right}{Up}{Down}{Home}{End}{PgUp}{PgDn}{Del}{Ins}{BS}{Capslock}{Numlock}{PrintScreen}{Pause}
; 需要注意，由于 SingleKey 只记录按键按下后生成的字符，所以产生不可见字符的按键应放在 EndKeys 参数中。
{% endhighlight %}

这里实际上创建了一批热键，比较而言，Hotkey 创建单个热键很方便，但创建批量热键时应优先考虑 Input。创建批量热键的一种典型的情况是类似码表式输入法，其中需要批量转译输入为对应的输出，一个码表（保存了输入字符组与输出字符组的对应关系），一个平台（脚本中实现转换功能），该平台中核心功能就是 Input：

{% highlight ahk %}
Input, Code, C I L4, {Space}{Enter}{Esc} ; 码表式输入法核心示例。
{% endhighlight %}

例如五笔输入法的特点：最长四码，按空格键可输入一、二、三级简码。当然，实际情况比较复杂，需要处理全角、半角、标点符号和特殊按键，加上候选框就更复杂了。到这里，您理解我之前曾说热字串是序列键（热键的一种）吗？刚才谈论的功能和热字串本源上都是热键。

说到这个命令，必须和大家分享一段代码，实现很精妙（原本想放到 [AutoHotKey 常用函数或小技巧有哪些分享？](http://www.zhihu.com/question/19645501)上的，但可能不容易理解）：

{% highlight ahk %}
; 节选自小众屏幕密码锁（http://www.appinn.com/lock-screen-appinn/）并做了简单调整
Key := "test"
i := 1
Loop ; 锁定后就一直处于循环状态，直到解锁
{
    Input, a, L1
    Temp := SubStr(Key, i, 1) ; 提取 Key 中第 i 位字符
    If (a = Temp)
    {
        i++ ; 准备匹配下一位。
    }
    Else
    {
        i := 1 ; 重头开始匹配。
    }
    If (i= StrLen(Key) + 1)    ; 输入匹配了，退出循环。
    {
        MsgBox, 您输入了正确的通行码。
        Break
    }
}
{% endhighlight %}

前面几句可能不好理解，我们考察开始的几次循环：i=1 时接受一位输入并与 Key 中的第一位比较，相同则 i 自增，否则继续从新开始。i=2 时（第一次的输入已经匹配 Key 中的第一位），继续接受输入一位与 Key 中的第二位比较……若连续 i 位都与 Key 中的第 i 位相同，即刚才输入的字符串已经完全匹配 Key。

为什么不按下面这样实现？

{% highlight ahk %}
Key := "test"
i := 1
Loop
{
    Input, a, % "L" StrLen(Key)
    If (a = Key)
    {
        MsgBox, 您输入了正确的通行码。
        Break
    }
}
{% endhighlight %}

乍一看，他们的功能好像没什么区别，自己动动手比较吧（如果不实践，永远不会知道看起来正常的代码却不会按预期运行的原因）。另外，小众屏幕密码锁也是个很实用的功能，输入密码时别人看不到，也不容易猜出来（先输一些错的字符）。

## 使用 A_PriorKey

上面没有提到，实际上[使用 Input 是有局限性的](http://ahk8.com/thread-2133.html)，如不支持鼠标按键，而通过 [A_PriorKey](http://ahkcn.github.io/docs/Variables.htm#PriorKey) 不仅能检测到按键，还能检测到按钮（需要安装钩子）：

{% highlight ahk %}
\#InstallKeybdHook
\#InstallMouseHook
Loop {
    ToolTip % A_PriorKey
    Sleep 100
} 
{% endhighlight %}

之前说到**任意键**，您意识到按钮了吗？

## 小结

动态热键的用途不限于本文开头介绍的情况， 例如还可以快速创建大量热键。
