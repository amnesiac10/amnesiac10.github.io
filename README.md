AutoHotkey 特点介绍、编写经验及使用心得等。<br /><br />如果喜欢，请分享到微博、微信等。

博客相关说明：

本项目派生自 [Yonsm.NET](https://github.com/Yonsm/NET)，在此基础上做了下列调整：<br />
最重要的一点是，派生本项目并拉到本地后无需安装 Jekyll 等诸多工具（在 Windows 中安装对普通用户较困难，此时无法本地预览），直接新建 md 文件编辑后上传即可（熟悉 md 语法后预览功能纯属多余）。

* 在主页添加“Fork me on the GitHub”彩带
* 在页面右侧增加百度分享
* 修改文章下的评论插件：从多说改为友言
* 修正了页面底部“Powered by Jekyll @ GitHub”中的绝对链接并提取为变量，可直接在配置中设置
* 增加了语法高亮插件 pygments 所需文件 pygments.css，不过对于 ahk 高亮样式有点丑
* 高亮语法为 {% highlight ahk %}、{% endhighlight %}。
* 调整源文件结构：[从命名文件夹改为命名 HTML 文件](http://jekyllcn.com/docs/pages/)
* 把[到内部资源（如图片或其他文件）的链接使用 {{ site.url }} 表示](http://jekyllcn.com/docs/posts/#section-3)，而不使用相对链接或具体网址的绝对链接
* [文章内链使用 post_url 变量表示](http://jekyllcn.com/docs/templates/#post-url)
* 把引用的谷歌网络字体库替换为国内的缓存，如将 Google 免费字体库的域名 http://fonts.googleapis.com 修改为：http://fonts.useso.com 即可

* 文章内链
	* THML：`{% post_url 2010-07-21-name-of-post %}`
	* MD：`[Name of link]({% post_url 2010-07-21-name-of-post %})`
* 图片：`![备用图片提示文本]({{ site.url }}/assets/images/20140720000.png)`
* 其他文件：`[备用资源提示文本]({{ site.url }}/assets/downloads/12345.pdf)`

<br />
一些注意事项：
* 图片和下载的文件分别放在 `assets` 目录的 `images` 和 `downloads` 中
* 引用文件时使用站点根目录的绝对路径，如 `![有帮助的截图]({{ site.url }}/assets/screenshot.jpg)`
* 调整每篇文章下方的”上一篇“和”下一篇“到右边对齐
* 能否使用 page.id 代替 page.thread？page.thread 好像只是多说在使用，目前改为友言已无必要。
* 使用分页器变量重新设置文章底部的页面导航
* 每次编辑时，语法高亮最后设置，对于简单的编辑直接在网页中进行而不本地编辑
* 每次本地修改前首先 git pull 从服务器版本库获取更新，本地修改后上传（以防在线修改的丢失）

