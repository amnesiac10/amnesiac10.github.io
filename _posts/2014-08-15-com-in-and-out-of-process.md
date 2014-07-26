---
date: 2014-08-15 06:10:53+08:00
layout: post
title: COM 对象的进程内、外运行
categories: 甜点
tags: COM IE
---

正餐外来甜点，点缀生活。总结性的文章尽管有内容，但一般比较长、看起来也累，而实用脚本可能某些读者用不上（可能是暂时），虽然我写的时候告诉你所以然，所以本文既非总结性内容，也非实用脚本，可无需打开编辑器运行实践，但还是能了解些东西的。

下面我们编写一个脚本，让它创建一个 Internet Explorer 实例，并在屏幕上显示，在暂停 10 秒钟后退出：

{% highlight ahk %}
objIE := ComObjCreate("InternetExplorer.Application")
objIE.Visible := True
Sleep, 10000
MsgBox, 脚本执行完毕。
{% endhighlight %}

该脚本将创建一个 Internet Explorer 实例并显示在屏幕中。 在经过 10 秒钟的暂停之后，会出现一条消息，提示您脚本已执行完成。 单击“确定”后，脚本将立即终止。  
您可能已经注意到，脚本终止后 Internet Explorer 仍在运行，也就是说，脚本终止后 Internet Explorer 并未终止。这是什么原因呢？是这样，有些 COM 对象（比如，FileSystemObject）与脚本在同一个进程中运行。也就是说，脚本所在进程终止后，在该进程中运行的 COM 对象也将终止运行（这就是**进程内运行**的含义）。脚本进程终止后，FileSystemObject 也将终止。  
您不相信吗？下面我将为您证实。我们编写一段脚本，在脚本中创建 FileSystemObject 的一个实例，打开任务管理器后运行该脚本。此时您会在任务管理器中观察到只创建了一个新进程，这是因为脚本和 FileSystemObject 在同一个进程中运行。

{% highlight ahk %}
objFSO := ComObjCreate("Scripting.FileSystemObject")
objFolder := objFSO.GetFolder("C:\")
Sleep, 10000
MsgBox, 脚本已运行完毕。
{% endhighlight %}

现在打开任务管理器后再次运行前面的 Internet Explorer 脚本，这时应该能够看到新增了两个进程：AutoHotkey.exe 和 iexplore.exe。这是因为 Internet Explorer 在自己的进程中运行。脚本运行结束后，脚本进程（AutoHotkey.exe）将消失，但 Internet Explorer 进程（iexplore.exe）将继续存在。

这很重要，如果不在 Internet Explorer 对象中执行退出操作，它将持续运行并继续占用内存。因为终止脚本的运行不会自动终止 Internet Explorer 程序。

看样子要束手无策了，是吗？别失望。您要终止一个 Internet Explorer 实例吗？只需要确保在脚本中的某处执行 Quit 命令就可以终止此实例。例如，下面的脚本创建将创建一个 Internet Explorer 实例，暂停 10 秒钟后，使用 Quit 命令关闭它，再暂停 10秒钟后，自动终止脚本。如果在打开任务管理的情况下运行此脚本，您将会看到系统创建了两个新进程，即 AutoHotkey.exe 和 iexplore.exe，经过短时间的暂停之后，将会看到 iexplore.exe 和脚本进程先后消失。

{% highlight ahk %}
objIE := ComObjCreate("InternetExplorer.Application")
objIE.Visible := True
Sleep, 10000
objIE.Quit
Sleep, 10000
{% endhighlight %}

提示：有时您会发现脚本编写者将对象引用设置为空，就象下面这样：

{% highlight ahk %}
objIE := ComObjCreate("InternetExplorer.Application")
objIE.Visible := True
Sleep, 10000
objIE := ""
{% endhighlight %}

该语句用来释放对象引用（即 objIE 将不再指向 Internet Explorer 的实例），但它不会终止 Internet Explorer 的运行，实际上，iexplore.exe 将继续运行，就好像任何事情都没有发生，因为确实没有发生任何事情。如果希望关闭 Internet Explorer（这里指在脚本中），就必须使用 Quit 方法。

注：上面的脚本可正常执行于 Windows XP，我不清楚 FileSystemObject 对象在 Windows 7/8 系统中是否存在（请帮忙确认）。

甜点完了，还可口吗？
