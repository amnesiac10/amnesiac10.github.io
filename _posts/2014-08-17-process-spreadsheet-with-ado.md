---
date: 2014-08-17 14:16:23+08:00
layout: post
title: 使用 ADO 操作 Excel 文档
categories: 进阶
tags: ADO Excel COM
---

注：本文改写自微软知识库文章，待找到源网址后补上。

Office 家族系列软件功能强大，更强大的是通过相应的对象模型可以把这些软件脚本化，唯一的问题可能是每个软件都有自己专用对象模型。例如如果要操作 Word 或 Excel，您必须学习两种对象模型，这样并不是说学习它们很难，意思是例如知道如何添加数据到 Word 文档并不能给您在添加数据到 Excel 文档时带来多少帮助。

所以在一些只需要执行基本操作的情况下，使用 ADO 操作 Excel 文档可能是较好的选择。比起 Excel 对象模型，使用 ADO 具有以下优点：

* ADO 对象模型简单易学。
* ADO 且可用于 CSV、TSV、xls、mdb 等多种文件类型。
* ADO 中通过 SQL 查询可以方便地进行过滤和聚合等。
* 使用 ADO 是在进程内执行，速度快（请参阅 [COM 对象的进程内、外运行]({{ site.url }}/2014/08/15/com-in-and-out-of-process.md)）。
* 无需安装 Excel，这可节省了一笔不小的费用（基于国情，这点放最后）。

## 测试文件样本

一般而言，除了需要通过脚本演示在 Excel 中进行的操作和使用 Excel 中专有的一些高级功能外，基本上 ADO 都可以满足要求。下面将通过一个简单的电子表格说明如何使用 ADO 访问电子表格。（如果一定还要寻找其他适合使用 Excel 的原因，那么很可能是您对 Excel 对象模型很熟悉，而目前对 ADO 还不大了解，这样看过本文后也许会给您带来较大的收获。）

下面是一个简单的电子表格文件 C:\Scripts\Test.xls：
![Excel 中的样本数据]({{ site.url }}/assets/images/20140817000.png)

具体的内容结构：一个标签为 Name，另一个为 Number。为了确保您能对 Excel 电子表格使用数据库查询，必须让电子表格符合简单的风格：让第一行作为标题行，从第二行开始为数据，并且不要跳过任何行或列。同时为了让代码简单些，在标题中不要包含空格，例如使用 SocialSecurityNumber 作为列标题代替 Social Security Number。

## 访问样本数据

现在我们看看使用 ADO 来访问这样电子表格数据的代码：

{% highlight ahk %}
adOpenStatic := 3
adLockOptimistic := 3
adCmdText := 0x0001

objConnection := ComObjCreate("ADODB.Connection")
objRecordSet := ComObjCreate("ADODB.Recordset")

objConnection.Open("Provider=Microsoft.Jet.OLEDB.4.0; Data Source=C:\Scripts\Test.xls; Extended Properties=""Excel 8.0;HDR=Yes;"";")

objRecordset.Open("Select * FROM [Sheet1$]", objConnection, adOpenStatic, adLockOptimistic, adCmdText)

while !objRecordset.EOF
{
  MsgBox, % objRecordset.Fields.Item("Name") objRecordset.Fields.Item("Number")
  objRecordset.MoveNext
}
{% endhighlight %}

运行代码后结果像这样：

> A 1  
> B 1  
> C 2  
> D 2  
> E 1  
> F 1  

## 代码分析

开始部分定义了一些常量和两个对象——ADODB.Connection 和 ADODB.Recordset，这两个对象用来连接数据源并从中获取数据。这几乎是所有 ADO 脚本中的模板了，这里不会对这部分进行详细的说明。

现在来看看接下来这行代码，它实际上建立了到 Excel 电子表格的连接：

{% highlight ahk %}
objConnection.Open("Provider=Microsoft.Jet.OLEDB.4.0; Data Source=C:\Scripts\Test.xls; Extended Properties=""Excel 8.0;HDR=Yes;"";")
{% endhighlight %}

其中 Data Source 部分指定了电子表格的文件路径和名称。如果文件名称中包含空格呢？仍然直接写就行了，例如 Data Source=C:\Scripts\My Spreadsheet.xls。

注意，您可能会把连接字符串中的 Excel 8.0 改为您电脑上安装的 Excel 版本，但会执行错误，因为这里的 Excel 8.0 并非电脑上安装的 Excel 程序的版本，而是指用来访问 Excel 文档提供者（Provider）的版本。

还有，其中的 HDR=Yes 表示电子表格含有标题行，如果没有标题行则设置 HDR 为 No。

连接到数据源后，使用 SQL 查询来获取其中的数据。这是用来返回包含了电子表格中所有行的记录集的代码：

{% highlight ahk %}
objRecordset.Open("Select * FROM [Sheet1$]", objConnection, adOpenStatic, adLockOptimistic, adCmdText)
{% endhighlight %}

这里只需要关心 SQL 查询的参数，Select * FROM [Sheet1$] 是标准的 SQL 查询，选择数据库（工作表）中的所有字段（列）。在查询中指定了工作表的名称，需要注意它的格式：工作表名称后面附加了 $ 符且括在方括号中。

操作这里得到的记录集和操作从 SQL Server 得到的记录集没多大区别，因此可以用下面这些代码简单地显示记录集中每个记录中的 Name 和 Number 字段：

{% highlight ahk %}
while !objRecordset.EOF
{
  MsgBox, % objRecordset.Fields.Item("Name") objRecordset.Fields.Item("Number")
  objRecordset.MoveNext
}
{% endhighlight %}

任务完成了，很漂亮。

## 进一步思考

您可能会产生这样的疑问：不能使用 Excel 对象模型来获取这样的信息吗？确实可以这么做。我承认，如果只是要显示电子表格中的每行内容，使用 ADO 并没有体现出多少好处（好处还是有的，或许您可以使用 Excel 对象模型实现相同的任务，并比较它们的代码，不过这个不是决定性的好处）。

不过，设想一下如果我们只要显示 Number 等于 2 的那些行呢？使用 Excel 脚本我们需要检查每一行的 Number 是否等于 2，以决定是否显示出来。这样不难，但却很麻烦，尤其当您需要检查多个列的时候（例如您需要寻找在 Finance 部门且头衔为 Administrative Assistant 的所有用户）。比较起来，通过 ADO 我们可以不用检查电子表格中的每行，唯一需要做的只是修改 SQL 查询，即使用类似下面的查询就可以获取 Number 等于 2 的所有行：

{% highlight ahk %}
objRecordset.Open("Select * FROM [Sheet1$] Where Number = 2", objConnection, adOpenStatic, adLockOptimistic, adCmdText)
{% endhighlight %}

这样我们就能得到下面的结果：

> C 2  
> D 2  

从这里可以看出，在 ADO 使用 SQL 查询进行过滤很方便，实际上除了过滤，还可以方便地进行排序、聚合等操作。

## 小结

本文中简单介绍了使用 ADO 从 Excel 电子表格中读取数据，主要说明了在一些情况下使用 ADO 访问 Excel 电子表格比使用 Excel 更好的原因，实际上，使用 ADO 除了能从 Excel 中获取数据外，还可以写入数据。可以把 ADO 先看成一种通用数据访问接口，详细特性请参阅微软的 ADO 手册，简单了解可参考 [ADO 教程](http://www.w3school.com.cn/ado/index.asp)。

对于普通用户而言，本文缺乏背景介绍学习起来有难度，但与其他技术类似，对于这些高级技术的学习不应局限在 AutoHotkey 自身帮助，本身也不属于它的内容。需要自己从其他资料或语言中学习，例如 Windows API、COM、SQL、WMI，这些一般都是语言独立的。
