---
date: 2014-08-12 10:26:23+08:00
layout: post
title: 粘贴网页内容时附上来源
thread: 
categories: 即用
tags: 复制
---

许多朋友经常摘录一些网页内容到其他地方，供查阅、编辑等，在这时，常常要复制两次，一次是内容，接着一次是内容所在的网址。脚本比较简单，只有一个热键，当我们粘贴从网页中复制的内容时，它会自动附加上网页的地址。

## 脚本

最初我写了这种功能的脚本，但一些方面处理不太好，下面这个脚本是 Lexikos 重写的，比较完善，不影响其他复制粘贴操作。

原理是，从网页复制内容时其中的内容实际上包含了来源，所以直接从中提取。

{% highlight ahk %}
~^v:: 
; 最初灵感：http://ahk8.com/thread-4198.html
; 脚本来源（英文）：http://www.autohotkey.com/board/topic/82393-auto-attach-its-url-when-copy-from-a-webpage/#entry525258
Sleep 100
CF_HTML := DllCall("RegisterClipboardFormat", "str", "HTML Format")
bin := ClipboardAll
n := 0
while format := NumGet(bin, n, "uint")
{
    size := NumGet(bin, n + 4, "uint")
    if (format = CF_HTML)
    {
        html := StrGet(&bin + n + 8, size, "UTF-8")
        RegExMatch(html, "(*ANYCRLF)SourceURL:\K.*", sourceURL)
        break
    }
    n += 8 + size
}
if !sourceURL
    return
Clipboard := "`nSource: " sourceURL
Send ^v
Sleep 250
Clipboard := bin
return  
{% endhighlight %}

使用时开启脚本后与平常一样复制， 然后使用 Ctrl + V 粘贴就行（鼠标粘贴无效）。

## 实际效果

我复制[【其他】Copyheart、改版](http://zhuanlan.zhihu.com/autohotkey/19751034)中的部分内容，如下：
![复制网页内容]({{ site.url }}/assets/images/20140812000.png)

粘贴到 Word 中后（因内容过宽，右边部分被截除）
![粘贴到 Word 中]({{ site.url }}/assets/images/20140812001.png)

可以看到在原内容后自动增加了文章的网址，以后复制网页内容（包括从浏览器、CHM 文件等复制的情况）时开启这个脚本就方便多了。

## 小结

可根据需要调整脚本，上面的脚本中没有注释，如果有兴趣进一步了解原理，请参阅：

* CF_HTML 剪贴板格式的数据结构：[HTML Clipboard Format](http://msdn.microsoft.com/en-us/library/aa767917)
* 最初的实现思路及改进过程： 上面脚本中的来源链接
