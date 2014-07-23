---
date: 2014-08-03 06:10:54+08:00
layout: post
title: 选择哪个分支？
thread: 17
categories: 指南
tags: build 版本 历史
---
对于绝大多数用户这个问题的答案都应该是 AutoHotkey_L，那么这个问题还是问题吗？对于新人我还是有必要简要介绍它们（这是他们的主要困惑之一），以便对这些名称及它们的含义有个清晰的认识，同时明白自己选择的原因（顺便也了解些典故）。
![AutoHotkey 各分支演进图]({{ site.url }}/assets/images/20140803000.png)
图 AutoHotkey 的历史和当前分支一览（[来源](http://maul-esel.github.io/ahkbook/en/images/versions.png)）

## AutoHotkey_L（首选）
[AutoHotkey_L](http://ahkscript.org/download/) 指主要由 Lexikos 在原 AutoHotkey Basic（见下文）源码基础上开发的分支，具体包括从1.0.48.05 L4 版本号 （更新记录为 Revision 4，发布于 2008-07-18）至今的所有版本，更新记录请参阅 [AutoHotkey_L 更新记录](http://ahkscript.org/docs/AHKL_ChangeLog.htm)。有时会被笼统的称为“AutoHotkey 1.1“（目前在下载页面即是如此）。

### 功能的增强

它在 AutoHotkey Basic 基础上增加或增强的主要功能（详细说明请参阅 [AutoHotkey_L 新特性](http://ahkcn.github.io/docs/AHKL_Features.htm)）：

* 提供 Unicode、COM 和 64 位原生支持
* 支持各种文本编码
* 对象（可扩展的联合数组）
* 交互式调试支持
* 增强的错误处理
* 面向对象文件 I/O

功能的增强是我们选择的重要原因之一，在如今的 Windows 主流系统中不支持 Unicode 的脚本用处还有多大呢？尤其对于中文用户。

### 主流的支持 

功能的增强是其中一个方面，主流的支持则是另一个重要原因。与其他比较，AutoHotkey_L 是目前 AutoHotkey 社区用户使用的主流分支：

* 目前尚在更新，包括修复缺陷和完善功能
* 社区中的函数和脚本几乎都适用该分支
* 遇到问题时能方便从社区或网络获得支持

所以，**如果您在犹豫，那么选择 AutoHotkey_L 吧**。

### 构建的选择

该分支中因编码和平台类型分成三种构建（build）：

> Unicode 32-bit - recommended for new scripts.  
>   Unicode 64-bit - for increased performance on 64-bit systems.  
>   ANSI 32-bit - better compatibility with some older scripts.   

如下载页面的说明所示，大多数用户请选择 Unicode 32-bit，但这个链接的目标文件仅有主程序（即一个 AutoHotkey.exe，适合偏爱绿色版的老用户）。**建议直接下载页面开始处的安装包**（Installer），其中包含了这三种构建和后面的编译器和离线帮助（英文的，中文帮助请在底部链接处下载）**并在安装时选择 Unicode 32-bit** 即可。

Unicode 64-bit 仅能运行于 64 位系统，至于比较此时的 32 位构建其性能有增强多少还没看到有比较数据。
ANSI 32-bit 的兼容性主要指运行为 AutoHotkey Basic 编写的脚本而言，新用户无需考虑。

注：若未特别说明，以后本专栏中的代码的测试版本均为 AutoHotkey_L Unicode 32-bit。一般而言，AutoHotkey_L 及其他基于 AutoHotkey_L 分支的版本（如 AutoHotkey_H）应该能正常运行这些代码。

## AutoHotkey v2（不推荐）

[AutoHotkey v2](http://ahkscript.org/v2/) 由 Lexikos 根据 Chris Mallett（AutoHotkey Basic 作者）对 AutoHotkey 未来的计划基于 AutoHotkey_L 代码开发，目前仍在测试过程中，AutoHotkey v2 只有 Unicode 构建（含 32 和 64 位）。目前在开发过程中的许多新特性都会合并到 AutoHotkey_L 中。

开发状态：它正在调整语法（带来不兼容的许多变化）和功能，包含了许多细节改进。由于许多细节仍在调整中、功能和语法尚未定型，文档也严重过时，且使用这个版本后您可能需要在每个新版发布时修改自己的代码，同时也无法直接执行论坛上大量的脚本。

**对于普通用户及常规用途：目前该分支语法和功能尚未定型**，较 v1.1 的更新情况也未全部写入日志，且相应的**文档过时，所以不推荐日常使用**。

对于其他用户：该分支将很可能是 AutoHotkey 第三代（尽管版本为 v2），虽然目前含有一些缺陷，不过已经可以使用，所以欢迎有经验的老用户和开发者下载测试。关于这个分支的语法、功能等的讨论正在官网热烈进行，如果希望了解目前状态或反馈相关的建议、意见（作者也活跃其中）：

* v1.1 至 v2.0 变更记录【[英文](http://ahkscript.org/v2/v2-changes.htm)、[中文](http://ahk8.com/thread-4926-post-30223.html#pid30223)（内容较旧，robertlzj 翻译）】
* 【[AutoHotkey v2 讨论版块](http://ahkscript.org/boards/viewforum.php?f=37)】

使用 AutoHotkey（Basic 至 _L）那么长时间以来，您不是一直在抱怨这个吐嘈那个吗，还等什么呢？与作者直接交流吧，也许正式版出来时其中某个功能就是您的提议呢。

## AutoHotkey_H（不推荐）

[AutoHotkey_H](http://www.autohotkey.net/%7EHotKeyIt/AutoHotkey/) 是由 HotkeyIt 合并了原有 AutoHotkey.dll（介绍见下文）并在AutoHotkey_L（及 AutoHotkey v2）基础上开发的增强分支。它没有使用自己的版本号，一般与 AutoHotkey_L（及 AutoHotkey v2）并行开发，最近更新时间为 2013-08-11。

一般提到 AutoHotkey_H 时，实际上包含了 AutoHotkey.dll 和 AutoHotkey.exe (H 版本) 及相关文件。其中：

* AutoHotkey.dll 最初由 tinku99 开发，已由 HotkeyIt 合并至 AutoHotkey_H，之后新增了简化版本（AutoHotkeyMini.dll）。其他语言通过 DLL 接口或 COM 接口利用该文件可执行 AutoHotkey 代码，而 AutoHotkey_L 也可通过它实现多线程。
* AutoHotkey.exe（H 分支）是 HotkeyIt 在 AutoHotkey_L（及 AutoHotkey v2）基础上主要增加线程和结构相关函数并增强了 DLL 调用功能的分支，详细的新增功能及细节变化请参阅其帮助。
>* AutoHotkey v1（基于 AutoHotkey_L）可执行文件包含了 ANSI 和 Unicode 32 位版本及 Unicode 64 位版本。  
>* AutoHotkey V2（基于 AutoHotkey v2）可执行文件包含了 Unicode 32 和 64 位版本。  

它实现了多线程、支持动态运行 AutoHotkey 代码、在 #Includes 中使用通配符或动态 #Includes、简化了 DLL 尤其是 Windows API 的调用，适用于已经使用 AutoHotkey 较长时间的有经验用 户。其中的帮助仅说明了在 AutoHotkey_L 外有修改或增强部分的内容，所以需要与 AutoHotkey_L 帮助一起使用。 **AutoHotkey_H 功能上有所增强，不过用户群较小（可能测试不充分）、不易获得支持，同时帮助文件比较粗糙（尽管有译成中文的版本），更新较不稳定。因此，新用户无需考虑。**

这个分支的水很深，不过如果您有一定编程经验，我觉得可以试试。

## 不活跃分支

* IronAHK【 [论坛主题页](http://www.autohotkey.com/forum/topic54494.html)、[开发项目@ GitHub](https://github.com/polyethene/IronAHK)】

用于 Windows/Linux/Macintosh 的 .NET/Mono 分支。

这个分支由 polyethene 和其他贡献者使用 C# 为 .NET 和 Mono 而完全重写以实现 AutoHotkey 的跨平台的分支。使用它您能把脚本编译为平常的 .NET 编译语言，因此需要 .NET 框架或 Mono 才能安装。IronAHK 目前尚处于 Alpha 测试阶段，并且不幸的是，目前开发似乎暂停较长时间了。还有许多事情需要做。
尽管这是个很有前景的项目，不过当前不建议初学者安装使用。

* AutoHotkey Mobile【[论坛主题页](http://www.autohotkey.com/forum/topic27146.html)】

用于 Pocket PC、WinCE 和 Smartphone 的 AutoHotkey 分支。

* AHKLinux【[论坛主题页](http://www.autohotkey.com/forum/topic50534.html)】

用于 Linux/Wine 的版本。

## 其他历史分支

对于出现过的其他历史版本，下面尽可能用一句话简单介绍以供了解，不建议使用。

* AutoHotkey Basic（经典版）

AutoHotkey Basic 包括从首个测试版本至 1.0.48.05（更新于 2009-09-25）的所有版本，更新记录请参阅 [AutoHotkey Basic 更新记录](http://ahkscript.org/docs/ChangeLogHelp.htm)。主要由 Chris Mallett 开发，在 2009 年更新到 1.0.48.05 版本后停止更新。作者在 2010 年宣布 AutoHotkey_L 为它的后续分支。

关于名称：作者 Chris Mallett 称之为“AutoHotkey Basic”，多数社区用户也使用”Classic“，论坛上有些时候会表示为“Vanilla”（我感觉这个是代号），中文用户多接受“经典版”（符合其内涵）。与 AutoHotkey_L 被称为“AutoHotkey 1.1”相对应，有时该分支也笼统的使用“AutoHotkey 1.0”代称。
注：由于这个分支作者已停止开发、论坛提问也不容易获得支持，同时不包含 Unicode、64位系统及其他重要的特性，目前仅有少数老用户及一些旧的脚本在使用。

* AutoHotkey.dll

AutoHotkey.dll 是 AutoHotkey 的动态链接库版本，已合并至 AutoHotkey_H。

AutoHotkey.dll 它允许被 AutoHotkey_L（及基于该分支的版本）加载多次以实现多线程和使用它的导出函数和内置功能；同时可向其他编程和脚本语言嵌入了 AutoHotkey 解释器而打开了 AutoHotkey 的世界。通过它可以在其他许多语言中使用 AutoHotkey 的功能，如 C#、C++、VB、Python、Javascript 等，只要它们能能加载 DLL 或使用 COM 接口。如果您可以在 Excel 或 Word 宏中执行 AutoHotkey 代码，这是不是很酷？这是它的内置功能.

* AutoHotkeyU

AutoHotkeyU 是由 jackieku 开发的 AutoHotkey_L 的 Unicode 版本，已合并回 AutoHotkey_L（Revision 41）。

* AutoHotkey64

AutoHotkey64 是由 fincs 开发的 AutoHotkey_L 的 64 位版本（并增加 COM 支持），已合并回 AutoHotkey_L（Revision 53）。
## 小结

看到这里您是不是后悔了，长长的篇幅看下来，我就没有选择啊？哈哈，您也不会再为版本困惑了嘛。

参考： [What AutoHotkey version should I choose?](http://maul-esel.github.io/ahkbook/en/What-Version-To-Choose.html)
