---
date: 2014-07-22 06:10:53+08:00
layout: post
title: 字符串排序之应用
thread: 5
categories: 实例
tags:
---
字符串处理是脚本中最常见的一种操作，诚然，在这种操作中正则表达式的强大毋庸置疑，不过大多数人要使用它首先需要进行学习。在实际中，普通的字符串处理命令/函数能解决大量常见的情况，例如 [SubStr\(\)](http://ahkcn.github.io/docs/Functions.htm#SubStr) 和 [Instr\(\)](http://ahkcn.github.io/docs/Functions.htm#InStr) 的功能，在本文中将以具体实例介绍 [Sort](http://ahkcn.github.io/docs/commands/Sort.htm) 一些用法的灵活应用。

## 来自论坛的问题
为了方便之后分析，这里把[整个问题](http://ahk8.com/qa/255/)引用过来：

> d:\1\文件夹下有一大堆pdf文件, 我现在想用pdftk将它们合并,但是怎么先按页码大小排序呢?  
> 文件名如下, 规律是这样的: 最后一个~符号开始后面就是页码,比如第一个就是166页到210页:(1,2,3,4,5,8,9.pdf 这个则始终应该是排放最开头的,可以想成是0.pdf.)  
>> d:\1\E736B0D95D3C7D0D6D4EA4BF466F22D03yhiahzU~ahzU79~04~166-210.pdf
>> d:\1\B9DF7166549AC8737AC402DC5F132D5B3yhiahzU~ahzU79~04~1-15.pdf
>> d:\1\0C73455F2E19ECE5E903095930199CE33yhiahzU~ahzU79~04~1,2,3,4,5,8,9.pdf
>> d:\1\946349152F37C6BD5561E90C9C6D79463yhiahzU~ahzU79~04~211-227.pdf
>> d:\1\D3E0FF0E0DA73972F43A8DB89BD540153yhiahzU~ahzU79~04~116-165.pdf
>> d:\1\AD872772E42DCFA4254B978B5B724B0F3yhiahzU~ahzU79~04~66-115.pdf
>> d:\1\72EF1017BB29B0302F7E57419E8999D83yhiahzU~ahzU79~04~16-65.pdf

> 能怎么按页码从小到大排序后放到变量里呢?  
> 比如这样:  
>> A=d:\1\0C73455F2E19ECE5E903095930199CE33yhiahzU~ahzU79~04~1,2,3,4,5,8,9.pdf
>> B=d:\1\B9DF7166549AC8737AC402DC5F132D5B3yhiahzU~ahzU79~04~1-15.pdf 
>> C=d:\1\72EF1017BB29B0302F7E57419E8999D83yhiahzU~ahzU79~04~16-65.pdf
>> ....  

> 然后我就好调用了:  
> run d:\PDFtk\bin\pdftk.exe %A% %B% %C% output "d:\1\合并.pdf"  

## 具体分析问题
首先，这肯定属于字符串处理，且为排序问题。字符串排序？自然联想到  [Sort 命令](http://ahkcn.github.io/docs/commands/Sort.htm)。

* 如果您什么想法都没有，这是基本的熟悉程度待提升。很多命令可能一时用不到，但需要基本了解，最基本的可以从查看帮助目录和[命令与函数索引](http://ahkcn.github.io/docs/commands/index.htm)开始。

不过粗略观察后可以发现，直接用 Sort 显然不行，需要进行前期处理，例如：
`d:\1\72EF1017BB29B0302F7E57419E8999D83yhiahzU~ahzU79~04~16-65.pdf`
为了排序，可以先转换成 `16-65.pdf`，这通过字符串替换完成，实际可以有多种操作方法：

* 通过 Instr() 找到**最后一个 ~**（或 `~ahzU79~04~` 的位置，接着 SubStr() 提取（可自由选择类似的命令，这里是我个人习惯）；
* 通过正则表达式利用 [RegExReplace\(\)](http://ahkcn.github.io/docs/commands/RegExReplace.htm) 提取，我想会正则的同学很容易考虑到这点吧，这里不作为首要考虑；

实际中，可以先重命名文件、接着排序新文件名、然后直接构造为 pdftk.exe 的参数（无需又麻烦的赋值给 A、B、C 了）。此外，如果不重命名文件则需考虑用自定义排序（请参照帮助）。
上面是我的思路，下面说说我的具体方法，如果您有兴趣先自己动动手？
***
## 我的实现脚本
实现的是否方便往往与对工具的熟悉程度有直接关系（当然对问题的理解也很关键），我个人观点是一般的脚本能用就行，如果没有特殊情况或兴趣却花费大量时间去优化很可能得不偿失。看看具体的脚本：

{% highlight ahk linenos %}
Str = 
(
d:\1\E736B0D95D3C7D0D6D4EA4BF466F22D03yhiahzU~ahzU79~04~166-210.pdf
d:\1\B9DF7166549AC8737AC402DC5F132D5B3yhiahzU~ahzU79~04~1-15.pdf
d:\1\0C73455F2E19ECE5E903095930199CE33yhiahzU~ahzU79~04~1,2,3,4,5,8,9.pdf
d:\1\946349152F37C6BD5561E90C9C6D79463yhiahzU~ahzU79~04~211-227.pdf
d:\1\D3E0FF0E0DA73972F43A8DB89BD540153yhiahzU~ahzU79~04~116-165.pdf
d:\1\AD872772E42DCFA4254B978B5B724B0F3yhiahzU~ahzU79~04~66-115.pdf
d:\1\72EF1017BB29B0302F7E57419E8999D83yhiahzU~ahzU79~04~16-65.pdf
)
StringReplace, Str, Str, ~, ~~\, All
Sort, Str, N \
StringReplace, Str, Str, ~~\, ~, All
MsgBox, % Str
{% endhighlight %}

仔细观察后发现，目标字符串中要比较的那部分子字符串都为数值，且该子字符串前的字符序列比较固定，都为 `~`（更长一些则为 `~ahzU79~04~`），这样就具备了使用 Sort 中 \ 的条件，因此代码出来了：

{% highlight ahk linenos %}
Sort, Str, N \
{% endhighlight %}

在比较之前先替换就行了，不再赘述。有趣的是，当我在 Vim 中写出上面的代码时，我又发现一个更简便的方法：

{% highlight ahk linenos %}
Sort, Str, N P57
{% endhighlight %}

从第 57 个字符开始比较，显然这种方法得益于观察，因为我 Vim 中用等宽字型，一眼就能看出待比较子字符串的起始位置相同（在上面代码块中也能看出来，但最初在论坛看到问题时我没发现）。显然，这里打破了我之前**无法直接使用 Sort** 的想法。

* 尽管这里 `d:\1\0C73455F2E19ECE5E903095930199CE33yhiahzU~ahzU79~04~1,2,3,4,5,8,9.pdf` 会排在第二位，但这是次要问题。

最后，问题到这里并非终结，运用之妙存乎一心，如果您有好例子或好问题，欢迎交流。为了分享我常常去构造应用场景，效果往往不理想，让我想到教科书式的苍白无力，而实际中的使用则是那么的生动活泼。

## 必要的补充
前面没有说完美的解决，真不仅只是小心，现在问题来了。849112292 在[问题评论](http://ahk8.com/qa/255/?show=272#c272)中指出：

> \ 和 N 选项不能同时起效  

我太大意了，一段时间不用很多要点就忘了，以前翻译帮助时这些印象都非常深刻的。您发现了吗？如果没有，那么您动手了吗？
思路还是简单，直接按位置排序，要数值比较则需先重命名，自定义排序仍然行的通。继续发现问题吧，不过思路我只能分析到这里了。
