---
date: 2014-07-24 06:10:53+08:00
layout: post
title: 宏录制和自动化脚本创建
thread: 7
categories: 工具
tags:
---
## 为什么需要
我曾在[哪些事情是 AutoIt 可以而 AutoHotkey 不行的？](http://www.zhihu.com/question/20224354/answer/20773391)问题中提到 AutoHotkey 学习方法的几点建议，其中一点是：

> 对于一般的初学者，从录制/回放操作开始慢慢接触认识脚本是个学习脚本的重要途径。  

一句话，通过录制/回放工具可极大降低学习难度，适合零基础用户。

## 曾经的记忆

两三年前的用户可能还记得 AutoHotkey Basic（关于各分支介绍请参阅[选择哪个分支？](http://amnesiac10.github.io/2014/08/02/choose-versions.html)）中自带有录制鼠标和键盘宏的工具——AutoScriptWriter：
![](http://ww2.sinaimg.cn/mw690/6ef7171bgw1eh3gephcbtj20f50av400.jpg)
从图中可以看出界面非常简单，只有这么几个按钮，功能也十分有限。目前 AutoHotkey_L 已经去除了，可能因缺乏更新而与目前的 AutoHotkey_L 存在兼容问题。长期以来我一直关注论坛上其他录制工具，不过看过的一些都感觉有所欠缺，如在易用性方面仍不够理想或可操作性较差。

## 推荐工具
本文中我将推荐一款适用于 AutoHotkey 目前版本的录制器，即 [Pulover's Macro Creator](http://www.macrocreator.com/)，这是一个成熟且功能强大的**宏录制和自动化脚本生成工具**，之前我曾[在论坛中简单介绍过](http://ahkscript.org/boards/viewtopic.php?f=28&t=1175)。该工具在这两方面都超出我的期待，完全是为普通电脑用户（这里主要指零脚本/编程知识）所准备的，不仅如此，它还具有强大的功能，控制窗口、控件、操作文件、字符串、搜索图像等从简单的重复任务到复杂的操作都不在话下：

* 对于普通用户，只需点点鼠标或使用快捷键就能录制/回放重复的操作，几乎零学习成本，您所需要做的就是大胆的尝试；
* 对于了解脚本/编程的用户，简化脚本编写，使用高级功能如使用 COM、消息等，以及把代码用在自己的工具中；

该工具已成为 [SciTE4AutoHotkey](http://fincs.ahk4.net/scite4ahk/) 自带的辅助工具之一，且受到许多下载站点的大力推荐，包括下面两个知名软件站点：
[![](http://s1.softpedia-static.com/base_img/softpedia_free_award_f.gif)](http://www.softpedia.com/progClean/Pulover-s-Macro-Creator-Clean-228684.html)  [![](http://img.informer.com/awards/software_awards_no_viruses.gif)](http://pulover-s-macro-creator.software.informer.com/)

## 介绍
以下内容编译自[其官方网站](http://www.macrocreator.com/)：
### 简单说明
PMC 不仅仅是宏录制工具！
Pulover’s Macro Creator 是免费的自动化工具和脚本生成器，它基于 AutoHotkey 语言，为用户提供了多样的自动化功能及内置的录制工具！
您不仅可以添加键盘和鼠标操作到脚本中，还可以操作窗口、控件、文件、字符串、搜索图像甚至使用 If/Else 来控制宏的执行流程。从简单的重复任务到复杂的自动化项目，而这一切都可以在友好且直观的界面中完成。Pulover’s Macro Creator 能节省大量重复劳动的时间，减少单调操作造成的肢体重复性劳损。
如果您不太了解编程，那么通过它您可以创建宏来自动化各种重复的操作或导出为有效的 AHK 脚本。无需了解每个命令，仅仅通过录制、回放和导出按钮您就可以录制、测试和创建能运行的热键宏。它的默认设置适合大多数用户。
如果您熟悉 AHK 或编程，那么它可以节省您创建和编辑宏的时间。您可以导出脚本、在一个脚本中放入多个热键宏，可以方便从预览窗口把生成的代码复制到您的项目中。它还提供像变量赋值、函数和 COM 接口这样的高级功能。

### 功能列表
* 可自定义的界面布局
* 命令搜索
* 屏幕命令提示
* IE 自动化（COM）
* 通用 COM 接口
* 支持多级宏
* 录制键盘、鼠标和窗口
* 录制鼠标的相对点击/拖动
* 精确录制鼠标移动
* 回放速度控制
* 预览最终脚本（包括在录制期间）
* 保存/加载宏或根据自定义选项和热键将其导出为 AHK 脚本

更多功能请参阅帮助文档，[MacroCreator学习资源](http://amnesiac10.github.io/2014/07/22/resource-for-macrocreator.html)将介绍 Pulover's Macro Creator 学习资源。
