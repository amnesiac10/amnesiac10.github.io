---
date: 2014-08-16 06:10:54+08:00
layout: post
title: 传统形式与表达式
thread: 17
categories: 风格
tags: If 赋值
---

本篇中主要专注于 AutoHotkey 中容易引起困惑的传统形式和表达式两种风格，进行比较并提供一些建议，以尽可能去除在学习或使用过程中的困惑。

## 从示例开始

有人在论坛中提问（[加不加括号，效果截然不同 • AHKScript](http://ahkscript.org/boards/viewtopic.php?f=27&t=3357)），在帮助文档 ListView 的一个示例中（注：这里只是片段，完整代码请参阅帮助 [ListView 页面](http://ahkcn.sourceforge.net/docs/commands/ListView.htm)）：

{% highlight ahk %}
GuiContextMenu:
if A_GuiControl <> MyListView
    return

Menu, MyContextMenu, Show, %A_GuiX%, %A_GuiY%
return
{% endhighlight %}

然后他做了点小小的修改（把 If 后的部分加在括号中）：

{% highlight ahk %}
GuiContextMenu:
if (A_GuiControl <> MyListView)
    return

Menu, MyContextMenu, Show, %A_GuiX%, %A_GuiY%
return 
{% endhighlight %}

运行时他发现效果完全不同了：修改前在 ListView 控件上点击才显示右键菜单，而修改后则是在窗口的空白处点击才显示菜单，很不理解。

为什么会这样呢？因为他在不理解 If 传统型和 If（表达式）就加上括号，这是产生问题的根源，加上括号则成了表达式（在修改后的脚本中），同时 MyListView 成了变量，但该脚本中不存在这个变量，实际上换成任何一个脚本中不存在的变量名效果都一样（不过这里的名称有迷惑性，让人误解它有特定意义）。为什么还能运行？这必须提到 **AutoHotkey 的变量机制：使用即存在**，换句话说，大部分情况下当我们需要变量时直接使用就行了。如果是其他语言，一般会出现变量不存在的提示。

从另一角度看，困惑的来源是为什么会存在容易引起误解的两种风格呢？AutoHotkey 最初参照自典型命令式语言 AutoIt v2，这是传统型存在的历史原因，因为有些情况下传统型无法解决，在开发过程中加强了表达式型，所以目前形成并存的局面。

## 传统赋值与表达式赋值

{% highlight ahk %}
str = 这是个字符串。 ; 传统赋值
str := "这是个字符串。" ; 表达式赋值
{% endhighlight %}

上面这两个赋值语句是等效的，可以看到传统赋值在变量名后使用等号接着是作为赋值内容的字符串。而表达式赋值使用冒号等号，后面的赋值内容括在引号中。看起来似乎传统赋值简单一些，这里确实如此，然而它只适用于赋值字符串、数字、变量或它们组合的内容，并且在包含变量时变量名称必须括在百分号中。现在在看看赋值内容包含字符串和变量的例子：

{% highlight ahk %}
str = 这是个字符串，后面跟着 Var 变量的内容：%var%
str := "这是个字符串，后面跟着 Var 变量的内容：" var
{% endhighlight %}

这时可以发现传统赋值和表达式赋值复杂度上接近，进一步的，如果只赋值变量，显然表达式方式简单了。它们的区别主要不在这里，请看这个例子：

{% highlight ahk %}
var = 1 + 1
var := 1 + 1
{% endhighlight %}

保存为脚本，运行看看结果。这里说明了它们的主要区别，表达式赋值时可以进行运算，操作数可以为数字、字符串、函数调用等。这种情况时无法用传统形式代替。

如果能明确它们的区别，可以两者都用。如果担心会混淆，那么只能采用表达式赋值了，这是我对初学者建议的方法。

## If 传统型与 If 表达式型

这里包含好几个命令及等价的比较结构，除了 If 表达式型外我唯一建议使用的是下面这种形式：

{% highlight ahk %}
If var
    MsgBox, var 变量既不为零也不为空。
{% endhighlight %}

这里 If 语句中的 var 是使用这个变量的值，而 MsgBox 命令中则使用这个变量的名称。下面是其他一些句式：

> IfEqual, var, value (等同于: if var = value)  
> IfNotEqual, var, value (等同于: if var <> value) (可以使用 != 代替 <>)  
> IfGreater, var, value (等同于: if var > value)  
> IfGreaterOrEqual, var, value (等同于: if var >= value)  
> IfLess, var, value (等同于: if var < value)  
> IfLessOrEqual, var, value (等同于: if var <= value)  

这些命令都很简单，名称的含义也显而易见。第一个变量是不需要百分号括住的变量，第二个是原义的值。可以使用两种形式：

{% highlight ahk %}
IfEqual, var, value
    MsgBox, 变量 var 的值为：value。

if var = value
    MsgBox, 变量 var 的值为：value。
{% endhighlight %}

这两种形式完全相同，后一种形式虽然含有等号，但这里不是表达式。现在看看使用表达式的形式：

{% highlight ahk %}
if (var = "value")
    MsgBox, 变量 var 的值为：value。
{% endhighlight %}

传统形式比较简单，但也只能比较最简单的情况。下面这个例子则无法使用传统形式代替了：

{% highlight ahk %}
if (Mod(intYear, 100) ? Mod(intYear, 400) : Mod(intYear, 4))
    MsgBox, % intYear "不是润年。"
{% endhighlight %}

还有很多情况无法使用传统形式代替，同时接受两种很可能产生混淆，**一是变量什么时候应括在百分号中，二是值什么时候要加引号。**

## 两种同时使用不行吗？

在只赋值字符串时使用传统形式，而需要运算时则通过表达式，两者都很简单，为什么非要两者选一呢？这样的问题不是“行不行”，既然语言自身提供了，当然不会不行。主要问题在于如果没有理解，那么会给您带来困惑，即让您的脚本增加出错的可能。

请看这个例子：

{% highlight ahk %}
if Var = ""
    MsgBox, 变量 Var 为空。
{% endhighlight %}

请闭上眼睛几秒钟，想想什么时候会显示消息框？

**【=====================分隔区域，请暂停往下看=====================】**

您刚才是否在想当 Var 变量为空时这个 If 语句结果为真呢？这样就错了，实际是只有在 Var 包含一对双引号时才为真。如果要判断一个变量是否为空，正确的传统形式和表达式形式应该这样：

{% highlight ahk %}
; 传统形式
if Var =
    MsgBox, 变量 Var 为空。

; 表达式形式
if (Var = "")
    MsgBox, 变量 Var 为空。
{% endhighlight %}

所以建议完全使用表达式形式，当然在理解的基础上一起用是没问题的。

## 小结

最后，[从 AutoHotkey Basic 到 AutoHotkey_L]({{ site.url }}/2014/08/02/choose-versions.html) 中表达式型一直在加强，个人猜测 AutoHotkey v2 版本可能完全转向表达式型（目前测试版中尚未实现，但在 [Thoughts for v2.0](http://ahkscript.org/v2/v2-thoughts.htm) 中 Lexikos 有这种倾向），所以不论从目前的理解或以后的学习、过渡而言，建议采用表达式型。
