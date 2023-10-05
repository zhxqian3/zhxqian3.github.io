---
title: '删除双系统引导程序'
date: 2023-09-09T15:03:34+08:00
draft: false
tags: ["notes"]
authors: ["zhxqian3"]
summary: "删除windows以外的系统引导程序"
---

## 管理员 PowerShell

在开始菜单的windows搜索中输入powershell 。查找带有“ Windows PowerShell ”标签的蓝色图标，右键单击该图标并在上下文菜单中选择“以管理员身份运行” 。

## 将 EFI/系统分区挂载为卷
要在给定的驱动器上挂载 EFI 系统分区，请使用/S参数使用mountvol 命令。您可以选择任何您想要的免费驱动器号。举些例子”。
```
mountvol S: /S
```

## 访问挂载卷
现在分区已挂载。我们可以通过cd 命令和驱动器号 S: 作为参数更改为驱动器来访问已安装的卷
```
cd S:
```

为确保我们位于该卷的根目录，我们应该执行：
```
cd \
```

我们现在可以使用dir 命令列出当前路径下的目录，以确保我们确实在 UFI 分区驱动器上。
```
dir
```

输出应与此类似：
```
Directory: S:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2019-01-17     12:55                EFI
-a----       2018-10-16     10:57             31 startup.nsh
```

## 删除引导加载程序
您的引导加载程序位于EFI目录中。使用cd 命令更改为它并通过dir 命令列出条目。
```
cd .\EFI\
```
```
dir
```

您的输出取决于您安装的引导加载程序，这里是Windows 和 Ubuntu 的示例。
```
Directory: S:\EFI


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2018-12-06     06:55                Ubuntu
d-----       2018-12-05     05:21                Microsoft
d-----       2019-01-17     12:55                Boot
```

现在您可以通过命令 Remove-Item和参数 -Recurse删除不需要的加载程序。

不要删除引导目录或 WINDOWS 目录！

```
Remove-Item -Recurse .\ubuntu
```

[参考来源](<https://askubuntu.com/questions/429610/uninstall-grub-and-use-windows-bootloader>)