---
date: 2014-07-19 06:10:53+08:00
layout: post
title: AutoHotkey之中国情
thread: 2
categories: 其他
tags:
---
注：本文中的链接因年代久远可能失效，若需要查看有关内容可根据关键词搜索或到网络缓存、存档中查找，也可联系我。

导言：这里我以个人视角简述 AutoHotkey 在国内发展的大致历程，由于早期许多事件我没有亲身经历，疏漏在所难免，欢迎留言补充。简录于此，仅做纪念。

如未特别说明，以下提到的人皆以其微博或论坛 ID 表示。

## 国内发展史

国内用户接触到 AutoHotkey 最早大概在 2005 年（早期开发历史可参阅维基百科 [AutoHotkey 条目](http://zh.wikipedia.org/wiki/AutoHotkey)），当时网上能找到零星的中文资料。现在记忆犹新的是 yonken 的博客（记忆中最早在 blogcn.com，之后辗转多处），里面发了许多译自帮助的内容，也写了不少即用型脚本。此后，还写了[自动化操作轻松入门系列](http://ahk8.com/thread-3111.html?highlight=%E8%87%AA%E5%8A%A8%E5%8C%96+and+%E8%BD%BB%E6%9D%BE%E5%85%A5%E9%97%A8)（原网址已不可考）并把已翻译的内容打包成中文帮助（即 1.0.36.06 版本，现在仍可从 [AutoHotkey 中文化项目](http://sourceforge.net/projects/ahkcn/)下载我收藏的版本）。若说国内推广，他当推第一人。可惜，他的博客历经变迁，多数文章现在都看不到了。

2006 年末，xbeta 写了[《AutoHotkey 0级入门教程:让重复工作一键完成》](http://blog.sina.com.cn/s/blog_46dac66f010005g7.html)，被转载到太平洋电脑网（此时更名为[《演绎段氏"凌波微步" AutoHotkey 0级入门教程》](http://arch.pconline.com.cn//pcedu/soft/gj/others/0609/872613.html)）。此后几年在其新浪博客中写了许多实际应用文章开始着力推广 AutoHotkey（请参阅[搜索“AutoHotkey”_善用佳软](http://blog.sina.com.cn/search/search.php?uid=1188742767&keyword=autohotkey)）及在其独立博客中的更多文章（参阅：http://xbeta.info/tag/autohotkey）。
![2014-07-02 095828.png](http://upload-images.jianshu.io/upload_images/19661-32cbe8565ce36577.png)
从上图中可以看到，AutoHotkey 在国内的兴起大致在 2007 年末。也是在这时，sfufoet 在煎蛋开始了 [AHK 快餐店系列](http://jandan.net/2007/10/21/ahk-fast-food-restaurant-advance-notice.html)教程，之后转到小众软件[继续更新](http://www.appinn.com/ahk-fast-food-restaurant/)，并在 2008 年末打包了连载的 AHK 快餐店教程（含所有脚本）及中文帮助而出了个 [AHK 懒人包](http://www.appinn.com/autohotkey-all-in-one/)，极大方便了 AutoHotkey 初学者上手（该教程至今我仍推荐初学者使用）。

2008 年 2 月，helfee [创建了中文论坛](http://ahk.5d6d.com/thread-941-1-1.html)（ahk.5d6d.com，目前打开将被重定向），他与 BLooM2、天堂之门等论坛管理在初期做了大量建设性工作，至此中文爱好者终于有一个集中讨论交流 AutoHotkey 的地方了。4 月，论坛[开始组织翻译](http://ahk.5d6d.com/thread-4-1-1.html)大家需求最迫切的中文帮助（在 [AutoHotkey 中文化项目](http://ahkcn.sf.net)中可以下载论坛翻译的最终版）。

2008 年 7 月，xbeta 联系在深度论坛建立了 AutoHotkey 专版（大致在 2010 年关闭），之后在博客中发起[为 AutoHotkey 做点事](http://xbeta.info/thanks-autohotkey.htm)活动，得到众多响应。

受到上面这些众多的事情的推动，一些用户逐渐注意到 AutoHotkey，因此在 2007 年末到 2009 年初，搜索呈现持续上扬的趋势，这可以部分看出当时用户的活跃。不过，在 2009 年中后期逐渐进入了平稳下滑时期。此时 helfee 等中文论坛管理因毕业等原因不再活跃于论坛，缺乏管理让气氛转为冷清，同时许多个人博客中对 AutoHotkey 的介绍与应用也进入沉寂。

2009 年 9 月，AutoHotkey 到了 1.0.48.05 版本（当前所称的经典版），但更新此时进入停滞状态。 在[选择哪个分支？](http://amnesiac10.github.io/2014/08/02/choose-versions.html)可以看到当时的分支百花齐放，主因是许多迫切需求在主分支尚未实现，如 Unicode 支持（这个对非英语用户尤甚，还有记得热字串中曾不能直接发送中文的老用户吗？）。

这是个孤独的时期，论坛管理缺失导致广告充斥、问题无人回复，翻译工作也中止，几无气氛可言。

幸运的是从 2008 年 7 月开始，AutoHotkey_L（当时为旁支，目前成为主分支）的开发[进入活跃期](http://ahkcn.sourceforge.net/docs/AHKL_ChangeLog.htm)（开源的福利），之后合并 AutoHotkey64、AutoHotkeyU（添加 64 位和 Unicode 支持）并增加 COM、对象等支持。在 2010 年 10 月，Chris Mallett（AutoHotkey Basic 作者）表示[不再开发并宣布 AutoHotkey_L 为其后续分支](http://www.autohotkey.com/board/topic/58864-my-status-and-website-changes/#entry369976)。

在 2010 年秋，amnesiac 来到论坛，次年参与管理（清理广告、回复问题等），并开始发表使用心得与经验。2011 年 2 月，在善用佳软上发表 [AutoHotkey 学习指南](http://xbeta.info/autohotkey-guide-2.htm)。下半年开始独力在论坛翻译版基础上继续翻译中文帮助，于 2011 年 1 月完成（1.0.90.00 版本），之后开始了同步更新（之前翻译时并未更新已过时内容）和维护工作。

2012 年中论坛转为主要由 nepter 负责，在次年中转为 morler，6 月，在 helfee、天堂之门等老管理协助下将论坛[迁移到 ahk8.com](http://ahk8.com/Announcement-%E6%96%B0%E8%AE%BA%E5%9D%9B%E5%90%AF%E7%94%A8%E4%B8%AD%EF%BC%8C%E6%AC%A2%E8%BF%8E%E5%8F%8D%E9%A6%88)，不久原免费论坛 5d6d 关闭。 2013 年 AutoHotkey 在国内又迎来了兴盛期（上图中相当明显）。2014 年初，热心网友 robertL 参与管理至今。

到这里算告一段落了。

注：上文中所述的几个基础入门教程写的很好，为方便初次接触脚本的朋友，我已将它们转载到本专栏并根据现状进行了更新：

* [让重复工作一键完成](http://amnesiac10.github.io/2014/07/31/one-key-for-repeat-tasks.html)
* [自动化操作轻松入门](http://amnesiac10.github.io/2014/07/30/learn-to-do-automatically.html)
* [AHK 快餐店系列索引](http://amnesiac10.github.io/2014/07/25/ahk-fast-food-restaurant.html)

## 翻译人员列表

从中文论坛组织帮助的翻译开始，经历了坎坷曲折，并一度停滞至中文帮助完成，期间离不开许多人的默默支持和付出，这里列出参与帮助翻译的人员名单（按字母顺序，这里的 ID 提取自论坛翻译版中各人翻译的页面，可能有所遗漏，且由于论坛变迁，有些用户可能不存在于目前中文论坛了）：

* amnesiac
* baggio1987
* BLooM.2
* bu45gande
* handt
* helfee
* hong
* hsudatalks
* Kookee
* lastmore
* lwjiee
* okey3m
* tcgbp
* wanggang999
* wz520
* Xiangee
* xiaohui
* yugi
* 单菜子
* 第八单元
* 甲壳虫
* 惊幻
* 天堂之门
* 游否

特别感谢：

* yonken，翻译了早期部分帮助。 

