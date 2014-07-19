---
date: 2014-08-06 06:10:54+08:00
layout: post
title: 文件处理之传统与对象
thread: 20
categories: 风格
tags:
---
注：对于 AutoHotkey，初学者几乎都会困惑的一点是可选的风格很多，初学者多感觉语法凌乱，而熟悉的人则认为灵活多样，这点是好是坏见仁见智了。不过这种现象的产生有一定的历史原因，以后可能会做更详细的说明。

与文件系统交互是脚本中最常见的操作之一。本文从文件处理的角度比较传统方式和对象方式的一些差异，以便整体上对它们有一定的理解，避免混乱。比较而言，两种方式的差异：

* 命令较简单（有批处理的感觉），能满足大部分需求 ；
* 对象则更灵活，可进行更精细的控制，用的好也更高效；

## 文件处理的传统命令

AutoHotkey 中主要为操作文件提供了下列命令（和函数）：

> FileAppend：附加内容到文件末尾，若文件不存在则首先创建文件。  
> FileDelete：删除文件。  
> FileRead/FileReadLine/文件读取循环：读取整个文件或某行文件的内容，或者从文件头部开始逐行获取文件的内容。  
> FileExist()：判断文件、文件夹是否存在。  
> 文件和文件夹循环：遍历文件系统中指定位置的文件和文件夹。  
> 其他命令：与文件相关的其他命令。   

首先设置需要操作的目标文件：

{% highlight ahk linenos %}
strFileName := "C:\Test.txt"
{% endhighlight %}

### FileAppend

附加一些内容到文件末尾，当目标文件不存在时会首先创建文件：

{% highlight ahk linenos %}
FileAppend, 需要追加的文本。`n, %strFileName%
{% endhighlight %}

### FileDelete

删除文件。例如：

{% highlight ahk linenos %}
FileDelete, %strFileName%
{% endhighlight %}

如果需要覆盖文件，则需要首先使用 FileDelete 删除文件（传统方式中没有直接覆盖的命令）。

### FileRead/FileReadLine/文件读取循环

使用 FileRead 可以一次性读取整个文件的内容：

{% highlight ahk linenos %}
FileRead, strFile, %strFileName%
{% endhighlight %}

如果只需要读取文件的少数几行，请使用 FileReadLine：

{% highlight ahk linenos %}
intLineNum := 1 ; 设置需要读取的行号。
FileReadLine, strRow, %strFileName%, %intLineNum%
{% endhighlight %}

如果需要逐行处理文件内容，则使用循环比较方便（比 FileReadLine 效果更好）：

{% highlight autohotkey %}
strObjectFileName := "C:\Object.txt" ; 设置用来存放处理后的数据的目标文件。
Loop, Read, %strFileName%, %strObjectFileName%
{
  MsgBox, 第 %A_Index% 行的内容为 %A_LoopReadLine%。
}
{% endhighlight %}

注意：只有在文件读取循环中才存在 A_LoopReadLine 变量。此外，在这种循环中需要写入文本时建议使用仅带一个参数（写入的文本）的 FileAppend 命令，这样执行地更高效。

### FileExist()

判断文件、文件夹是否存在。例如：

{% highlight ahk linenos %}
if FileExist(strFileName)
  MsgBox, 目标存在。
{% endhighlight %}

### 文件和文件夹循环

使用 Loop 循环遍历文件系统中指定位置的文件和文件夹（与上面读取文件内容不同）：

{% highlight ahk linenos %}
strObjectDir := "C:\"
Loop, %strObjectDir% ; 仅获取文件。
{
  MsgBox, 在 %strObjectDir% 中第 %A_Index% 个文件的名称为 %A_LoopFileName%。
}
{% endhighlight %}

注意：这个 Loop 循环中存在许多自己特有的变量，许多时候很有用。

### 其他命令

上面介绍了操作文件、文件夹时的主要命令，还有一些较不常用的，这里提一下：

> FileCopy：复制文件。  
> FileCopyDir：复制目录（类似批处理中的 xcopy）。  
> FileCreateDir：创建目录。  
> FileMove：移动或重命名文件。  
> FileMoveDir：移动或重命名目录。  
> FileRemoveDir：删除目录。   

### 小结

上面介绍了文件、文件夹操作的主要命令，其中的读取、写入命令大多数情况下都是操作文本文件，不过一些也能操作二进制文件。另外，对于与文件编码、大小、属性等方面相关的命令，这里没有进行说明，请参阅帮助。

## 文件对象
### 创建文件对象

使用 FileOpen() 函数：

{% highlight ahk linenos %}
strFileName := "C:\Test.txt"
objFile := FileOpen(strFileName, "r", "UTF-8")
if ObjFile
  MsgBox, 文件 %strFileName% 打开成功。
{% endhighlight %}

在创建对象后，必须立即判断是否创建成功，初学者容易忽略这点。

### 操作文件

这些操作是文件句柄的方法，所以必须首先通过 FileOpen() 获取文件句柄才能调用这些方法：

> Read：从文件当前指针位置读取字符串并使文件指针向前移动。  
> Write：写入字符串到文件的当前指针位置并使文件指针向前移动。  
> ReadLine：从文件中读取一行文本并使文件指针向前移动。  
> WriteLine：写入字符串后面跟着 \`n 或 \`r\`n，取决于打开文件时使用的标志。使文件指针向前移动。  
> RawRead：从文件读取原始的二进制数据到内存。  
> RawWrite：写入原始的二进制数据到文件。  
> AtEOF：判断文件指针是否到达文件末尾。  
> Close：关闭文件，把缓冲区的数据写入磁盘并释放共享锁定。   

上面是文件对象常用的方法和属性（其中只有 AtEOF 是属性），其他方法和属性请参阅帮助。

### 文件对象用法模板

{% highlight ahk linenos %}
FileName := "d:\test.txt"
ObjFile := FileOpen(FileName, "r")
if !IsObject(ObjFile) ; 另一种判断文件对象是否创建成功的方法。
{
  MsgBox, 文件 %FileName% 打开失败。
  return
}

Loop ; 此处可能 While 循环较好，后续文章中我会比较几种循环的差异。
{
  If ObjFile.AtEOF ; 判断是否到了文件末尾
    break
  Text := ObjFile.ReadLine()
  MsgBox, 第 %A_Index% 行文本为: %Text%
}
ObjFile.Close() ; 结束操作后必须关闭对象，以释放资源。
{% endhighlight %}

这个文件对象的使用模板，就像武术中的套路，在需要时适当调整一下很方便用到自己的脚本中。

## 文件命令与对象的比较

命令简单就不解释了，不过上面的内容也许还不容易看出文件对象的强大，所以这里做个小结：文件对象的强大在于它的灵活，尝试使用它的几个方法会对此有所理解，尤其在处理二进制内容的情况时（这方面命令的功能极其有限）；文件对象的高效则在于对同一文件进行大量的读取、写入操作时，我们都知道 I/O 操作是比较耗时的，在大批量文件处理时十分明显。

对于初学者，从命令开始吧，基本都能满足需求（需要文件对象的强大之处实际使用相对较少），熟悉后可以适当了解文件对象。到最后，会选择使用哪种风格往往成了个人习惯，而与其他关系不大。
