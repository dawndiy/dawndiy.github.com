---
title: Linux 下使用 fastboot 给手机刷机
author: DawnDIY
layout: post
permalink: /archives/389
categories:
  - Android
  - Linux
tags:
  - Android
  - fastboot
  - Linux
  - 刷机
---

近期一直都没电脑用，一直在用手机。今天看看手机的东西好杂好乱，想好好理理啦，所以准备刷下机。之前都是下好卡刷包然后卡刷的，想到之前我在电脑上配好了 Android 的开发环境，可以尝试一下在 Ubuntu 下线刷。应该很多人都是在 Windows 下线刷的吧，其实在 Linux 下一样非常简单，尤其是现在一些包还写好了脚本。

在 Linux 下首先应该准备一下环境，也就是 Android 开发调试环境。可以参考我之前的文章：[《Ubuntu 下搭建 Android 开发环境(图文)》][1] 这里其实你只是想刷机的话，看完前4点就行了。老早就配置好了环境的朋友就跳过咯~

 [1]: http://www.dawndiy.com/archives/153 "Ubuntu 下搭建 Android 开发环境(图文)"

接下来我们准备好你手机的线刷包，注意里面一定是解压后包含 images 文件夹的包，其余的就是一些脚本。

剩下的就是我们开始连接手机咯。**注意：**要确定你的手机 **设置 > 开发人员选项 > USB 调试** 勾选上了。好的，接下来使用USB数据线连接你的手机到电脑。运行下面代码：

    lsusb

这时候你会看到下面类似的结果：



    Bus 001 Device 002: ID 064e:a111 Suyin Corp. 
    Bus 001 Device 003: ID 0bda:8189 Realtek Semiconductor Corp. RTL8187B Wireless 802.11g 54Mbps Network Adapter
    Bus 002 Device 003: ID 18d1:9025 Google Inc. 
    Bus 005 Device 002: ID 046d:c52b Logitech, Inc. Unifying Receiver
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
    Bus 007 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

其中 Google Inc 这一行现实的是我的设备。你可以更具自己的手机厂商来判断那个是你的设备，在这里我们需要记录下的是 ID  18d1:9025 这一串。接下来运行下面代码：

    sudo gedit /etc/udev/rules.d/android.rules

在文件中添加如下信息：

    SUBSYSTEMS=="usb", ATTRS{idVendor}="18d1", ATTRS{idProduct}="9025", MODE="0666"

注意上面的数字和前面我们获得的那一串的对应，而最后的 MODE 是不变的。好的，修改保存好以后，给这个文件添加读和执行权限：

    sudo chmod  rx /etc/udev/rules.d/android.rules

接着我们就能够连接手机了，运行下来代码：

    sudo adb devices

然后我们会看到如下信息，成功连接手机的话会显示 device 信息：

    * daemon not running. starting it now on port 5037 *
    * daemon started successfully *
    List of devices attached 
    bc762e7c	device

如果你显示 ？？？？？ 或者 没权限，可能是前面的步骤没做好，或是环境变量没有配好。而且，这里 adb 记得用 root 权限来运行，即 sudo。

好了，连接了，我们可以试一试，如果你动 shell 的话，你可以运行 sudo adb shell ，这样就直接在电脑上运行你 Android 手机上的 shell 命令了。

好了，回到正题，我们是要刷机，而且是线刷。首先线刷不是在开机状态下执行的，所以运行下面代码使得你的手机进入 fastboot 模式 准备开始线刷：

    sudo adb reboot-bootloader

稍等片刻手机就会重启至 Fastboot 模式，等待刷机的开始。如果你早就知道怎么进入你手机的 Fastboot 模式，你也可以省去上面的步骤直接进入 Fastboot 模式，进行下面的步骤：

    sudo fastboot devices

如果能够现实如下信息，表示你的电脑此时能够连接到手机的 Fastboot 模式。

    bc762e7c fastboot

这时候要通过 cd 命令来到你的线刷包的目录，一般线刷包里面会有 fash_all.sh这个刷机脚本，这是我们可能需要修改一下它的可执行权限：

    sudo chmod  x flash_all.sh

修改好权限以后，我们执行它就行了：

    sudo ./flash_all.sh

接下来我们看到的就是正在刷机了，等几分钟就OK了~下面是我的刷机显示时长：

    sending 'tz' (102 KB)...
    OKAY [ 0.010s]
    writing 'tz'...
    OKAY [ 0.209s]
    finished. total time: 0.219s
    sending 'sbl2' (106 KB)...
    OKAY [ 0.009s]
    writing 'sbl2'...
    OKAY [ 0.226s]
    finished. total time: 0.235s
    sending 'rpm' (112 KB)...
    OKAY [ 0.011s]
    writing 'rpm'...
    OKAY [ 0.238s]
    finished. total time: 0.248s
    sending 'sbl3' (596 KB)...
    OKAY [ 0.046s]
    writing 'sbl3'...
    OKAY [ 0.227s]
    finished. total time: 0.274s
    sending 'sbl1' (82 KB)...
    OKAY [ 0.009s]
    writing 'sbl1'...
    OKAY [ 0.050s]
    finished. total time: 0.059s
    sending 'aboot' (575 KB)...
    OKAY [ 0.044s]
    writing 'aboot'...
    OKAY [ 0.401s]
    finished. total time: 0.445s
    erasing 'boot'...
    OKAY [ 0.009s]
    finished. total time: 0.009s
    sending 'misc' (8 KB)...
    OKAY [ 0.003s]
    writing 'misc'...
    OKAY [ 0.008s]
    finished. total time: 0.011s
    sending 'modem' (28780 KB)...
    OKAY [ 2.160s]
    writing 'modem'...
    OKAY [ 6.352s]
    finished. total time: 8.513s
    sending 'cache' (1024 KB)...
    OKAY [ 0.083s]
    writing 'cache'...
    OKAY [ 0.352s]
    finished. total time: 0.435s
    sending 'system' (219136 KB)...
    OKAY [ 16.157s]
    writing 'system'...
    OKAY [ 35.635s]
    finished. total time: 51.793s
    sending 'system1' (219136 KB)...
    OKAY [ 15.927s]
    writing 'system1'...
    OKAY [ 35.692s]
    finished. total time: 51.619s
    sending 'recovery' (5592 KB)...
    OKAY [ 0.405s]
    writing 'recovery'...
    OKAY [ 0.785s]
    finished. total time: 1.190s
    sending 'userdata' (204800 KB)...
    OKAY [ 14.776s]
    writing 'userdata'...
    OKAY [ 31.363s]
    finished. total time: 46.139s
    sending 'boot1' (4430 KB)...
    OKAY [ 0.328s]
    writing 'boot1'...
    OKAY [ 0.733s]
    finished. total time: 1.061s
    sending 'boot' (4430 KB)...
    OKAY [ 0.320s]
    writing 'boot'...
    OKAY [ 1.028s]
    finished. total time: 1.349s

OK，最后用下面代码重启一下手机你就能看到系统刷机成功了~ ^.^

    sudo fastboot reboot

附上我刷机的脚本：

    fastboot flash tz images/tz.mbn
    fastboot flash sbl2 images/sbl2.mbn
    fastboot flash rpm images/rpm.mbn
    fastboot flash sbl3 images/sbl3.mbn
    fastboot flash sbl1 images/sbl1.mbn
    fastboot flash aboot images/emmc_appsboot.mbn
    fastboot erase boot
    fastboot flash misc images/misc.img
    fastboot flash modem images/NON-HLOS.bin
    fastboot flash cache images/cache.img.ext4
    fastboot flash system images/system.img.ext4
    fastboot flash system1 images/system.img.ext4
    fastboot flash recovery images/recovery.img
    fastboot flash userdata images/userdata.img.ext4
    fastboot flash boot1 images/boot.img
    fastboot flash boot images/boot.img

不过一般线刷包里面都会有脚本的，呵呵~ 也祝你刷机成功！！！

over….

 

 
