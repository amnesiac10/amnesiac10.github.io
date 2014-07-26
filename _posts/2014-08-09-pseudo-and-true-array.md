---
date: 2014-08-09 14:16:23+08:00
layout: post
title: 伪数组与关联数组
categories: 风格
tags: array 数组 伪数组
---

 伪数组应该是 AutoHotkey 中特有的概念，下面的内容需要稍有了解，其中部分内容来自帮助，稍做了调整。

## 伪数组

### 伪数组的含义

伪数组是概念上的：每个伪数组实际上只是一系列连续编号的变量或函数的集合，它们的每一个被视为伪数组的“元素”。AutoHotkey 内部不会以任何方式把这些元素链接在一起。

任何接受 OutputVar 或能赋值的命令都可以用来创建伪数组。最简单的例子是使用赋值运算符（:=），如下所示：

{% highlight ahk %}
Array%j% := A_LoopField
{% endhighlight %}

通过在索引间使用自选分隔符可以创建多维伪数组，例如：

{% highlight ahk %}
Array%j%_%k% := A_LoopReadLine
{% endhighlight %}

### 操作伪数组
下面的例子演示了如何创建和访问伪数组，这里是从文本文件获取一系列的名称：

{% highlight ahk %}
; 写入数据到伪数组：
ArrayCount := 0
Loop, Read, C:\Test.txt   ; 循环获取文件中的每行，一次一行。
{
    ArrayCount += 1  ; 记录伪数组中的项目数，这里不记录后面获取不方便。
    Array%ArrayCount% := A_LoopReadLine  ; 把此行保存到伪数组中的下一个元素。
}

; 从伪数组中读取：
Loop, %ArrayCount%
{
    ; 下一行使用 := 运算符获取伪数组元素：
    element := Array%A_Index%  ; A_Index 是内置变量。
    MsgBox, % "索引号" . A_Index . "的元素的值为" . Array%A_Index%
}
{% endhighlight %}

看过这两个例子后，里面的操作中我们只是连续的操作变量罢了，与操作普通变量在性质上没有区别。唯一的区别是名称，这系列变量的名称一般后面部分为数字，比较典型的如 StringSplit、WinGet List 及 RegExMatch 这些命令创建的伪数组。还有些命令虽然会创建一系列变量，变量的名称却不是以数字结尾，如 GuiControlGet Pos、SysGet 等。

在帮助（v1.1.14.04）中搜索“伪数组”即可找到创建伪数组的所有方法。

## 关联数组

关联数组类似于其他语言中的数组，这是一种数据结构，本质上是一种特殊的对象。

### 操作关联数组

自包含关联数组可以使用 Object() 创建. 例如:

{% highlight ahk %}
; 创建数组后，初始为空：
Array := Object()

; 写入数据到数组:
Loop, Read, C:\Guest List.txt ; 依次获取文件中的每行文本。
{
    Array.Insert(A_LoopReadLine) ; 添加到数组中。
}

; 从数组中读取数据，在一般情况下建议使用这种方式（即 for 循环）：
for index, element in Array
{
    MsgBox, % "索引号为" . index . "的元素的值为" . element
}
{% endhighlight %}

这个例子仅演示了对象提供的功能中很小的一部分，还可以可以设置、获取、插入、移除和枚举项目。除了数字，还可以把字符串和对象作为键使用。对象可以作为值存储到其他对象中并且可以作为函数参数或返回值传递。对象还可以用新功能进行扩展。

### For 循环与 Loop 循环

对于这样特殊的关联数组，还可以把上面的 For 循环替换为 Loop 循环：

{% highlight ahk %}
; 使用传统方式
Loop, % Array.MaxIndex()
{
    ; 使用“Loop”，索引必须是连续的数字，从 1 到数组中元素的个数（或者必须在循环中进行计算）。
    MsgBox, % "索引号为" . A_Index . "的元素的值为" . Array[A_Index]
}
{% endhighlight %}

实际上不建议使用 Loop 对关联数组进行循环操作（很蹩脚），上面的例子中数组的索引从 1 开始且是连续的整数，所以可以使用 Loop 循环。然而，关联数组中的键可以为字符串、整数或对象，即使键为整数时还可能是稀疏分布的，例如 {1:"a",1000:"b"}，在这些一般情况下都无法使用 Loop 代替。

## 关联数组与伪数组的比较

尽管 Insert() 和枚举数有它们的用途, 不过一些用户可能会发现使用它们比用更传统的方式容易些。下面的例子中把伪数组和关联数组的操作方式进行比较，其中注释中的为伪数组的操作方式：

{% highlight ahk %}
; 与变量可直接使用不同，数组在使用前必须初始化：
Array := Object()

; Array%j% := A_LoopField
Array[j] := A_LoopField

; Array%j%_%k% := A_LoopReadLine
Array[j, k] := A_LoopReadLine

ArrayCount := 0
Loop, Read, C:\Guest List.txt
{
    ArrayCount += 1
    ; Array%ArrayCount% := A_LoopReadLine
    Array[ArrayCount] := A_LoopReadLine
}

Loop, %ArrayCount%
{
    ; element := Array%A_Index%
    element := Array[A_Index]
    ; MsgBox % "Element number " . A_Index . " is " . Array%A_Index%
    MsgBox, % "Element number " . A_Index . " is " . Array[A_Index]
}
{% endhighlight %}

这个是帮助中的例子，但实际意义不大，因为它是**以伪数组的方式在伪数组的功能上进行的比较**。例如使用 ArrayCount 变量保存数组元素个数，我们知道这对于关联数组是多余的，它的元素个数是由程序维护，可以直接使用 MaxIndex() 方法获取，而且获取值时无需通过元素个数，可直接 for 循环。

帮助中提到这个比较会“让大家更容易从原来使用伪数组的习惯中过渡过来”，不过我个人认为这样做是否像用汉字标出英语单词的发音来学英语呢？

## 小结

目前而言，在支持创建伪数组的命令中还是需要伪数组（伪数组存在的必要性），在其他地方关联数组具有优势（关联数组的功能强大、使用方便），即关联数组尚无法完全取代伪数组（即使作为中间产物），尽管我推荐在一般情况下使用关联数组。

至于 AutoHotkey v2 中将如何，我们且拭目以待。以后提到“数组”时都指关联数组，对于“伪数组”会直接以伪数组表示。
