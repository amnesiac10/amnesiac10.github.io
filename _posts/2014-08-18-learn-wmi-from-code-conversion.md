---
date: 2014-08-18 14:06:23+08:00
layout: post
title: 学习 WMI 从代码转换开始
categories: 进阶
tags: WMI COM
---

导言：最近有幸邀请到 RobertL（AutoHotkey 中文论坛管理员）一起写本专栏，在论坛上看到他的诸多回答时让我感叹他的思路一针见血，接下来期待他的好文章，这里也必须感谢知乎对本专栏的大力支持，尽管专栏主题很小众。WMI 功能强大，尤其在系统管理方面，即使你不打算使用它，你也几乎一定会遇到使用它的代码。但由于 WMI 体系结构庞大，因此初学者**学习 WMI 的难点在于如何找到适合的命名空间、类和相应的属性、方法或事件来实现我们需要的功能**。不过很容易注意到，使用 WMI 的代码结构异常简单且网上（尤其是 MSDN）有大量现成的实现各种各样功能的 WMI 代码，所以如果能找到使用其他脚本语言的 WMI 代码并将其转换为 AutoHotkey，那么就绕过了这个难点。

目前关于 Windows 系统管理的脚本中，以 VBScript 居多（例如 MSDN 中的 WMI 代码都是使用这种脚本语言），所以本文以 VBScript 代码的 WMI 脚本为例说明转换为 AutoHotkey 代码的过程，如果看到其他语言的代码也可以参照这个过程，例如对于 VB、JScript、AutoIt 等。本文根据 WMI 代码的不同实现方式把它们分成三类：查看 WMI 属性、执行 WMI 方法和接收 WMI 事件。

注：本文重点说明转换过程中的一些情况，但不会解释 WMI 代码的含义，如果您尚不了解 WMI 基础知识，请先参阅 WMI 脚本第一阶系列教程。

## 查看属性
### 分析脚本
下面这个 VBScript 脚本显示操作系统的名称：

```vbscript
strComputer = "." 
Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\CIMV2") 
Set colItems = objWMIService.ExecQuery( _
    "SELECT * FROM Win32_OperatingSystem",,48) 
For Each objItem in colItems 
    Wscript.Echo "-----------------------------------"
    Wscript.Echo "Win32_OperatingSystem instance"
    Wscript.Echo "-----------------------------------"
    Wscript.Echo "Caption: " & objItem.Caption
Next
```

先分析一下这段 VBScript 代码：

* 第一行赋值，这里的一个句点表示本地计算机。 
* 第二行连接到WMI服务。 
* 第三、四行查询WMI服务。 
* 第五行至代码末尾为For循环，并在循环中显示内容。 

### 转换脚本
现在直接看看转换过来的 AutoHotkey 脚本：

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
colItems := objWMIService.ExecQuery(""
    . "SELECT * FROM Win32_OperatingSystem", ComObjMissing(), 48) 
For objItem in colItems {
    MsgBox, %  "-----------------------------------"
    MsgBox, %  "Win32_OperatingSystem instance"
    MsgBox, %  "-----------------------------------"
    MsgBox, %  "Caption: " . objItem.Caption
}
return
```

对比这两段代码，简单说说基本的转换方法：

* 第一行，修改赋值语法：AutoHotkey 中的表达式赋值使用 :=，所以这里把 = 修改为 :=。 
* 第二行，修改赋值语法、对应函数和字符串连接符：去除行首的 Set，在 AutoHotkey 中连接 WMI 服务使用 ComObjGet\(\) 函数，并且字符串连接符为句点。 
* 第三、四行，修改续行方式：去除前一行末尾的下划线并在下一行开始处加上一个句点及空格（这里用于连接字符串），由于前一行末尾没有需要连接的变量或字符串，所以另加一对空字符串。 
* 第五行至代码末尾，修改 For 循环语法：去除 For 的 Each 关键字，并在末尾加上左大括号（注意它的前面至少需要一个空格或 TAB），并把循环后面的 Next 替换为右大括号。 
* 在循环中，修改显示命令：把 Wscript.Echo 替换为 `MsgBox, %`，因为前者直接接表达式，所以 MsgBox 后需加百分号。 

### 调整转换的脚本
这样的转换比较直接，除了个别部分其他都可以用脚本实现转换。这里做些简单的修改， ExecQuery\(\) 的参数不长就不用续行了，循环中的几个消息框相互关联所以放在一起显示（至于字符串连接符——句点，可以保留，也可以去除，这里不管它了）：

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
colItems := objWMIService.ExecQuery("SELECT * FROM Win32_OperatingSystem", ComObjMissing(), 48) 
For objItem in colItems {
    MsgBox, %  "-----------------------------------"
      . "Win32_OperatingSystem instance"
      .  "-----------------------------------"
      .  "Caption: " . objItem.Caption
}
return
```

### 可选参数的默认值
前面代码的差异中，有重要的一点没有提到：在 VBScript 代码中 ExecQuery\(\) 方法省略了第二个参数表示这个参数是可选的并且使用它的默认值，但在 AutoHotkey 中在调用 COM 的方法时若可选参数后还有其他参数，那么这个参数不能省略，可以使用默认值或者用 ComObjMissing\(\) 代替，所以下面两者效果一样（这个方法的这个参数的默认值是字符串 WQL）：

```autohotkey
colItems := objWMIService.ExecQuery("SELECT * FROM Win32_OperatingSystem", "WQL", 48)
colItems := objWMIService.ExecQuery("SELECT * FROM Win32_OperatingSystem", ComObjMissing(), 48)
```

这点也适用于转换 VB 家族其他语言的情况。

### 补充自定义函数
在一些情况下会遇到 AutoHotkey 中没有与 VBScript 中相对应的命令或函数，此时要考虑自定义函数了。在下面这个例子中，由于这个属性的值是数组，所以需要先进行处理才能显示出来（原来的 VBScript 代码中使用 Join\(\)，不过这是它的内置函数）。

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
colItems := objWMIService.ExecQuery("SELECT * FROM Win32_BIOS", ComObjMissing(), 48) 
For objItem in colItems {
    MsgBox, % "-----------------------------------"
      . "`nWin32_BIOS instance"
      . "`n-----------------------------------"
      . "`nBiosCharacteristics: " . Join(objItem.BiosCharacteristics, ",")
}
return

Join(arrList, strDelimiter)
{
    For strItem in arrList
    {
        If (A_Index = 1)
          strList := strItem
        strList .= strDelimiter . strItem
    }
    Return, strList
}
```

在 WMI 中有许多属性的值是数组，很可能需要这个函数。另外，许多 WMI 属性的值是时间：

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
colItems := objWMIService.ExecQuery("SELECT * FROM Win32_OperatingSystem", ComObjMissing(), 48) 
For objItem in colItems {
    MsgBox, % "-----------------------------------"
      . "`nWin32_OperatingSystem instance"
      . "`n-----------------------------------"
      . "`nInstallDate: " . objItem.InstallDate
}
return

WMIDateStringToDate(dtmDate)
{
    WMIDateStringToDate := SubStr(dtmDate, 5, 2) . "/"
    . SubStr(dtmDate, 7, 2) . "/" . SubStr(dtmDate, 1, 4) 
    . " " . SubStr(dtmDate, 9, 2) . ":" . SubStr(dtmDate, 11, 2) . ":" . SubStr(dtmDate,13, 2)
    Return, WMIDateStringToDate
}
```

WMI 中时间格式类似于 20101220164120.000000+480，看起来不太方便，这时一般需要进行适当的处理。

## 执行方法
下面这个 VBScript 脚本把计算机名称从 MS-201012201636 修改为 NewComputerName：

```vbscript
strComputer = "." 
Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\CIMV2") 
' Obtain an instance of the the class 
' using a key property value.
Set objShare = objWMIService.Get("Win32_ComputerSystem.Name='MS-201012201636'")

' Obtain an InParameters object specific
' to the method.
Set objInParam = objShare.Methods_("Rename"). _
    inParameters.SpawnInstance_()

' Add the input parameters.
objInParam.Properties_.Item("Name") =  "NewComputerName"
objInParam.Properties_.Item("Password") =  ""
objInParam.Properties_.Item("UserName") =  "Administrator"

' Execute the method and obtain the return status.
' The OutParameters object in objOutParams
' is created by the provider.
Set objOutParams = objWMIService.ExecMethod("Win32_ComputerSystem.Name='MS-201012201636'", "Rename", objInParam)

' List OutParams
Wscript.Echo "ReturnValue: " & objOutParams.ReturnValue
```

转换并进行简单调整后的 AutoHotkey 脚本：

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
; Obtain an instance of the the class 
; using a key property value.
objShare := objWMIService.Get("Win32_ComputerSystem.Name='MS-201012201636'")

; Obtain an InParameters object specific
; to the method.
objInParam := objShare.Methods_("Rename").inParameters.SpawnInstance_()

; Add the input parameters.
objInParam.Properties_.Item("Name") := "NewComputerName"
objInParam.Properties_.Item("Password") := ""
objInParam.Properties_.Item("UserName") := "Administrator"

; Execute the method and obtain the return status.
; The OutParameters object in objOutParams
; is created by the provider.
objOutParams := objWMIService.ExecMethod("Win32_ComputerSystem.Name='MS-201012201636'", "Rename", objInParam)

; List OutParams
MsgBox, % "ReturnValue: " . objOutParams.ReturnValue
return
```

除了把行注释符替换为分号，这里不进行更多的解释了。把其中的两处 MS-201012201636 替换为您计算机的名称（请在【系统属性】对话框中查看计算机名），就可以执行这个代码来修改计算机名称了（其中输入参数中的用户名和密码可能需要根据具体情况进行调整）。

## 接收事件
### 同步监听
下面这个 VBScript 脚本监听进程创建、关闭事件：

```vbscript
strComputer = "." 
Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\CIMV2") 
Set objEvents = objWMIService.ExecNotificationQuery _
("SELECT * FROM Win32_ProcessTrace")

Wscript.Echo "Waiting for events ..."
Do While(True)
    Set objReceivedEvent = objEvents.NextEvent

    'report an event
    Wscript.Echo "Win32_ProcessTrace event has occurred."
Loop
```

转换并进行简单调整后的 AutoHotkey 脚本：

```autohotkey
strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
objEvents := objWMIService.ExecNotificationQuery("SELECT * FROM Win32_ProcessTrace")

MsgBox, % "Waiting for events ..."
Loop {
    objReceivedEvent := objEvents.NextEvent

    ; report an event
    ToolTip, % "Win32_ProcessTrace event has occurred."
}
return
```

为什么这里把 Wscript.Echo 替换为 ToolTip 而不是 MsgBox 呢？因为 MsgBox 是阻塞的，在显示对话框时可能会错过其他事件。里面包含的无限循环用来持续进行监视，对于所发生事件的相关信息可以从接收到的事件对象中获取（即 objReceivedEvent）。使用同步方法监听事件这个脚本就没办法做别的事情了，请看后面的解决方法。

### 异步监听
下面这个 VBScript 脚本与前一个的用途相同，也是监听进程的创建和关闭事件，不过这里使用异步方法：

```vbscript
strComputer = "." 
Set objWMIService = GetObject("winmgmts:\\" & strComputer & "\root\CIMV2") 
Set MySink = WScript.CreateObject( _
    "WbemScripting.SWbemSink","SINK_")

objWMIservice.ExecNotificationQueryAsync MySink, _
    "SELECT * FROM Win32_ProcessTrace"

WScript.Echo "Waiting for events..."

While (True)
    Wscript.Sleep(1000)
Wend

Sub SINK_OnObjectReady(objObject, objAsyncContext)
    Wscript.Echo "Win32_ProcessTrace event has occurred."
End Sub

Sub SINK_OnCompleted(objObject, objAsyncContext)
    WScript.Echo "Event call complete."
End Sub
```

转换并进行简单调整后的 AutoHotkey 脚本：

```autohotkey
#Persistent

strComputer := "." 
objWMIService := ComObjGet("winmgmts:\\" . strComputer . "\root\CIMV2") 
MySink := ComObjCreate("WbemScripting.SWbemSink")
ComObjConnect(MySink, "SINK_")

objWMIservice.ExecNotificationQueryAsync(MySink, "SELECT * FROM Win32_ProcessTrace")

MsgBox, % "Waiting for events..."
return

SINK_OnObjectReady(objObject, objAsyncContext) {
    ToolTip, % "Win32_ProcessTrace event has occurred."
}

SINK_OnCompleted(objObject, objAsyncContext) {
    ToolTip, % "Event call complete."
}
```

其中需要重点注意的是把

```vbscript
Set MySink = WScript.CreateObject( _
    "WbemScripting.SWbemSink","SINK_")
```

替换为了：

```autohotkey
MySink := ComObjCreate("WbemScripting.SWbemSink")
```

`ComObjConnect(MySink, "SINK_")` 还有原来 VBScript 脚本中的无限空循环被替换为 #Persistent 指令，这样可以在脚本中执行其他操作。从这里可以看出，在具体情况中需要进行灵活的替换，不应该拘泥于某种固定的模式。

对于 WMI 事件，建议采用后面这种异步方式，这样一个脚本中可以同时监听多个事件，还可以在监听事件的同时执行其他操作（虽然使用多个脚本或多线程也可以实现，然而会复杂多了）。

## 小结
从前面的例子可以看出，转换过程中语法的转换比较简单，在实际情况中应根据需要灵活进行转换。此外，WMI 是 COM 的一个部分，所以这样的转换方法也可以扩展到使用 COM 的脚本。

