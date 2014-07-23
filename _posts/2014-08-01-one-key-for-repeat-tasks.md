---
date: 2014-08-01 06:10:54+08:00
layout: post
title: 让重复工作一键完成
thread: 15
categories: 指南
tags: 教程
---
转者按：本文属于**零基础入门**专题教程，原[发表于新浪博客](http://blog.sina.com.cn/s/blog_46dac66f01011ar7.html)，后[被转载自太平洋电脑网](http://arch.pconline.com.cn//pcedu/soft/gj/others/0609/872613.html)，目前[更新于善用佳软](http://xbeta.info/autohotkey-guide.htm)，作者 xbeta。（另：我曾[转载到中文论坛](http://ahk8.com/thread-3113-post-17838.html)。）本专题选取一些通俗易懂的基础入门教程，经适当整理（以反映目前 AutoHotkey 现状）后集中发表，以方便初次接触脚本的朋友入门（帮助中的[初学者向导](http://ahkcn.github.io/docs/Tutorial.htm)也是很好的入门教程）。

![AutoHotkey_logo]({{ site.url }}/assets/images/AutoHotkey_logo_no_text.gif)
AutoHotkey 是一个神奇的工具。为了便于新人上手，xbeta 写了此篇最最傻瓜的 0 级入门教程。

## 何为 AutoHotkey

AutoHotkey 是一个小工具软件，可以简化你的重复性工作。

比如要登录某论坛，你只要按一个键，AutoHotkey 就会替你：打开浏览器、输入网址、输入用户名和密码、回车，完成登录过程。

只要有想像力，AutoHotkey 可以完成更多工作，参见[更多文章](http://xbeta.info/tag/autohotkey)。

## 下载及安装
注：安装程序可能不定时更新，所以后面的描述和截图可能与最新版本有差异，不过安装界面的英文内容都简单易懂，所以无需担心安装错误带来问题。

软件名称： AutoHotkey  
软件版本： 1.1.15.01  
软件大小： 2+MB  
软件授权： 开源、免费  
适用平台： Windows 2000 - Win8  
下载地址：[点击这里下载](http://ahkscript.org/download/ahk-install.exe)  
中文帮助：[点击前往](http://ahkcn.github.io/docs/)

安装：按提示操作即可。我习惯上装在 d:\program files\AutoHotkey

安装界面中，可选择典型安装、自定义安装或绿色提取：
![AutoHotkey 安装界面1]({{ site.url }}/assets/images/20140801000.png)

* 典型安装（Express Installation）：将安装默认版本到默认位置中，没有特殊要求时建议选择
* 自定义安装（Custom Installation）：将提供更多选项供用户选择
* 绿色提取（extract to ...）：将提取程序文件到指定目录，不会写入注册表（以后将无法双击执行 ahk 脚本）

点击了自定义安装后，后续对话框会提供更多选项：

1. 安装构建（[点击了解它们的区别]({{ site.url }}/2014/08/02/choose-versions.html)），推荐 Unicode 32-bit
2. 目标目录、是否创建开始菜单快捷方式
3. 其他附加选项（安装脚本编译器、启用脚本的拖放功能），建议选上

点击 `Install` 后才真正开始执行安装过程，完成后提示：
![AutoHotkey 安装界面2]({{ site.url }}/assets/images/20140801001.png)

AutoHotkey 的帮助文件，写得很细。有耐心的就认真拜读，想成高手的必须要研读。

## 应用例 1：提示与访问网页

### 创建脚本文件

如下图，打开你的文本编辑器（记事本、或 gVIM），新建一个文件，把下两行内容复制进去。

{% highlight ahk %}
msgbox, 这是我的第一个 AutoHotkey 脚本 \`n 我既关注效率，也尊重版权
run, http://xbeta.info/autohotkey-guide.htm
{% endhighlight %}

先任意保存到一个地方（比如桌面），文件名任意（比如 new.ahk）
![保存脚本]({{ site.url }}/assets/images/20140801002.png)
注意：①文件名后辍必须为ahk；②保存格式必须选为 UTF-8 with BOM！（记事本保存为 UTF-8 时会加上 BOM，对于其他编辑器请参阅相关说明。）
![使用 UTF-8 编码]({{ site.url }}/assets/images/20140801003.png)

### 运行脚本文件

这时，双击 new.ahk 看到效果了：
先弹出如下提醒
![显示消息框]({{ site.url }}/assets/images/20140801004.png)

你点击“确定”按钮后，就会启动浏览器，打开本文网址（注：此处的本文是指作者博客中的本文地址）。

### 原理解释

所谓脚本，其实就是一个 txt 文件。它由用户编写，由 AutoHotkey 来执行。

第 1 句：msgbox 是一个命令（或称为函数），AutoHotkey 见到它，就知道要弹出一个消息窗口了。后面的文字是参数，在这一命令中，就是弹出消息的文字。其中的 \`n 表示换行。中间用半角逗号分隔。

第 2 句：类似，函数是 run，就是运行。后面的参数就是本文的 url。也就是说，AutoHotkey 的 run 功能，可以运行程序，也可以打开文档（如 d:\freeware-list.txt），也可以打开网址。

## 应用例2：缩写

将下面的语句保存为 new2.ahk （提醒 UTF-8 with BOM 编码）：

{% highlight ahk %}
::test1:: 善用佳软。ひらがな 平仮名；カタカナ 片仮名。Korean/한국어/조선말。
{% endhighlight %}

运行后，在任何能正常显示 unicode 字符的程序中（比如浏览器的地址栏、MS Word），键入 test1 后，再加空格、或 tab、或回车，就可以触发缩写，“善用佳软……”内容就上屏了。

通过这一例子，可以看到 AutoHotkey 实现常用短语（地址、邮箱、密码、网址、签名）的缩写非常方便。

关于缩写功能，还有人用 AutoHotkey 开发过一款专门用于缩写功能的 [Texter](http://xbeta.info/texter-ahk.htm) 呢。

## 自动登录网站

将下面的语句保存为 new3.ahk （提醒 UTF-8 编码）：

{% highlight ahk %}
#1::
run, http://mail.163.com
WinWaitActive, 网易 ; 等待网页加载成功（至少标题显示出来）
sleep, 1000 ; 保险起见，再等1秒（视网速而定）
send, user-id{tab}password{enter} ; 模拟键入用户名、密码、回车
return
{% endhighlight %}

运行脚本……但没有反应？没错，这是因为脚本中为相应命令定义了热键。#1 表示 Win+1 键。  
按下 Win+1 键，脚本会自动打开 163 信箱、输入用户名、密码，完成登录。

注意：本例实际执行中有可能不成功。因为邮箱登录页面可能已经保存了用户名，甚至也保存了密码，导致初始输入焦点不准确。笔者真实在用的例子是登录 Lotus Notes 客户端，并输入密码。代码如下：

{% highlight ahk %}
#n::
run, "c:\Program Files\lotus\notes\nlnotes.exe"
winwait,,输入口令
sendinput, mypassword{enter}
return
{% endhighlight %}

## 后记

作为 0 级入门教程，就写到这里吧。只要大家边读、边动手实践，就不难从这些例子中发现 AutoHotkey 的神奇作用。

如要再进步发掘 AutoHotkey 的魔力，可以：

* 阅读官方帮助文档。
* 参见笔者使用 AutoHotkey 的更多实例：
 * [AutoHotkey｜win run加它更方便](http://blog.sina.com.cn/u/46dac66f010005cf)
 * [AutoIT3 vs AutoHotkey](http://blog.sina.com.cn/u/46dac66f010005cr)
 * [AutoHotkey 调用 Irfanview 把 24 位真彩图片优化到实际色深](http://blog.sina.com.cn/u/46dac66f010005dx)
* 目前最全面的 AutoHotkey 学习资料，由 amnesiac 整理：[AutoHotkey 学习指南](http://xbeta.info/autohotkey-guide-2.htm)

注：xbeta 用 gVIM 编辑 ahk 文件的高亮效果如下：
![在 Vim 中编辑 ahk]({{ site.url }}/assets/images/20140801005.jpg)
