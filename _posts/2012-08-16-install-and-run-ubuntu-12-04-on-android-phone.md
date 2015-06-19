---
title: Android 手机上安装并运行 Ubuntu 12.04
author: DawnDIY
layout: post
tags:
  - Android
  - Linux
  - Ubuntu
  - 手机
---

* ubuntu.sh脚本的原地址变动了，导致下载不了，现在更新了网盘地址。小技巧：遇到一些下载失效的时候可以试一试p2p下载工具（如 easyMule、迅雷等）试一试，说不定有人分享过~*  
————————————————————————————-

Android 是基于Linux内核的开源操作系统，主要用在移动设备上。当然同样是基于Linux内核的操作系统，现在支持的Android的智能手机理论来说都能运行基于Linux的操作系统，比如现在流行的发行版：Ubuntu、Fedora 等等。不仅如此，现在的智能移动设备的硬件也越来越强，更为能运行Linux系统提供了良好的硬件支持。今天[DawnDIY][1]就带大家来尝试一下在 Android 手机上安装 Ubuntu 12.04 操作系统。

 [1]: http://www.dawndiy.com

## 一.效果预览

先上图，解个馋~这就是安装后的效果。

![][2]

 [2]: http://i.imgur.com/1O2xX.png "Screenshot_2012-08-16-00-50-44"

![][3]

 [3]: http://i.imgur.com/8uj3J.png "Screenshot_2012-08-15-01-48-46"

![][4]

 [4]: http://i.imgur.com/7PY2e.png "Screenshot_2012-08-16-02-13-20"

![][5]

 [5]: http://i.imgur.com/Za4Rz.jpg "20120815004"

看到上面的图了吧，这就是安装最新的 Ubuntu 12.04 在 Android 智能机上的效果。同时因为 Unity 原生就是支持触屏设备的，所以操作方面还是可以的，只不过就是屏幕小了点而已~好了，下面我来介绍一下我的安装过程。

## 二.配置要求

*   设备需要root权限，并且安装了[BusyBox][6]
*   最小 1GHz 处理器(推荐)
*   Android 系统版本 2.1 或以上
*   Android 设备需要自定义的ROM固件
*   SD卡至2.5GB (安装大映像的需要3.5GB)
*   设备需要支持WIFI (这个用于其他设备通过WIFI登录)
*   支持 Ext2 文件系统(大部分 Android 设备应该都支持)

 [6]: http://zh.wikipedia.org/wiki/Busybox



**我的设备**

*   手机型号：Mi-One Plus
*   处理器主频：1.5GHz * 2
*   SD卡：16G class 4
*   系统ROM：MIUI\_v4\_2.8.10
*   BusyBox版本：1.20.2

## 三.需要的软件

*   Android Terminal Emulator (终端模拟器) ：用于运行 [shell][7] 脚本     [Google Play][8]
*   BusyBox ：用于提供 shell 命令的支持   [Google Play][9]
*   Android VNC Viewer：用于 Android 设备的远程连接工具     [Google Play][10]
*   Ubuntu 12.04  的映像文件：用于安装 Ubuntu 的映像文件    选择下载： [Full][11]、[Small][12]、[Core][13]
*   ubuntu.sh ：Ubuntu 的安装脚本    [点这里下载][14](已失效，或用迅雷)    新地址-> [下载][15]
*   bootscript.sh：Ubuntu 的启动脚本     [点这里下载][16]
*   Linux Installer：Linux 安装向导(这个支持个帮助向导，可以不需要)     [点这里下载][17]

 [7]: http://zh.wikipedia.org/wiki/Shell
 [8]: https://play.google.com/store/apps/details?id=jackpal.androidterm
 [9]: https://play.google.com/store/apps/details?id=stericson.busybox
 [10]: https://play.google.com/store/apps/details?id=android.androidVNC
 [11]: http://sourceforge.net/projects/linuxonandroid/files/Ubuntu/12.04/full/ubuntu1204-v4-full.zip/download
 [12]: http://sourceforge.net/projects/linuxonandroid/files/Ubuntu/12.04/small/ubuntu1204-v4-small.zip/download
 [13]: http://sourceforge.net/projects/linuxonandroid/files/Ubuntu/12.04/core/ubuntu1204-v4-core.zip/download
 [14]: http://sourceforge.net/projects/linuxonandroid/files/Ubuntu/ubuntuV6-1-script.zip
 [15]: http://pan.baidu.com/share/link?shareid=83492&uk=2416019402
 [16]: http://sourceforge.net/projects/linuxonandroid/files/bootscript.sh/download
 [17]: http://sourceforge.net/projects/linuxonandroid/files/App/Complete%20Linux%20Installer%20v2-8.apk/download

## 四.开始安装

首先您的手机需要 [chroot][18]，需要root权限去操作，相当于越狱。不懂的可以去 Google 一下“[Android获取root权限][19]”。root是前提，所以先要把这个做好，不过现在很多ROM都做的很好，比如MIUI就有很好的权限管理。

 [18]: http://zh.wikipedia.org/wiki/Chroot
 [19]: https://www.google.com/search?hl=zh-CN&newwindow=1&client=ubuntu&hs=YgZ&channel=fs&q=Android获取root权限&oq=Android获取root权限&gs_l=serp.12..0.22005.22005.0.22938.1.1.0.0.0.0.210.210.2-1.1.0...0.0...1c.OT_GdyR88Do

### 1.安装文件下载

首先就是下载必要的文件，上面讲到的需要的 Ubuntu 12.04 的映像文件，这个是在 sourceforge.net 上的一个叫 Linux-on-Android 的项目。我上面给的地址中有三个包可供下载：

![][20]

 [20]: http://i.imgur.com/Z71id.png "2012-08-15 16:00:15的屏幕截图"

其实下面就有英文的介绍，我就在这里简单介绍一下：

*   full 映像包含了完整的 Ubuntu 系统，其中包括 Unity 桌面，还有很多如GIMP等常用软件，非常齐全。需要 3.5G 以上空间。
*   small 映像包含了的基本的 Ubuntu 系统，其中包括 LXDE 桌面，需要 2G 以上空间。
*   core 映像包含了基础的 Ubuntu 系统，不过这个没有GUI的，也就是没有桌面只有命令行。

上面下载的就是待安装的 Ubuntu 12.04 的映像文件，然后我们安装还需要安装脚本，也就是上面说的 ubuntu.sh ，还有安装后的启动脚本 bootscript.sh 。有了这些文件后我们在手机的SD卡的根目录，新建一个文件夹取名为 ubuntu ，然后把这里我们刚才下载好的文件放到这个文件夹里面，到这里 ubuntu 文件夹里就分别有 ubuntu.img、ubuntu.sh、bootcript.sh 这三个文件了。

### 2.安装软件

先展示一下我们需要的三个软件，如图：

![][21]

 [21]: http://i.imgur.com/dHA4h.png "Screenshot_2012-08-15-18-45-48"

首先需要的是 Terminal 这个软件，也就是一个终端，通过终端我们可以用来执行很多命令和脚本。上面我给出了Google Play的地址，这个在很多地方都有的下的，还有Android VNC Viewer也可以在 Google Play 里面找到安装。

在这里我要说一下BusyBox，它使得你可以在 Terminal 中运行很多命令，现在很多 Android 的 Rom (我用的MIUI\_v4\_2.8.10也是) 的终端中很多命令都不能运行，比如 cp、mv、cut 等，但是这些都是我们脚本里面需要用到的，如果不能运行这些命令而执行脚本的话，会提示 : **not found** 这样的提示。所以安装 BusyBox 可以使得这些命令都能够在终端里面执行。如果你的Rom本来够强大已经包含了BusyBox的新版本，能够运行基本的shell命令的话，那也可以不用装这个。

当然安装BusyBox以及后面我们在Terminal中都需要 root 权限，如果是MIUI系统的话则可以直接在 **授权管理 > ROOT权限管理** 里面打开该选项，然后需要root权限的时候允许就可以了。其他的系统我没用过，不过可以直接用 **一键ROOT工具** 来操作。

安装BusyBox，安装好后，打开BusyBox点击 **Install** 开始安装，如果弹出需要ROOT权限，点下一步允许就行，如图：

![][22]

 [22]: http://i.imgur.com/SCntt.png "Screenshot_2012-08-15-18-49-06"

### 3.安装 ubuntu 12.04

首先，打开 **终端模拟器(Terminal)** ，在光标处输入 “**cd /sdcard/ubuntu**”(不包括引号，注意cd后有空格)然后回车，这样就来到了刚才我们在SD卡里面新建的目录了，如图：

![][23]

 [23]: http://i.imgur.com/Qlq0j.png "Screenshot_2012-08-16-00-18-55"

然后我们可以输入命令 “**ls**” 然后回车，我们就可以看到当前目录下的所有文件了，看一下里面是不是我们需要的三个文件，如图：

 ![][24]

 [24]: http://i.imgur.com/Q2due.png "Screenshot_2012-08-16-00-19-15"

 接下来我们就要开始运行 ubuntu.sh 这个安装脚本了，但在这之前我们需要使用 root 用户来运行这个脚本，在终端中使用命令 “**su**” 来切换至 root 用户权限，如果弹出授权信息点击下一步允许就行了，或者直接用 一键ROOT 来开启终端重复上面操作，成功后如图之前的“**$**”变成了“**#**”，这就说明已经获得Root权限了，如图：

![][25]

 [25]: http://i.imgur.com/STeo5.png "Screenshot_2012-08-16-00-19-43"

然后运行安装脚本，输入命令 “**sh ubuntu.sh**”，进行安装，如图：

![][26]

 [26]: http://i.imgur.com/9hHAD.png "Screenshot_2012-08-16-00-41-42"

然后脚本为你建立了一个名字为“**ubuntu**”的帐号，这里提示你需要为你的帐号设置一个密码，这个密码会在以后你操作 Ubuntu 的时候一些授权应用到，比如我在这里设置密码为：“**ubuntu**”，这里**注意**的是在终端里面输入密码是不会显示出来的，你看见光标没有动静，但实际上你已经输入进去了。回车后提示再次输入密码以保证你两次密码一样，如图：

![][27]

 [27]: http://i.imgur.com/KAwLW.png "Screenshot_2012-08-16-00-42-01"

密码设置完成后，提示是否启动[VNC][28]服务和[SSH][29]服务，我们只要输入“**y**”然后回车，开启了这两个服务后我们才能通过远程连接来连上系统，如图：

 [28]: http://zh.wikipedia.org/wiki/VNC
 [29]: http://zh.wikipedia.org/wiki/SSH

![][30]

 [30]: http://i.imgur.com/zb3If.png "Screenshot_2012-08-16-00-45-39"

然后提示我们输入设备屏幕的尺寸，我的屏幕是854×480的，所以我输入“**852×480**”（**小米手机注意**：小米手机是854×480的，但是后面用Android VNC 连接的时候有问题，在右边会显示一条线，所以**小米手机用户最好设置成“852×480”**，其他手机没有测试过，在设置的时候请注意！）。**注意：**这里两个数字之间的**不是乘号**，而是**字母“xyz”的“x”**，输错了不能远程连接的，如图：

![][31]

 [31]: http://i.imgur.com/tNQsv.png "Screenshot_2012-08-16-00-46-10"

如图的提示已经启动了一个新的桌面，提示是否保存你刚才的设置为默认设置，只要输入“**y**”即可，如图：

![][32]

 [32]: http://i.imgur.com/j6jFo.png "Screenshot_2012-08-16-00-46-46"

然后你就可以看到操作完成后光标前的字符变成了“**root@localhost:~**# ”，有没有发现。其实到这里你已经进入了 Ubuntu 12.04 系统，已经完成安装配置并启动了 Ubuntu 12.04 系统，不信？你可以输入命令 “**cat /etc/issue.net**” 然后回车查看当前系统是不是Ubuntu 12.04，如图：

![][33]

 [33]: http://i.imgur.com/1421g.png "Screenshot_2012-08-16-00-46-54"

![][34]

 [34]: http://i.imgur.com/B5lNa.png "Screenshot_2012-08-16-00-48-30"

### 4.远程桌面连接

当然，光用命令行当然体验不到什么，我们这时候确实是已经启动了 Ubuntu12.04 ，现在只需要用远程连接工具来连接登录桌面就能看到完整的桌面系统了。这时候我们就要用到前面安装的 **Android VNC Viewer** 了。按手机的 **Home** 键回到手机桌面，保持**终端**还在后台运行。找到 **Android VNC** 并且打开，然后进行一些简单的配置。**Nickname**，为你的连接去一个名字如“**ubuntu**”。**Password** 为 “**ubuntu**” 。**Address**是ip地址，这里我们是在同一台手机上连，所以我们填写“**localhost**”，当然你想在别的设备上连接当前的设备那就要填写启动时提示的地址。**Port** 是段口号，默认**5900**。还有这里比较重要的是 **Color Format**，这个是连接的色彩设置，建议设置成“**24-bit color (4 bpp)**”，要不然画质太低的话画面就惨不忍睹了。如图：

![][35]

 [35]: http://i.imgur.com/foYau.png "Screenshot_2012-08-16-01-38-07"

![][36]

 [36]: http://i.imgur.com/avTxQ.png "Screenshot_2012-08-16-01-38-17"

全部设置好以后，点击 **Connect** 就可以连接上我们本地已经在运行的 Ubuntu 12.04 系统了，如图：

![][2]

使用 **LibreOffice Writer** ，并且支持使用**手机端输入法**：

![][4]

使用 **LibreOffice Calc**：

![][37]

 [37]: http://i.imgur.com/Y2kgH.png "Screenshot_2012-08-16-01-36-07"

使用 **FireFox** 打开 **Google**：

![][38]

 [38]: http://i.imgur.com/KwsUC.png "Screenshot_2012-08-15-12-05-53"

如果想要退出桌面连接，只需要点击手机的 **菜单** 键，然后选择 **disconnect** 就可以断开连接。

### 5.退出 Ubuntu 12.04 系统

退出 Ubuntu 系统，只需要回到刚才我们运行的终端，输入命令 “**exit**” 回车，等待片刻即可退出 Ubuntu 系统，再次输入 “**exit**” 回车 则是退出手机终端的 root 用户权限，然后再次 “**exit**” 回车后则是退出手机终端，这样就完全退出了，如图：

![][39]

 [39]: http://i.imgur.com/9TRbM.png "Screenshot_2012-08-16-01-39-13"

### 6.下次启动

下次启动的时候只需要开启** 终端**，然后输入 “**su**” 获得 root 权限，再输入 “**cd /sdcard/ubuntu**” 来到ubuntu文件夹下，然后在输入 “**sh bootscript.sh**” 运行启动脚本就可以运行启动 Ubuntu 了，需要连接桌面的话按照上面说的用** Android VNC** 就可以了。

## 四.电脑端连接使用手机上的 Ubuntu 12.04

到这里你一定成功在手机上跑起 Ubuntu 12.04 了吧，感觉不一样吧！还没完呢！在这里因为我们在手机上的 Ubuntu 12.04 开启了 VNC 和 SSH 服务，当然在手机连上网（最好是WIFI或局域网）了以后，我们也可以用电脑去远程连接登录到手机上的 Ubuntu 12.04 ，这样的话，我们就可以通过电脑来操作手机上的 Ubuntu 了。

VNC服务是与操作系统无关的，所以不管你电脑是什么系统都可以通过VNC来连接登录到手机上的 Ubuntu。在 Windows 操作系统上可以通过 **VNC 客户端** 来连接。因为我电脑的系统是Linux 所以我在这里只演示 Linux 下连接登录到手机的 Ubuntu。Windows的也大同小异，所以Google一下吧。

在Linux下，使用 **Remmina 远程桌面客户端** 这款工具就可以连接到按照我们上面的方法启动了 Ubuntu 的手机，其中**服务器地址** 就是你在手机上启动 Ubuntu 的时候，提示的 VNC 地址 。配置如图：

![][40]

 [40]: http://i.imgur.com/7qtaz.png "2012-08-16 02:41:46的屏幕截图"

点击连接，就可以连接登录到手机上的 Ubuntu 12.04 了，这样你就可以在电脑上操作手机上的 ubuntu 系统了，如图：

![][41]

 [41]: http://i.imgur.com/GIUac.png "2012-08-16 02:47:29的屏幕截图"

## 五.总结

Android 智能手机 装上了原生的 Ubuntu 12.04 ，这需要感谢 Zachary Powell 团队在 SourceForge 上提供的文件及脚本，不仅是 Ubuntu ，你也可以在 Android 手机上安装 Fedora、openSUSE、Debian、ArchLinux 等系统，这些系统的影响文件及脚本 Zachary Powell 团队在 SourceForge 上都有提供，感兴趣的朋友可以尝试一下！

说到底装上的还是原生的 Ubuntu ，但是还是挺期待 Canonical 专门为 Android 设备定制的 Ubuntu 系统，现在 Canonical 公司也已经在为 Android 设备打造专属的 Ubuntu 系统了，期待它能早日面世。相关信息可以查阅：http://www.ubuntu.com/devices/android

**作者：[DawnDIY][1]**  
**本文原创，如果转载请注明原文出处及原文地址，3Q**

 
