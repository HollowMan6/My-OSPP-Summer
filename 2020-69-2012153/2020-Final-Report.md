# 结项报告

蒋嵩林，GNU Emacs（中文）社区

项目地址：https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153

## 项目信息

### 项目名称

Invent a better dark mode color algorithm for PDF pages **and** exclude images and graphs

### 方案描述

此项目需要完成[Todo List](https://github.com/manateelazycat/emacs-application-framework/wiki/Todo-List)中所有（7个）低难度任务，7个中等难度项目，1个高难度项目。

### 时间规划

中期检查之前完成所有（7个）低难度任务外加2个Terminal相关的中等难度任务，结项检查之前完成所有任15个任务。

## 项目总结

#### [EAF浏览器可以支持快捷键选中文本功能了！](https://manateelazycat.github.io/emacs/eaf/2020/07/22/eaf-support-caret-browse.html)

### 项目成果
  
- **2020.6.11-8.7** 完成Invent a better dark mode color algorithm for PDF pages **and** exclude images and graphs中关于排除图片部分并完成暗颜色算法的设计

- **2020.6.13** 完成Core: Investigate the possibility to support chemacs. If possible, support it, if not, explain. [#297](https://github.com/manateelazycat/emacs-application-framework/issues/297)

- **2020.7.1-7.2** 完成Core: English documentation for all functions

- **2020.7.2** 完成Terminal: Fix error in connection establishment: net::ERR_CONNECTION_REFUSED和Terminal: Refresh page after server is ready because sometimes terminal is blank when opened

- **2020.7.3-7.7** 完成Terminal: Close terminal buffer with EXIT signal

- **2020.7.3-7.28** 完成Terminal: Catch change directory signal in node-pty, rename terminal buffer and change Emacs default-directory variable real-time，并完成添加程序运行指示的附加任务

- **2020.7.4** 完成Mindmap: Change node color, pick from color list

- **2020.7.5-7.7** 完成Mindmap: Support multi-line insert dons nodes

- **2020.7.5-7.6** 完成Mindmap: Export org with correct indent level


- **2020.7.12-7.13** 完成Browser: Create ad-blocker plugin

- **2020.7.17-7.19** 完成Browser: Autofill password by storing password in a database

- **2020.7.21-8.5** 完成Browser: Smart select text area using cursor和Browser: Mouseless browsing. [#270](https://github.com/manateelazycat/emacs-application-framework/issues/270)，额外增加用 marker 选中文本模块，Caret适配暗模式。

- **2020.8.25** 额外完成Add PDF Rotation function for PDF viewer [#379](https://github.com/manateelazycat/emacs-application-framework/issues/379)

- **2020.9.7** 额外完成Add set default zoom for each website function [#392](https://github.com/manateelazycat/emacs-application-framework/issues/392)

- **2020.9.22** 额外完成Browser: Support duplicating current page in new tab

### 遇到的问题及解决方案

#### 1. Core: Investigate the possibility to support chemacs. If possible, support it, if not, explain. 

**相关issue**: [#297](https://github.com/manateelazycat/emacs-application-framework/issues/297)，[#1](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/1)

***相关PR***: [#309](https://github.com/manateelazycat/emacs-application-framework/pull/309)

经过亲自尝试，确认[Chemacs](https://github.com/plexus/chemacs)支持EAF， 下面是一份 `init.el`的配置示例：

```Elisp
;; An example of init.el
(defun add-subdirs-to-load-path (dir)
  "Recursive add directories to `load-path'."
  (let ((default-directory (file-name-as-directory dir)))
    (add-to-list 'load-path dir)
    (normal-top-level-add-subdirs-to-load-path)))
(add-subdirs-to-load-path "~/.emacs.d/")
(require 'eaf)
```

在安装EAF时，你仅需要修改此文件中调用函数`add-subdirs-to-load-path`的参数值，将其更改为你的EAF安装位置的绝对路径。 配置示例中的EAF是安装在`~/.emacs.d/`.

在EAF安装完成后，假设你将它安装到了`~/eaf`,你电脑中只有存放在`~/.emacs.d`的Emacs配置文件和安装的EAF，并且你电脑中安装了Chemacs，此时`.emacs-profiles.el`应该这样配置:

```Elisp
(("default" . ((user-emacs-directory . "~/.emacs.d")))
 ("EAF" . ((user-emacs-directory . "~/eaf"))))
```

当你想使用Chemacs打开EAF，只需在终端中执行`emacs --with-profile EAF`。

#### 2. Core: English documentation for all functions

**相关issue**: [#2](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/2)

***相关PR***: [#309](https://github.com/manateelazycat/emacs-application-framework/pull/309) ，[#320](https://github.com/manateelazycat/emacs-application-framework/pull/320) ，[#323](https://github.com/manateelazycat/emacs-application-framework/pull/323)

给elisp和python函数增加注释，直接写到代码里。同时按照Emacs的elisp函数注释要求，在另外的PR中修复拼写错误和第一行需要函数parameter的全大写问题。

#### 3. Terminal: Close terminal buffer with EXIT signal

**相关issue**: [#3](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/3)

***相关PR***: [#324](https://github.com/manateelazycat/emacs-application-framework/pull/324)，[#334](https://github.com/manateelazycat/emacs-application-framework/pull/334)

在开始工作时遇到了Terminal报错：`js: Uncaught ReferenceError: Terminal is not defined`，随后通过查阅相关资料，暂时性地绕过了此错误，并在issue中提出了自己的[临时解决方案](https://github.com/manateelazycat/emacs-application-framework/issues/310#issuecomment-652726781)

一开始使用方案是监听socket关闭方法，但是这种方法不稳定，随后换成了监控是否执行`exit`命令，但是仍然会出BUG，因而最后通过监听 server.js 的子进程状态来决定什么时候退出，并且修复了此BUG：

```bash
Traceback (most recent call last):
  File "/home/mt/.emacs.d/site-elisp/emacs-application-framework/app/terminal/buffer.py", line 87, in on_change_directory
    self.update_title()
  File "/home/mt/.emacs.d/site-elisp/emacs-application-framework/app/terminal/buffer.py", line 92, in update_title
    self.change_title(self.buffer_widget.execute_js("title"))
  File "/home/mt/.emacs.d/site-elisp/emacs-application-framework/core/buffer.py", line 209, in change_title
    self.update_buffer_details.emit(self.buffer_id, new_title, self.url)
TypeError: Buffer.update_buffer_details[str, str, str].emit(): argument 2 has unexpected type 'QVariant'
```

#### 4. Terminal: Catch change directory signal in node-pty, rename terminal buffer and change Emacs default-directory variable real-time

**相关issue**: [#4](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/4)

***相关PR***: [#325](https://github.com/manateelazycat/emacs-application-framework/pull/325)，[#329](https://github.com/manateelazycat/emacs-application-framework/pull/329)，[#331](https://github.com/manateelazycat/emacs-application-framework/pull/331)，[#351](https://github.com/manateelazycat/emacs-application-framework/pull/351)

我发现当更改目录时，服务器将向前端发送消息。这是将目录更改为`~`时发送的消息的示例：

`]0;hollowman@hollow-manjaro:~[01;32m[hollowman@hollow-manjaro[01;37m ~[01;32m]$`

因此，当信号发生时，我使用正则表达式`/:([^\x07].*?)\x07/g`提取目录信息并将其存储在变量中：

 > ~~]0;hollowman@hollow-manjaro:~~ `~` ~~[01;32m[hollowman@hollow-manjaro[01;37m \~[01;32m]$~~ 

因而，`AppBuffer`(Buffer.py)每250毫秒检查一次title变量，如果它检测到任何变化，则on_change_directory方法将设置标题和Emacs的默认目录。

随后修复以下BUG：

```bash
[EAF] Killed 54ab-347e-492b-c905-3201-aa3d-fdbc.
[EAF] Killed 0aec-a9cf-9048-13f3-8de6-132f-c8e0.
[EAF] Killed 2 EAF buffers
[EAF] eaf 已杀死
[EAF] Process terminated.
apply: Setting current directory: 没有那个文件或目录, None
Mark set [2 times]
Error running timer: (file-missing "Setting current directory" "没有那个文件或目录" "None") [2 times]
```

最后对于附加任务，添加对重命名ssh下的终端缓冲区的支持，下面提供了ssh下的终端缓冲区名称示例：
`user@example.com:~/`

添加对运行程序时重命名终端缓冲区的支持，下面提供了运行程序时终端缓冲区名称的示例：
`test- ~/`

但是，最终因为系统适配问题，虽然此方法在我的环境中运行无问题，但是在一些系统上运行会出问题，因而未能合并进入上游，最终运行程序时重命名终端缓冲区合并进入上游，ssh问题正在解决。

#### 5. Mindmap: Change node color, pick from color list

**相关issue**: [#5](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/5)

***相关PR***: [#326](https://github.com/manateelazycat/emacs-application-framework/pull/326)，[#327](https://github.com/manateelazycat/emacs-application-framework/pull/327)

首先我制作了一个网页端鼠标操作，支持从颜色列表中更改文本和背景颜色，或自定义自己的颜色。

![](https://user-images.githubusercontent.com/43995067/86486137-1ba41d00-bd8d-11ea-9dcc-2f0f2fe8d1be.png)

如果未选择任何节点，则颜色列表将自动关闭。

![](https://user-images.githubusercontent.com/43995067/86486226-50b06f80-bd8d-11ea-8363-2bd6571e108b.png)

但是，因为这样的操作不符合Emacs键盘操作的文化，因而未被合并。随后我提出了另外一个方案：

用户可以输入任何HTML颜色代码，而不必使用颜色列表。

要更改节点背景颜色时，只需按“ c”，然后输入任何格式的HTML颜色代码。

当您想要更改节点文本颜色时，只需按“ C”，然后输入任何格式的HTML颜色代码。

| 输入格式 | 之前 | 之后 |
| :--------: | :--------: | :----: |
|颜色名称| <img src="https://user-images.githubusercontent.com/43995067/86501174-149b0000-bdc9-11ea-8588-89d30f48a18f.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86501200-54fa7e00-bdc9-11ea-8144-5e3f17cab293.png" width="400">|
|RGB颜色代码| <img src="https://user-images.githubusercontent.com/43995067/86501221-8bd09400-bdc9-11ea-908f-92adec118d51.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86501233-a3a81800-bdc9-11ea-99f7-e7eab001ace6.png" width="400">|
|十六进制颜色代码| <img src="https://user-images.githubusercontent.com/43995067/86501238-b4588e00-bdc9-11ea-9841-9cdca71d21a0.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86501245-c89c8b00-bdc9-11ea-89d2-0cfce339014c.png" width="400">|

#### 6. Mindmap: Support multi-line insert dons nodes

**相关issue**: [#6](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/6)

***相关PR***: [#332](https://github.com/manateelazycat/emacs-application-framework/pull/332)

修复此BUG

#### 7. Mindmap: Export org with correct indent level

**相关issue**: [#7](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/7)

***相关PR***: [#332](https://github.com/manateelazycat/emacs-application-framework/pull/332)

如果要添加多个子节点，只需按“ Alt + q”，然后输入多行，每一行代表相应节点的主题。

如果要添加多个兄弟节点，只需按“ Alt + Enter”，然后输入多行，每一行代表相应节点的主题。

如果要添加多个中间节点，只需按“ Alt + i”，然后输入多行，每一行代表相应节点的主题。

如果您当前无法添加此类节点，则在按下键后将显示一条消息。

| 多次添加类型 | 之前 | 之后 |
| :--------: | :--------: | :----: |
|子节点| <img src="https://user-images.githubusercontent.com/43995067/86523331-7462db00-be9d-11ea-8122-d8eb29c64608.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86523335-89d80500-be9d-11ea-8081-c9335bcdb598.png" width="400">|
|兄弟节点| <img src="https://user-images.githubusercontent.com/43995067/86523343-b68c1c80-be9d-11ea-9dee-430a62c796ad.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86523347-cad01980-be9d-11ea-8536-17a2c1845ec1.png" width="400">|
|中间节点| <img src="https://user-images.githubusercontent.com/43995067/86523358-ed623280-be9d-11ea-93cc-d69614ebd110.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/86523376-1387d280-be9e-11ea-832c-a1db052a23c2.png" width="400">|

#### 8. Browser: Autofill password by storing password in a database

* [EAF浏览器可以支持快捷键选中文本功能了！](https://manateelazycat.github.io/emacs/eaf/2020/07/22/eaf-support-caret-browse.html)

**相关issue**: [#9](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/9)

***相关PR***: [#341](https://github.com/manateelazycat/emacs-application-framework/pull/341)

该数据库位于`config_dir/browser/password.db`

* 数据库结构

![](https://user-images.githubusercontent.com/43995067/87796338-d0edcf00-c87b-11ea-8f2d-3cc1bb2d90e2.png)

`password`收集类型为“密码”的输入。

`form_data`收集当前页面中除类型“password”和“ hidden”以外的所有输入字段。

* 记录表格

在我的配置下，准备登录之前，可以按`C-d`将表单数据记录到数据库中： 您可以将多个帐户添加到数据库中。 如果数据库中已经存在具有相同`form_data`记录的记录，那么它将仅更新密码。 它也可以支持不带密码的记录表格。

![](https://user-images.githubusercontent.com/43995067/87796995-b6682580-c87c-11ea-9bfe-baec0d5e2ad4.png)

![](https://user-images.githubusercontent.com/43995067/87797305-1232ae80-c87d-11ea-891d-5d160c8fea43.png)

* 自动填充

如果启用了自动填充功能，它将从id数据库中最小的表格开始填充表格。在我的配置下，按`C-t`切换自动填充并在不同的帐户之间切换。

#### 9. Browser: Create ad-blocker plugin. 

**相关issue**: [#19](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/19)

***相关PR***: [#322](https://github.com/manateelazycat/emacs-application-framework/pull/322)

为浏览器添加基于CSS的Adblock功能。

`core/adblock.css`是Adblock的CSS规则的一个例子，来源于http://www.floppymoose.com/，用户可以自定义他们自己的规则。

使用`C-d`启用或禁用Adblock的。

这是一个演示，演示了在提供的CSS文件下在我的网站上启用和禁用adblock时阻止Google AD的效果：

![](https://user-images.githubusercontent.com/43995067/87243870-22f1b780-c46c-11ea-83b0-cec1a4f51139.png)
![](https://user-images.githubusercontent.com/43995067/87243875-2c7b1f80-c46c-11ea-925f-8ec1b2323b21.png)

#### 10. Browser: Smart select text area using cursor 和 11.Browser: Mouseless browsing. 

**相关issue**: [#10](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/10)，[#11](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/11)，[#270](https://github.com/manateelazycat/emacs-application-framework/issues/270)

***相关PR***: [#345](https://github.com/manateelazycat/emacs-application-framework/pull/345)，[#349](https://github.com/manateelazycat/emacs-application-framework/pull/349)，[#357](https://github.com/manateelazycat/emacs-application-framework/pull/357)，[#364](https://github.com/manateelazycat/emacs-application-framework/pull/364)，[#367](https://github.com/manateelazycat/emacs-application-framework/pull/367)

为EAF浏览器添加插入光标浏览功能。它可以用于智能选择文本区域。

`core/js/caret_browsing.js` 部分来自于 [qutebrowser的caret.js](https://github.com/qutebrowser/qutebrowser/blob/b44e3ba657e622cf813d0c072d6fe538fca9bf2a/qutebrowser/javascript/caret.js)

* 用法

在我的按键配置下，按`M-c`激活插入符号浏览，`C-q`禁用。光标浏览默认情况下处于禁用状态。

如果光标浏览已激活，请按`C-i`切换是否开启标记。如果标记打开，将使用光标进行选择。如果标记关闭，则光标将仅根据命令移动。标记默认为关闭。

这是在我的配置下用于光标浏览的按键绑定列表，只有在激活光标浏览后才能使用。

| 按键   | 动作 |
|----|----|
| z | 将光标移到下一行 |
| Z | 将光标移到上一行 |
| a | 将光标移到下一个字符 |
| A | 将光标移到上一个字符 |
| w | 将光标移到下一个单词 |
| W | 将光标移到上一个单词 |
| b | 将光标移到页面底部 |
| B | 将光标移到页面顶部 |

如果标记已启用并且光标浏览已激活，请按`/`向前搜索文本并选择，`?`向后搜索文本并选择。`C-.`清除当前搜索文本。

* 例子

以此处 https://superuser.com/questions/1236864/how-to-use-vimium-to-select-text-from-a-page 为例，如果我们要获得相同的结果，则可以执行以下操作：

首先，按`M-c`激活光标浏览，然后按`C-i`设置标记。
其次，按`/`打开搜索并选择，在小缓冲区输入`mailto`，然后按`enter`确认。
第三，按`C-.`清除当前的搜索文本，然后再次按`/`打开搜索并选择，在小缓冲区输入`very`，然后按`enter`确认。
再次，按一次`w`选择一个单词或`a`逐渐选择一个单词。
最后，按`M-w`即可提取文字`verycomplicatedandlonganduglydomainthatiwontsearachfor.com`

额外增加用 marker 选中文本模块：

使用`M-D`或者`d`进入Marker文本选中模式

| 之前| 之后| 激活caret mode|按`d`光标右移后|
| :--------: | :--------: |  :--------: | :--------: |
| <img src="https://user-images.githubusercontent.com/43995067/89091770-aa34aa00-d3de-11ea-8cc0-50a0ee15b4ec.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/89091777-b7ea2f80-d3de-11ea-90ca-8cb23e6abf0d.png" width="400">|<img src="https://user-images.githubusercontent.com/43995067/89092765-4b276300-d3e7-11ea-9d61-c0a20e2611d6.png" width="400">|<img src="https://user-images.githubusercontent.com/43995067/89092830-bec97000-d3e7-11ea-8c12-ffd0c1118989.png" width="400">|

仅仅无`span`或者`p`祖宗节点和有 文本内容的`span`节点会被选中。

随后设计了一个minor mode叫做`eaf-browser-caret-mode`，继续通过`M-c`开启关闭，开启时希望能够覆盖Emacs熟悉的`C-n`，`C-p`，`C-f`，`C-b`进行光标控制，但是由于未知原因，minor mode无法覆盖已经在eaf-browser-keybinding中定义的按键，随后已经尝试修复，但是均失败。随后，EAF大神[@loyalpartner](https://github.com/loyalpartner)成功厘清了动态绑定的秘密，在[#360](https://github.com/manateelazycat/emacs-application-framework/pull/360)中帮我修复了错误，在这里感谢他的努力！

另外，增加暗模式插入符号：

从深色模式bgcolor内联样式中排除类`CaretBrowsing`。

优化“插入符号”浏览光标的暗模式颜色算法，使其可以遵循eaf-browser-dark-mode设置。

![image](https://user-images.githubusercontent.com/43995067/89268582-512d7600-d66b-11ea-98be-8d93457080ed.png)

修复浏览器焦点问题导致有时使用caret选中文字时为灰色而不是蓝色（焦点不在浏览器中）：

![Screen Capture_select-area_20200806111435](https://user-images.githubusercontent.com/12422335/89549494-5f4bd580-d7d6-11ea-99b1-c71b4f31a3a4.png)

#### 12. Terminal: Refresh page after server is ready because sometimes terminal is blank when opened 和 13. Terminal: Fix error in connection establishment: net::ERR_CONNECTION_REFUSED

**相关issue**: [#12](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/12)，[#13](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/13)

***相关PR***: [#336](https://github.com/manateelazycat/emacs-application-framework/pull/336)，[#337](https://github.com/manateelazycat/emacs-application-framework/pull/337)

成功重现此错误：

![](https://user-images.githubusercontent.com/43995067/86306609-4d12d080-bc47-11ea-8cff-e2c30301ecec.png)

在这种情况下，您仍然可以通过单击鼠标右键，在菜单中单击`reload`来访问终端，然后就会一切正常。

因此，我相信此错误是由过早加载网页引起的，并且我进行了异常处理，当这种情况发生时，在1秒后自动刷新页面，并且该错误再也不会发生。

#### 14. PDF: Invent a better dark mode color algorithm for PDF pages 和 15. PDF: Invent a better dark mode color algorithm for PDF pages **and** exclude images and graphs

**相关issue**: [#14](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/14)，[#15](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/15)

***相关PR***: [#308](https://github.com/manateelazycat/emacs-application-framework/pull/308)，[#350](https://github.com/manateelazycat/emacs-application-framework/pull/350)，[#356](https://github.com/manateelazycat/emacs-application-framework/pull/356)，[#365](https://github.com/manateelazycat/emacs-application-framework/pull/365)

启用暗模式时，将整个pdf反转后，可通过反转图像显示位置来实现此功能。

效果：

![](https://user-images.githubusercontent.com/43995067/84397468-9c408500-abf6-11ea-99fb-8242b332e506.png)

现在，`page_annotate_color`将遵循emacs主题前景色

如果`eaf-pdf-dark-mode`为`follow`，则pdf背景将为emacs主题背景色。

例：

![](https://user-images.githubusercontent.com/43995067/88458562-84c90d00-cec1-11ea-865a-510dd056a647.png)

> app/pdf-viewer/buffer.py#L331
> ```python
>  if self.emacs_var_dict["eaf-pdf-dark-mode"] == "follow": 
> ```
> 

![image](https://user-images.githubusercontent.com/43995067/89021524-3e592f80-d353-11ea-8f9b-2a1869eaec37.png)

因为`page.drawRect`目前只支持pdf文件，我增加了限制，避免打开epub的时候执行这段语句

另外，增加计算排除图像时的重叠部分：

有了这个补丁，现在我的简历可以在黑暗模式下使用PDF-Viewer正确显示

如果图片有重叠，将通过反转重叠部分来进一步优化[#308](https://github.com/manateelazycat/emacs-application-framework/pull/308)，[#350](https://github.com/manateelazycat/emacs-application-framework/pull/350) 中PDF查看器中的暗模式图像排除算法。

| 之前 | 当前 |
| :--------: | :--------: |
| <img src="https://user-images.githubusercontent.com/43995067/89279841-6eb60c00-d67a-11ea-8d71-6ae11fb0ad08.png" width="400"> |<img src="https://user-images.githubusercontent.com/43995067/89280057-afae2080-d67a-11ea-9140-2cd2227396ea.png" width="400">|

复杂测试案例：

3层重叠：[2.pdf](https://github.com/manateelazycat/emacs-application-framework/files/5022755/2.pdf)

![image](https://user-images.githubusercontent.com/43995067/89307928-3b887280-d6a4-11ea-923b-55372d6bcc37.png)

4层重叠：[3.pdf](https://github.com/manateelazycat/emacs-application-framework/files/5022763/3.pdf)

![image](https://user-images.githubusercontent.com/43995067/89308084-6e326b00-d6a4-11ea-89c0-bfdad8041901.png)

[4.pdf](https://github.com/manateelazycat/emacs-application-framework/files/5022772/4.pdf)

![image](https://user-images.githubusercontent.com/43995067/89308258-a174fa00-d6a4-11ea-8439-e18757269ef4.png)

#### 额外任务 16. Add PDF Rotation function for PDF viewer

**相关issue**: [#23](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/23)，[#379](https://github.com/manateelazycat/emacs-application-framework/issues/379)

***相关PR***: [#385](https://github.com/manateelazycat/emacs-application-framework/pull/385)

用`C-<right>`顺时针旋转PDF。

用`C-<left>`逆时针旋转PDF。

![image](https://user-images.githubusercontent.com/43995067/91159218-51fd7900-e6fa-11ea-978f-126c5a45a24c.png)

适配了现在所有PDF-Viewer已有功能，增加了session data保存旋转信息。

#### 额外任务 17. Add set default zoom for each website function

**相关issue**: [#24](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/24)，[#392](https://github.com/manateelazycat/emacs-application-framework/issues/392)

***相关PR***: [#393](https://github.com/manateelazycat/emacs-application-framework/pull/393)

该数据库位于`config_dir/browser/zoom_data.db`

* 数据库结构

![image](https://user-images.githubusercontent.com/43995067/92376039-8e869700-f134-11ea-9fbc-cffcdfbf9302.png)

`Host`收集网站主机名。

`ZoomScale`收集主机名的网页缩放系数。

* 原则

![image](https://user-images.githubusercontent.com/43995067/92376680-8713bd80-f135-11ea-9caf-d7386f675f19.png)

放大或缩小时，如果缩放系数不等于`eaf-browser-default-zoom`，则会记录到数据库中。当缩放系数等于`eaf-browser-default-zoom`时，该程序还会删除记录，以确保用户只需设置`eaf-browser-default-zoom`即可更改每个网站的缩放比例，并确保数据库不会增长过大。

#### 额外任务 18. Browser: Support duplicating current page in new tab

**相关issue**: [#26](https://isrc.iscas.ac.cn/gitlab/summer2020/students/proj-2012153/-/issues/26)

***相关PR***: [#401](https://github.com/manateelazycat/emacs-application-framework/pull/401)

使得浏览器支持在新标签中创建当前页面的副本，使用`M-p`或`P`复制当前页面到新标签中。

## 项目感言

很荣幸这次能够参与Emacs中文社区的EAF项目，参与到EAF项目的建设。

在这次活动之前，我从未使用过Emacs。虽然我之前了解过一些Conjure Lisp，但当时仍然对Emacs Lisp一无所知。之后注意到了Emacs社区的任务，我立刻产生了挑战它们的想法，一方面是我对Python和JavaScript足够熟悉，即使未了解过Emacs，但是仍然充满了信心; 另外一方面是因为我之前投递的OpenCV社区QR码项目是中等难度项目，而在Emacs社区中我有把握能够完成PDF的高难度项目（高难度项目奖金更多XD）。

虽然EAF社区的项目和其它社区的存在许多不同，挑战者需要选择15个指定难度的项目进行挑战，挑战性较高，而且关于Emacs Lisp的相关中文资料较少，我需要使用Google搜索并阅读英文文档，了解相关API用法。但是在我的不懈努力下，我还是最终攻克下了Emacs这个知识盲区，并在一个月内实现了所有15个项目的基本功能。

随后，根据社区的实际需求，我还积极帮助社区额外实现了3个功能，最终进行了48次提交，3402行增，966行减。

最后，我要感谢我的项目导师[@MatthewZMD](https://github.com/MatthewZMD) 和 EAF项目创始人[@manateelazycat](https://github.com/manateelazycat)的指导，是他们让我更多地了解了Emacs以及键盘文化，对我进行了技术和细节上的指导，锻炼了我的团队协作和沟通能力，使我这次的Emacs项目参与收获丰厚。
