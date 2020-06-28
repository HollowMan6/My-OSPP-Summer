# 结项报告
蒋嵩林，openEuler社区

项目地址：https://gitlab.summer-ospp.ac.cn/summer2021/210010031

## 项目信息
### 项目名称

开发iso刻录DDE桌面应用

### 方案描述

日常工作中，需要把iso镜像刻录至u盘或光盘上。编写一个类似UltraISO软碟通的openEuler DDE桌面应用。可以完成iso刻录至u盘。

[目标操作流程](https://jingyan.baidu.com/article/f0e83a258b07d322e4910157.html)和界面：

**软碟通ISO刻录部分**
![软碟通ISO刻录部分](https://images.gitee.com/uploads/images/2021/0706/105739_cc8f0423_7637131.png "屏幕截图.png")

使用C++进行程序的编写，QT5框架进行软件前端的编写，可在Linux中运行（特别针对openEuler 20.03 SP1下Deepin桌面）。

#### 安装教程

```bash
qmake-qt5 ImageWriter.pro
sudo make install
```

#### 使用说明

1. 安装完成后，在`启动器`中点击`镜像文件刻录器`即可运行程序。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step1.png)
2. 根据提示输入root密码，点击`认证`。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step2.png)
3. 认证成功后即进入软件主界面。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step3.png)
4. 插入U盘，过一会儿即会自动显示出当前系统中新插入U盘，你可以在已插入的U盘中进行选择需要操作的U盘。你也可以手动点击刷新图标进行更新。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step4.png)
5. 然后点击打开文件图标，在对话框中选择源镜像文件，支持选择`iso`，`bin`，`img`格式的磁盘镜像。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step5.png)
6. 随后可以点击`写入`按钮，在进一步确认后，写入将开始，显示进度条和`取消`按钮，如果用户决定`取消`，则停止操作。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step6.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step6-1.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step6-2.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step6-3.png)
7. 要验证写入的U盘的完整性，请点击`验证`按钮，将会在屏幕顶部显示通知提示结果。这将比较磁盘镜像文件和U盘的数据散列号，如果镜像比较大，可能需要等待一段时间。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step7.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step7-1.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step7-2.png)
8. 如果要还原U盘设备，请点击`清除`按钮。它将从磁盘中删除分区数据，并且您将能够将其格式化为FAT32以达到最大容量。
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step8.png)
![](https://gitee.com/openeuler-competition/summer2021-15/raw/master/img/step8-1.png)

#### 测试

使用身边的U盘设备，选取各大发行版安装镜像，都可以正常进行写入。U盘都可以正常引导安装系统。

测试中U盘设备点击`清除`后都可以将其格式化为FAT32以达到最大容量。

点击`验证`按钮，将镜像和写入好的U盘对应起来可以提示验证成功，不对应则会正常提示验证失败。

其它交互过程中的测试都一切正常，无异常之处。

### 时间规划

- 第1周 (7.1 - 5, 2021)
1. 进一步了解Ventory，查阅相关资料。

- 第 2-6周 (7.6 - 8.9, 2021) 共计5周
1. 仿照UltraISO，使用QTDesigner初步设计好图形界面。
2. 进一步详细学习Ventory的原理。
3. 如果在此期间时间有剩余，可以帮助社区解决一些其它Issue，贡献社区。

- 第7周 (8.10 - 15, 2021)
1. 准备中期报告。
2. 为可能的拖延预留的赶进度时间。

- 中期报告 (8.15, 2021)
1. 使用QTDesigner初步设计好的图形界面。
2. 帮助社区解决一些其它Issue，贡献社区。

- 第8-13周 (8.17 - 9.24, 2021) 共计5周半
1. 进行图形界面的逻辑编写，实现刻录ISO功能。

- 第13-14周 (9.25 – 9.30, 2021)
1. 准备结项报告。
2. 为可能的拖延预留的赶进度时间。

- 结项(9.30, 2021)
1. 类似UltraISO软碟通的openEuler DDE桌面应用。可以完成iso刻录至u盘。

- 未来 (10.1, 2021 -)
1. 在成功完成此暑期2021项目后，我将继续为openEuler做出贡献并尽我所能继续维护。


## 项目总结
### 项目产出

1. 可在openEuler 20.03 SP1中Deepin桌面运行的镜像刻录软件，具备将可启动iso镜像刻录至u盘的功能，另外支持U盘存储空间还原和刻录校验功能。同时支持中文和英文界面，国际化支持。已完成，[项目成果提交仓库](https://gitee.com/openeuler-competition/summer2021-15)

2. 将其打包成了可直接在openEuler上安装的rpm安装包。已完成，[项目成果spec文件](https://gitee.com/openeuler-competition/summer2021-15/blob/master/ImageWriter.spec)。

### 方案进度

- 开发阶段第1周-第2周（2021/07/01-2021/07/15）0%
1. 学习ISO刻录实现原理。

- 开发阶段第3周-第4周（2021/07/16-2021/07/31）20%
1. 学习了ISO刻录实现原理（QT下可以使用QIODevice类调用设备，然后使用QFile进行镜像的写入操作）。
2. 完成了UI界面的初步设计。
3. 使用libudev库初步完成了识别U盘设备的功能。

- 开发阶段第5周-第6周（2021/08/01-2021/08/15）40%
1. 进行镜像写入功能的实现。
2. 写入镜像验证shell脚本的编写。

- 开发阶段第7周-第8周（2021/08/16-2021/08/31）60%
1. 初步完成UI逻辑的编写。
2. 完成中文支持。

- 开发阶段第9周-第10周（2021/09/01-2021/09/15）80%
1. 完成rpm打包，提交spec文件。
2. 编写程序使用教程，完善相关文档。
3. 对程序进行测试。

- 开发阶段第11周-第12周（2021/09/16-2021/09/30）100%
1. 对程序进行进一步测试。
2. 准备结项。

### 遇到的问题及解决方案

1. 识别U盘设备直接调用Linux提供的接口`libudev`。=> [usbdevice.cpp](https://gitee.com/openeuler-competition/summer2021-15/blob/master/usbdevice.cpp)

2. 写入镜像使用QIODevice类，然后使用QFile进行镜像的写入操作。=> [imagewriter.cpp](https://gitee.com/openeuler-competition/summer2021-15/blob/master/imagewriter.cpp)

3. 清除使用`mkfs`直接调用命令进行格式化的形式来实现。=> [mainwindow.cpp](https://gitee.com/openeuler-competition/summer2021-15/blob/master/mainwindow.cpp)

4. 验证则直接使用shell脚本进行实现，通过UI调用。=> [verifyimagewriter](https://gitee.com/openeuler-competition/summer2021-15/blob/master/verifyimagewriter)

### 项目完成质量

项目总体来说完全实现了原有计划，完成情况较好。

### 与导师沟通及反馈情况

每双周定时撰写[暑期2021项目任务进展双周汇报](https://gitee.com/openeuler-competition/summer2021-15/issues/I3Z9UM#note_6681866)，导师都能及时给予积极反馈。
