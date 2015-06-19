---
title: Ubuntu 下搭建 Android 开发环境(图文)
author: DawnDIY
layout: post
tags:
  - Android
  - Linux
  - Ubuntu
  - 开发
---

随着智能手机、平板电脑等越来越普及，现在的移动平台开发越来越火，IOS、Android等等，以前一直没有开发过移动平台的应用，然而网上的N多教程全是Windows平台的，而我却坚持这Linux桌面，那么这么新鲜、这么火、这么有前景的开发，我也先起个头，把环境搭建起来先。

## 1.安装JDK

请看这里 > [《Linux 下安装配置 JDK7》][1]

 [1]: /2012/07/31/install-and-configurate-jdk7-in-linux.html "Linux 下安装配置 JDK7"

## 2.安装Eclipse

现在Eclipse已经出4.2版本，并且官方也已经将4.x版作为默认的下载版本了，大家可以自己选择，下面给出4.x和3.7.x的下载链接：

**Eclipse Juno (4.2)：**  
Windows    [32-bit][2]    [64-bit][3]  
Mac    [32-bit][4]    [64-bit][5]  
Linux    [32-bit][6]    [64-bit][7]  


 [2]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-win32.zip
 [3]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-win32-x86_64.zip
 [4]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-macosx-cocoa.tar.gz
 [5]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-macosx-cocoa-x86_64.tar.gz
 [6]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-linux-gtk.tar.gz
 [7]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops4/R-4.2-201206081400/eclipse-SDK-4.2-linux-gtk-x86_64.tar.gz

**Eclipse Indigo (3.7)：**  
Windows    [32-bit][8]    [64-bit][9]  
Mac    [32-bit][10]    [64-bit][11]  
Linux    [32-bit][12]    [64-bit][13]

 [8]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-win32.zip
 [9]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-win32-x86_64.zip
 [10]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-macosx-cocoa.tar.gz
 [11]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-macosx-cocoa-x86_64.tar.gz
 [12]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-linux-gtk.tar.gz
 [13]: http://www.eclipse.org/downloads/download.php?file=/eclipse/downloads/drops/R-3.7.2-201202080800/eclipse-SDK-3.7.2-linux-gtk-x86_64.tar.gz

下载后解压到本地直接都可以使用了（在配置好JDK的前提下）。

## 3.下载安装 Android SDK



先下载好最新的 Android SDK Package。这里我给出下载地址：

Platform

Package

Size

MD5 Checksum

Windows

[android-sdk_r20.0.1-windows.zip][14]

90370975 bytes

5774f536892036f87d3bf6502862cea5

[installer_r20.0.1-windows.exe][15] (Recommended)

70486979 bytes

a8df28a29c7b8598e4c50f363692256d

Mac OS X (intel)

[android-sdk_r20.0.1-macosx.zip][16]

58217336 bytes

cc132d04bc551b23b0c507cf5943df57

Linux (i386)

[android-sdk_r20.0.1-linux.tgz][17]

82607616 bytes

cd7176831087f53e46123dd91551be32

官网下载地址：

 [14]: http://dl.google.com/android/android-sdk_r20.0.1-windows.zip
 [15]: http://dl.google.com/android/installer_r20.0.1-windows.exe
 [16]: http://dl.google.com/android/android-sdk_r20.0.1-macosx.zip
 [17]: http://dl.google.com/android/android-sdk_r20.0.1-linux.tgz

下载好后当然是解压了，解压到您的工作目录，这个目录就是今后使用SDK的目录：

```bash
tar zvxf android-sdk_r20.0.1-linux.tgz
```

解压找到 tools 目录下的 android 后如图：

![][18]

 [18]: http://i.imgur.com/qtV6C.png "2012-07-31 10:08:30的屏幕截图"

 

这个就是 Android SDK Manager，你可以通过这个来配置、管理和下载最新的SDK。

首先我们先通过 Android SDK Manager 来添加平台和包，打开 Android SDK Manager 后勾选你需要的工具和包，这里 Android SDK Manager 会默认为您勾选它所推荐的包，您只需要点击下载安装就可以了。如图：

![][19]

 [19]: http://i.imgur.com/kZRDG.png "2012-07-31 10:11:13的屏幕截图"

## 4.配置 Android SDK 开发调试环境

在这里我们是要配置开发调试环境，以便我们在控制台能够很好的使用 SDK 。如果你只是希望使用 Eclipse 来做 Android 开发的话，这里也可以省略。不过我还是觉得控制台挺好的，虽然一片片的看着头晕，呵呵。

首先配置环境变量，和配置 JDK 一样。运行一下代码来配置环境变量：

```bash
gedit ~/.bashrc
```

在文件的最末端添加下面内容：

```
# Android SDK
export ANDROID_SDK=/home/dawndiy/workspace/android/android-sdk-linux
export PATH=$ANDROID_SDK/platform-tools:$ANDROID_SDK/tools:$PATH
```

当然， “ANDROID_SDK=” 后面的内容当然是你自己的 SDK 所在的目录啦，千万别照搬啊，上面的可是我电脑上的。修改好了以后记得保存，最后运行一下：

```
source ~/.bashrc
```

 

————更新————-

**这里是后来添加上的**

在之后的使用中我发现在控制台使用  adb 命令正常，但是有的时候需要 root 权限的时候我们再使用 sudo adb 的时候居然会提示 找不到 adb 命令。后来我找到了解决方法，这里说明一下：

```
cd /usr/bin
rm -rf adb
sudo ln -s /home/dawndiy/workspace/android/android-sdk-linux/platform-tools/adb
```

这样就可以解决在 sudo 下也可以使用 adb 了，如果 fastboot 也有这样的情况，一样解决！

—————————–

 

## 5.安装 ADT(Android Development Tools) 插件

打开 Eclipse，选择 **Help** > **Install New Software…**.

点击 **Add**，在 **Name** 输入 “ADT Plugin” 作为名字，在 **Location** 输入 “https://dl-ssl.google.com/android/eclipse/”(不要引号)，如图：

![][20]

 [20]: http://i.imgur.com/jPRY9.png "2012-07-31 12:50:46的屏幕截图"

添加好插件地址后，在 **Work with** 中选择刚才添加的插件地址，然后等待一会儿下面就会出现需要安装的插件。选择需要安装的插件后点击安装即可。如图：

![][21]

 [21]: http://i.imgur.com/7Moeo.png "2012-07-31 12:57:59的屏幕截图"

接下来就是等待下载安装，安装后了以后重启Eclipse即安装完成。

## 6.配置 ADT 插件

这里可能重启Eclipse后就会弹出ADT的配置对话框，如果没有弹出的话下面会介绍。

弹出的对话框如图，只需要把前面安装好的 Android SDK 的目录填入 **Location** 中就可以了。

![][22]

 [22]: http://i.imgur.com/tssKQ.png "2012-07-31 13:15:48的屏幕截图"

然后弹出一个问你是否愿意想Google反馈使用信息的对话框，Yes or No 随便，然后 Finish。

**如果没有弹出ADT配置对话框，那么我们如下操作来配置。**

打开 Eclipse ，选择 **Window** > **Preferences…** 来打开选项面板。

在左侧选择 **Android** ，在右侧面板中找到 **SDK Location** 点击 **Browse…** 来选择你前面安装的SDK目录，最后点击 **Apply** 即可。如图：

![][23]

 [23]: http://i.imgur.com/9zpfS.png "2012-07-31 13:27:51的屏幕截图"

这样您的ADT就基本配置完成了。最后为了保证您的插件是最新的，可以选择 **Help** > **Check for Updates** 让Eclipse自动检测需要更新的组件来更新。

## 7.新建 AVD(android vitural device)

开发的时候当然需要一台设备来做测试，Android SDK 的工具中提供了 Android 虚拟设备的功能，能够在本地虚拟一台 Android 设备。在正式开发之前，我们需要配置新建一个 AVD ，当然你可以使用前面安装好的 Android SDK Manager 来新建，这里我们也可以直接在已经配置好了的Eclipse里面进行添加。

选择 **Windows > AVD Manager** 点击 **New** 来新建一台 AVD ，然后在里面配备相应的参数，如图：

![][24]

 [24]: http://i.imgur.com/3ypIG.png "2012-07-31 13:46:19的屏幕截图"

点击 **Create AVD** 完成。

你可以在新建完成后在 AVD Manager 里面运行您刚才新建的虚拟设备，附上几幅图：

![][25]

 [25]: http://i.imgur.com/xpi9z.png "2012-07-31 14:07:46的屏幕截图"

![][26]

 [26]: http://i.imgur.com/Hp9T3.png "2012-07-31 14:11:01的屏幕截图"

## 8.新建 Android 项目

打开 Eclipse ， **File > New > Other…** 选择 Android Application Project 后，就会出现向导对话框，然后更具向导填好相关信息，最后就可以生成一个 Android 项目，如图：

![][27]

 [27]: http://i.imgur.com/ndSpv.png "2012-07-31 16:20:49的屏幕截图"

填写应用名、项目名、包名等信息，还有选择构建的SDK版本。

![][28]

 [28]: http://i.imgur.com/wZGpZ.png "2012-07-31 14:21:57的屏幕截图"

设置应用的图标：

![][29]

 [29]: http://i.imgur.com/HjUCR.png "2012-07-31 14:23:03的屏幕截图"

![][30]

 [30]: http://i.imgur.com/2CK0b.png "2012-07-31 14:25:32的屏幕截图"

完成新建 Android 项目：

![][31]

 [31]: http://i.imgur.com/ISKJf.png "2012-07-31 16:14:08的屏幕截图"

项目建立好后，默认给出的是一个示例，我们直接运行一下看能否运行，点击工具栏的绿色运行按钮或者键盘 Ctrl F11 。运行效果如下：

![][32]

 [32]: http://i.imgur.com/fl8sA.png "2012-07-31 16:13:23的屏幕截图"

这样，我们的环境就配置完成了！

## 9.总结

一直都想去尝试开发一款自己的Android程序，但是一直都没有去学，趁今天下午有时间，参考了官方的文档，自己摸索的搭建了一下开发环境，算一个开头。接下来就慢慢的学习吧～ Over………………….
