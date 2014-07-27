---
date: 2014-08-07 14:16:23+08:00
layout: post
title: 正则的调出功能简谈
categories: 进阶
tags: 正则表达式 regex callout
---
导言：一般人接触正则表达式、了解一些基本用法后，遇到普通字符串函数难以或无法处理的文本问题时，就会想到正则表达式了。然而，使用正则过程中，常常会遇到许多问题：

* 每次使用都需要查阅手册熟悉规则
* 每次使用都是痛苦的在一边看手册一边修改（重复猜测-验证的过程）
* 常常写的正则逻辑上分析找不到问题却为何无法匹配

出现这些情况有个很重要的原因是正则表达式在执行时是个黑箱子，我们只能看到输入和输出，却不知道里面究竟怎么执行的。尽管可以通过一些正则辅助工具减轻这种痛苦，不过仍很难比较方便快速的定位并解决问题。

## 调出功能的意义
调出功能相当于在这个黑箱子上打孔，这样我们可以看到里面是怎样进行匹配的。

```autohotkey
Source := "Haystack`nxyz`nabc"
FoundPos := RegexMatch(Source, "m)xyz$")
MsgBox, % FoundPos
```

这里显示为 0，告诉我们它没有找到匹配。从逻辑分析：源字符串中 `xyz` 在换行符之前，在匹配模式中使用了多行匹配选项（即 `m`），这样 `$` 应该可以匹配换行符之前的位置，为什么没有匹配呢？现在开始一步步排除问题，首先直接使用 `xyz` 直接匹配源文本肯定没问题，那么问题会在哪里？

```autohotkey 
Source := "Haystack`nxyz`nabc"
FoundPos := RegexMatch(Source, "m)xyz(?CCallout)$")
MsgBox, % FoundPos

Callout(m) {
  MsgBox, m=%m%
}
```

其中，`(?CCallout)` 是调出语法，详细说明请参阅[正则表达式调出](http://ahkcn.github.io/docs/misc/RegExCallout.htm)。在正则表达式进行匹配过程中，调用了 Callout() 函数且显示此时模式匹配的字符串为 `xyz`，注意调出语法的插入点是在 `xyz` 后但 `$` 之前，它表示在源字符串中寻找到**该插入点之前模式**的匹配字符串时即执行相应的调出函数（并把此时的匹配传递过去），这样说明 `m)xyz` 也是能匹配的，显然问题出在 `$` 上。再仔细看看[正则表达式快速参考](http://ahkcn.github.io/docs/misc/RegEx-QuickRef.htm)会发现，AutoHotkey 中默认的新行符为 \`r\`n，而这里只有单个换行符，所以无法匹配。

顺便和大家分享个小技巧：在使用 `m` 选项时尽量搭配 \`a 选项，保证你会省却很多麻烦。这里的换行符我们可以直接看到，较容易发现，实际的情况就复杂了，如：

```autohotkey
Source =
(
Haystack
xyz
abc
)
```

假设从网上复制这个代码，自己执行时却总是匹配失败，容易想到是换行符的问题吗？

## 返回值的用途
在 AutoHotkey 中，目前 [RegExMatch()](http://ahkcn.github.io/docs/commands/RegExMatch.htm) 和 [RegExReplace()](http://ahkcn.github.io/docs/commands/RegExReplace.htm) 都支持调出功能，这里简单说说它们具体是如何支持的。

> 调出函数最多可以定义 5 个参数：
>
> * Match: 相当于 RegExMatch 中的 UnquotedOutputVar, 包含需要时数组变量的创建.

在调出函数中这个参数最重要，默认保存了对应调出插入点之前那部分模式所匹配的字符串（若有子模式则存于伪数组），通过 `P` 或 `O` 选项分别可保存位置和长度、匹配对象。下面看看调出函数的返回值。

> 模式匹配是继续进行或失败，取决于调出函数的返回值：
>
> * 如果函数返回 0 或没有返回数值，则匹配操作如常进行。
> * 如果函数返回 1 或更大的数字，则在当前位置匹配失败，但继续进行剩余部分的匹配测试。

先看看 RegexMatch 函数：

```autohotkey
Haystack = Whoa, the quick brown fox jumps over the lazy dog.
FoundPos := RegExMatch(Haystack, "(the) (\w+)\b(?CCallout)")

Callout(m) {
    MsgBox m=%m%`nm1=%m1%`nm2=%m2%
    return 0
}
```

此时，在匹配到 `the quick` 后就退出了，显然没有继续匹配，把 Callout() 中返回给调用者的值改为 1 再执行看看。这次看到了什么，还显示了 `the lazy` 对吗？所以，对于 RegexMatch() 若调出函数返回 0 则匹配成功并返回（且把当前匹配位置保存到 FoundPos 中），若返回 1 则继续往后寻找匹配。换句话说，通过让调出函数返回 1 我们可以执行一次 RegexMatch 即可获取源字符串中符合指定模式的所有匹配（不用再像以前只能通过循环了）。

现在到 RegexReplace 函数：

```autohotkey
Haystack = Whoa, the quick brown fox jumps over the lazy dog.
NewText := RegExReplace(Haystack, "(the) (\w+)\b(?CCallout)", "the wild")

Callout(m) {
    MsgBox m=%m%`nm1=%m1%`nm2=%m2%
    return 0
}
```

现在 Callout() 返回 0，那么替换了几次？嗯，两次，即在第一次替换后继续往后搜索，直至替换所有匹配。把返回的值改为 1 看看，出现什么情况了？提示找到了两次，但都没有替换，这里表示调出函数的返回值可以控制 RegexReplace 是否进行替换。

小结：调出函数返回值为 0 时匹配操作**如常进行**，但对于这两个函数是不一样的。此时，RegexMatch 匹配成功并返回，而 RegexReplace 则进行本次替换并继续往后搜索。而返回值为 1 时，则 RegexMatch 匹配失败并继续往后搜索，RegexReplace 则不进行本次替换并继续往后搜索。尽管还有其他返回值，等大家自行探索啦！

## 异想天会开吗？

这个例子改自 ahk8.com 一个问题的答案：

```autohotkey
Text =
(
1 Lesser Heal
2 Fen Creeper
2 Holy Nova
1 Mind Control
)
CardList := {"Lesser Heal":"次级治疗术","Fen Creeper":"沼泽爬行者","Holy Nova":"神圣新星","Mind Control":"精神控制"}

NewText := RegExReplace(Text,"`amO)^([12] )(.+)$(?CCallout)", r)
MsgBox % NewText
Return

Callout(o){
	global CardList, r
  r := o[1] . CardList[o[2]] "`n"
  return 0
}
```
这个脚本的目的是希望把源文本中特定字符串根据对应关系替换为相应的字符串，可以在循环中通过普通字符串函数实现，不过这里是想在正则调出中进行替换：在调出函数中为 Replacement 参数赋值并用于 RegexReplace 函数中的替换。这个操作乍一看似乎行，实际不起作用，因为执行 RegexReplace 函数时 r 是空的即此时已经被替换为空字符串，之后 r 值的变化不会产生影响。
要获取替换后的字符串却是可行的，只需把 Callout() 函数中赋值 r 的语句替换，这时替换后的字符串会保存在变量 r 中了（不是 NewText）：

{% highlight ahk %}
r .= o[1] . CardList[o[2]] "`n"
{% endhighlight %}

## 小结
通过调出功能揭示了正则表达式的内部匹配机制，可帮助找出匹配不成功的问题点（在怀疑处前后都进行调出，若之前匹配而之后没有则找到问题了），对于解决正则表达式的问题有直接的帮助。当然，只要你喜欢，可以在一个模式中多次使用调出语法，让这个黑箱子四处透光，不用担心有人会找你麻烦。
