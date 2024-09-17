---
title: "从文件资源管理器边栏删除Creative Cloud"
date: 2019-09-29 16:46:00 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

不幸的是，无论您是否计划实际使用文件存储功能，Adobe的Creative Cloud安装程序都会在安装任何Creative Cloud应用程序时将其添加到Windows文件资源管理器侧栏中。 更糟糕的是，目前还没有办法通过文件资源管理器或创意云设置删除该边栏条目。 对于那些不喜欢文件资源管理器的人来说，无用的文件不必要地混乱，

<!--more-->

首先，请务必注意，从文件浏览器侧边栏中删除Creative Cloud文件后，以下步骤实际上并不会删除Creative Cloud Files文件夹本身。 您仍然可以手动访问该文件夹，该文件夹默认位于`C：\ Users \ [User] \ Creative Cloud Files。`

这些步骤也不会禁用实际的Creative Cloud Files存储或同步功能; 为此，您需要启动创意云桌面应用程序，点击齿轮图标，然后导航到首选项>创意云>文件，您可以在其中将“同步”设置为关闭。 最后，我们在本文中的截图是在Windows 10中进行的，但这些步骤同样适用于Windows 8.1。

![img](https://img.akvicor.com/i/2024/09/17/66e9a45a59f82.jpg)

要从文件资源管理器侧栏中删除Creative Cloud文件，您需要修改Windows注册表中的条目。 按桌面上的Windows Key + R启动注册表编辑器，然后在“运行”框中键入regedit。 按键盘上的Enter键启动该实用程序并授权任何用户帐户控制提示。

![img](https://img.akvicor.com/i/2024/09/17/66e9a46793e75.jpg)

我们现在需要找到正确的注册表项，这将根据您的特定Windows配置而有所不同，但将位于HKEY_CLASSES_ROOT \ CLSID中的某个位置。 找到正确位置的最快方法是用Find命令搜索它。 选中注册表编辑器后，按键盘上的Control + F打开“Find”窗口。 在“Find what”框中键入“Creative Cloud Files ”文件，然后取消选中“Keys”和“Values”框。 单击查找下一步继续。

![img](https://img.akvicor.com/i/2024/09/17/66e9a4739f7a1.jpg)

您的第一个结果可能是一个类似于上面截图的条目。 如果您收到不同的结果，请继续按键盘上的F3搜索其他条目，直到您到达一个看起来像示例截图那样的项。

![img](https://img.akvicor.com/i/2024/09/17/66e9a480a1f40.jpg)

我们需要修改System.IsPinnedToNameSpaceTree来从DWORD的File Explorer侧栏中删除Creative Cloud Files。 双击它以编辑其值，并将“Value data”从默认值1设置为0。 单击确定保存更改。

![img](https://img.akvicor.com/i/2024/09/17/66e9a49ab5833.jpg)

现在，退出并重新启动文件资源管理器。您应该看到创意云文件的条目不再存在于侧栏中。如果您仍然看到它，请重新启动计算机，这将确保文件资源管理器完全关闭并重新加载，以使更改生效。

![img](https://img.akvicor.com/i/2024/09/17/66e9a4a9732d7.jpg)

如上所述，您仍然可以通过手动导航到主用户文件夹中的文件夹来使用创意云文件同步; 这里的步骤只从文件浏览器侧边栏中删除其快捷方式。按照相同的方式，如果您的意图是完全杀死Creative Cloud文件同步，则还需要在Creative Cloud首选项中关闭该功能。 

如果您想要在文件资源管理器中恢复Creative Cloud Files侧栏条目，只需重复上述步骤即可在注册表中找到正确的条目，将System.IsPinnedToNameSpaceTree更改  回“1”，然后重新启动文件浏览器或重新启动PC。

