---
date: 2014-07-25 06:10:53+08:00
layout: post
title: 获取窗口与控件信息
thread: 8
categories: 工具
tags:
---
常言道，工欲善其事，必先利其器。在脚本中常见的一种操作是操作窗口或控件，在操作之前，首先必须获取目标的各种信息，这时就要用上辅助工具了。本文会介绍一些获取窗口、控件信息的常用工具，这里的先后顺序是随意安排。如果目前用的没什么问题就继续用着，如果对某些地方不满意则可试试其他。
简单的截图不容易全面反映整个工具的功能和特色，使用才能获得真实体验。

## Active Window Info
![Active Window Info 界面截图]({{ site.url }}/assets/images/20140722000.png)

评论：这个最初来自于 AutoIt3 且安装包中自带的工具，就无需过多介绍了。功能简陋，但无需获取且使用还算方便（从托盘或主窗口菜单访问），没有特殊要求的情况下也基本够了。下面介绍的工具一般都包含了这个工具的功能。
这是这里唯一一个在单个截图中包含所有功能且打开之后没有额外操作的工具。

## AHK Window Info
![AHK Window Info 界面截图1]({{ site.url }}/assets/images/20140722001.png)
![AHK Window Info 界面截图2]({{ site.url }}/assets/images/20140722002.png)
[来源](www.autohotkey.com/forum/topic8976.html)：其中包含了功能介绍。
特色：使用 AutoHotkey 编写，可学习源码；获取窗口和控件信息功能全面。

## AHKInfo
![AHKInfo 界面截图]({{ site.url }}/assets/images/20140722003.png)
[来源](http://ahk8.com/thread-4010.html)：包含了更多截图和介绍。
评论：作者星雨朝霞，使用 AutoHotkey 编写，可学习源码；除一般功能外还可获取 IE 浏览器所打开网页的一些信息，方便操作网页。

## WndSpy
![窗体侦探界面截图]({{ site.url }}/assets/images/20140722004.png)
评论：这个工具我曾用过较长时间，不过现在看似乎比较中庸。

## Winspector
![Winspector 界面截图]({{ site.url }}/assets/images/20140722005.png)
评论：最初是在[消息指南](http://ahkcn.github.io/docs/misc/SendMessage.htm)中看到该工具介绍的，我以前使用时大部分情况也是用来监控消息。现在看如果仅仅获取窗口或控件的信息，该工具不方便。
下载时建议下载 WinspectorU，即 Unicode 版本。

## Microsoft Spy++
![Microsoft Spy++ 界面截图]({{ site.url }}/assets/images/20140722006.png)
介绍：此工具提取自 Microsoft Visual Studio 2003，由微软开发。可获取窗口、消息、进程、线程的信息。其监视消息的功能曾在[【即用】amnesiac 的 Everything 热键](http://zhuanlan.zhihu.com/autohotkey/19755148)中简要提及。

## Spy4Win
![Spy4Win 界面截图]({{ site.url }}/assets/images/20140722007.png)
[官网](http://www.ccrun.com/spy4win/)：其中包含了各种功能的详细说明。
 评论：这是 [tmplinshi 在官方中文子论坛中推荐](http://ahkscript.org/boards/viewtopic.php?f=30&t=1497)的工具。它功能上比 MS Spy++ 有所超越，方便性似乎也更好。不过，可能因某些功能所执行的操作被杀毒软件拦截，是否使用请自行判断。

## 小结
* 其中许多工具我只用过部分功能，所以上面的个人评论仅供参考。
* 这些工具多数有点年头了，Windows XP 中运行良好，至于更高系统中表现如何等待各位留言。
* 未提供下载的工具请自行搜索。
