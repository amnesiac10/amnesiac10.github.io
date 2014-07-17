---
date: 2014-07-21 06:10:53+08:00
layout: post
title: 视频网站瞬间提速（适用于优酷、爱奇艺、搜狐、乐视、土豆等）
thread: 4
categories: 即用
tags:
---
注：原本想直接转载过来就放了，但大家热烈响应出乎意料，看来想偷懒也不行啊。

<pre>
<div id="p2pprivacy" class="swfcontent"><embed type="application/x-shockwave-flash" src="http://www.macromedia.com/support/flashplayer/sys/settingsmanager.swf" id="p2pprivacy_swf" name="p2pprivacy_swf" bgcolor="#ffffff" quality="high" scale="noscale" wmode="opaque" flashvars="defaultTab=p2p_privacy" height="270" width="395"></div>

<script type="text/javascript">
   // <![CDATA[
if (top!=self){
    top.location.href=self.location.href;
}
var props = new Object();
props.swf = "http://www.macromedia.com/support/flashplayer/sys/settingsmanager.swf";
props.id = "p2pprivacy_swf";
props.ver = "6";
props.w = "395";
props.h = "270";
props.c = "#ffffff";
props.wmode= "opaque";
	props.scale = "noscale";
var swfo = new SWFObject( props );
swfo.addVariable( "defaultTab", "p2p_privacy" );
registerSWFObject( swfo, "p2pprivacy" );
   // ]]>
  </script>
</pre>

现在看视频真方便，有浏览器就够了。不过在网站上看视频，除了不用安装播放器，其他都和播放器播放无差别吗？

## 为什么那么卡？

很早很早以前，像优酷之类的网站，就在网页播放视频过程中，使用了 P2P 技术。也就是你用网页看视频时，它就会默默的无任何提示的使用你的上传通道，对其它用户进行上传。​此过程不可中断，不可控制，并尽可能占满整个上传通道。

很长一段时间，我完全不知道上述情况，于是常常出现的状况就是 4M 宽带，看个“标清”画质都会卡得像条狗，并且完全不知这是为什么……

很抱歉这里的代码块没有全选或另存为文件的按钮。

## 论坛中[兔子的脚本](http://ahk8.com/thread-5259.html)



```ahk
if not A_IsAdmin
{
  Run *RunAs "%A_ScriptFullPath%"  ; 需要 v1.0.92.01+
  ExitApp
}

MsgBox, 4148, 提示, 本工具将关闭 Flash 的 P2P 功能，以便释放上传通道，最终加速视频网站播放速度！`r`n`r`n适用于优酷、爱奇艺、搜狐、乐视、土豆等几乎所有视频网站。
IfMsgBox, Yes
{
  ; 对 3 个位置的特定配置文件写入特定配置“RTMFPP2PDisable=1”，也就是关闭 Flash 的 RTMFP 协议，即 Flash 的 P2P 功能
  FileRead, OutputVar, %A_WinDir%\system32\Macromed\Flash\mms.cfg
  Loop, Parse, OutputVar, `n, `r
  {
    if (A_LoopField <> "")
      lastline := A_LoopField
  }
  ; 避免重复写入同条配置
  if (InStr(lastline, "RTMFPP2PDisable=1") = 0)
    FileAppend, RTMFPP2PDisable=1`r`n, %A_WinDir%\system32\Macromed\Flash\mms.cfg
  OutputVar := "", lastline := ""

  FileRead, OutputVar, %A_WinDir%\syswow64\Macromed\Flash\mms.cfg
  Loop, Parse, OutputVar, `n, `r
  {
    if (A_LoopField <> "")
      lastline := A_LoopField
  }
  if (InStr(lastline, "RTMFPP2PDisable=1") = 0)
    FileAppend, RTMFPP2PDisable=1`r`n, %A_WinDir%\syswow64\Macromed\Flash\mms.cfg
  OutputVar := "", lastline := ""

  FileRead, OutputVar, %A_WinDir%\system32\mms.cfg
  Loop, Parse, OutputVar, `n, `r
  {
    if (A_LoopField <> "")
      lastline := A_LoopField
  }
  if (InStr(lastline, "RTMFPP2PDisable=1") = 0)
    FileAppend, RTMFPP2PDisable=1`r`n, %A_WinDir%\system32\mms.cfg
  OutputVar := "", lastline := ""

  MsgBox, 4160, 恭喜, 视频网站提速成功！`r`n`r`n如果浏览器用的是“搜狗”，需要勾选“设置”——“页面设置”——“使用系统公用的 Flash Player (需重启浏览器)”, 10
}
ExitApp
```

脚本很直白，无需再解释了。有重复代码，对新人而言先理解较重要。

## 评论中[芍青的脚本](http://zhuanlan.zhihu.com/autohotkey/19794762#comment-57490745)
经过简单的风格整理：

```ahk
f := {SilentAutoUpdateEnable:0, AutoUpdateDisable:0, ProtectedMode:0, RTMFPP2PDisable:1}
B := FileRead(L := "C:\Windows\" (A_Is64bitOS ? "SysWOW64" : "system32") "\Macromed\Flash\mms.cfg")
Loop, parse, B, `n, `t `r
  A .= ((f[P := SubStr(A_LoopField, 1, InStr(A_LoopField, "=") -1)] = "") ? A_LoopField : P"="f[P] ) "`n"
FileAppend(Trim(A, " `n"), L, 1, "")
```

思路清晰、代码紧凑，使用了内嵌赋值和对象，新手看起来可能不太直观。此外，注意其中另外设置的几个参数是其他用途的。
没有设置 %windir%\system32\mms.cfg 文件（只设置了一个文件），且后面用 FileAppend 而没有删除原来的内容
上面的 FileRead 可能应为 FileOpen()，后者是函数（也可能他另行包装了个 FileRead() 函数）。
？是否在文件不存在时会创建？是否在文件中不存在 RTMFPP2PDisable 时会添加？

## amnesiac 的改进脚本

```ahk
; Flash 配置文件的列表，集中在一起方便扩展。如果使用其他内置 Flash 的浏览器，请将其包含的 mms.cfg 文件（含路径）追加到这个变量中
MMSFileList =
(
%A_WinDir%\system32\Macromed\Flash\mms.cfg
%A_WinDir%\syswow64\Macromed\Flash\mms.cfg
%A_WinDir%\system32\mms.cfg
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\Pepper Data\Shockwave Flash\System\mms.cfg ; Chrome 内置 Flash 的配置文件
)
; 把每个文件解析出来
Loop, Parse, MMSFileList, `n, %A_Space%%A_Tab% ; ??
{
  MsgBox, % A_LoopField
  If FileExist(A_LoopField)
    DisableP2P(A_LoopField)
}
MsgBox, 禁用 Flash 的 P2P 上传功能已完成！

; 在文件中中修改或增加 RTMFPP2PDisable=1
DisableP2P(mmscfg)
{
  FileRead, content, %mmscfg%

  IfNotInString, content, RTMFPP2PDisable
  {
    FileAppend, RTMFPP2PDisable=1`n, %mmscfg%
    return
  }
  IfInString, content, RTMFPP2PDisable=0
  {
    StringReplace, content, content, RTMFPP2PDisable=0, RTMFPP2PDisable=1
    FileDelete, %mmscfg%
    FileAppend, %content%, %mmscfg%, CP936
  }
}
```

扩展性较好，便于自己或他人后续维护。

## 真相到底是什么
为什么上传会影响下载速度呢（具体表现是上传速度较大时下载明显卡起来）？http://www.williamlong.info/archives/3304.html

所以解决的方法很简单，通过修改 Flash 配置文件来禁用其 P2P 功能。此操作没有任何副作用（经过我几个月使用的实际测试），效果非常显著（现在非常非常少的时候我看“超​清”会卡）。
