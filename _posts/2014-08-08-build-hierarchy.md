---
date: 2014-08-08 14:16:23+08:00
layout: post
title: 如何构建知识体系
thread: 21
categories: 指南
tags: 
---

导言：很多人可能在生活中或电视上看到这样的情景：一个昆虫向前飞，撞到了蜘蛛网，开始挣扎，每一次的挣扎都让蛛网剧烈晃动，看起来摇摇欲破，但昆虫也让蛛丝越捆越紧，最终成为蜘蛛腹中之物（偶尔也有网破之时）。这整个网就像是知识体系，昆虫的落点则是所遇到问题与我们知识体系的连接处，当知识体系中节点越多，连接越紧密，那么遇到问题被解决的可能性也越大（撞到网上的机率大多了）。曾有人提出[一个 Total Commader 问题](http://www.zhihu.com/question/23816223)，我的回答里使用了消息，后来题主追问，为什么会想到消息呢？希望本文能给有类似疑问的朋友一些启发，这里讲述学习和使用 AutoHotkey 过程中如何构建知识体系中的个人看法，欢迎交流。

## 掌握基础部分

3. 热键、热字串  
这两个是最基础的，基本无需学习、即刻掌握。
3. 基本语法  
脚本的基本知识（如注释）、变量和表达式的用法，这部分也无需专门学习，简单了解即可。
3. 普通的命令、函数  
这里应着重于命令自身的用途、语法和参数等，需要能用于实际问题中。
3. 把同类命令（函数）联系起来  
分类命令，加强彼此之间的区别与联系，如文件操作命令、字符串操作命令等（帮助的目录中已经分类好了），又如 [Send 系列命令](http://ahkcn.github.io/docs/commands/Send.htm)中哪个适用于哪种环境。
3. 流程控制、子程序、函数  
在需要时重用代码，增加编写代码和解决问题的效率。

## 学习扩展内容
3. 指令  
能使用指令实现自己需要的控制
3. 数组、对象  
能理解，并对比文件对象与之前的文件命令、伪数组与数组的异同。
3. 图形界面  
了解 Gui/GuiControl/GuiControlGet 及个子命令用法，会使用 SmartGUI 创建图形界面或自行定制。
3. 正则表达式  
文本处理中，这个工具功能强大，要完全掌握委实不易，不过基础部分通过帮助中的参考在脚本中使用问题应该不大。

## 了解进阶知识
3. Run/RunWait  
不会批处理不要紧，适当了解系统命令行中的命令有些事情能事半功倍，有兴趣也可了解第三方工具，如 [NirCmd](http://nirsoft.net/utils/nircmd.html)（命令行中少见的瑞士军刀）等。注：这两个命令运行图形程序也是一样的，不过这对于系统程序或第三方工具都较简单，应该基础部分就会了。
3. COM 系列函数  
会使用系统或第三方 COM 对象，如 Office 系列组件、大漠插件等。
3. PostMessage/SendMessage/OnMessage()/RegisterCallback()  
消息，会查询系统或第三方工具的消息相关文档并用于脚本中。
3. WMI  
WMI 实际上也是通过 COM 调用的，但它异常强大同时异常复杂，所以这里单独提到。
3. DllCall()/VarSetCapacity()/NumPut()/NumGet()  
了解 Windows API，能构造出所需变量类型并使用，能使用第三方组件。
3. 其他  
AutoHotkey_H、机器码等，单从用处而言这些可能较罕见，不过有助于理解 AutoHotkey 的内部机制，如半线程的概念等。

## 小结
上面这些的学习材料，除了帮助一般都可以在论坛中找到说明或相关指引（对于第三方工具还需查阅相关文档），基本都带有测试代码，自己动手实践过，要掌握问题不大。
说到学习资源，许多用户觉得 AutoHotkey 的资源不多（基本都在论坛）。我是这么看的，基础或扩展部分的内容在帮助或论坛的资源作为学习是足够的，对于进阶部分除了查阅相关文档，很多时候还可以参照其他语言的例子，例如 vbs 调用 COM 的代码可以直接调用或转换过来（以前微软网站上有大量 vbs 代码的教程），而操作网页 js 的代码说少就说不过去了（它们调用 COM、WMI 的语法都大同小异），有相应的功力其他语言的教程也可参考。
学习这些不需按指定顺序，按个人的需求，假设你之前操作过了字符串，最近又遇到字符串问题，对某些命令的用法感到疑惑，那么可以把所有的字符串命令（函数）都看过，对不解的地方写个小脚本测试，这样对它们有个整体的理解和把握，那么以后遇到字符串问题时一般能很快找到最适用的命令。
构建了这个体系后，同一种问题常常有多种解决方法，例如使用命令行命令、COM、WMI 等，怎么选择呢？实际上无需选择，如果效果相同，选最简单、最顺手的就行了。另外还有个指导，一般无需选最强大的、够用就行了，说到这点估计 Windows API 的强大无其他方式可比，不过估计没几个人作为首选。
因此，这个网的结点越多，联系越紧密，那么猎物撞到网上的机率就越大（解决的方法也增多）。对于本文开头所说的问题，如果不用消息，可能还有其他方法，因为这个方法够简单。