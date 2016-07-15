---
title: 在中国体验 Pokémon GO for Android
author: DawnDIY
date: 2016-07-15 14:21:00 +0800
layout: post
tags:
  - Android
  - Pokémon GO
---

比卡丘～～～～


精灵宝可梦(口袋妖怪、宠物小精灵的官方译名，英语：Pokémon GO) —— 一代人曾经的童年。如今推出了一款移动平台的 AR(Augmented Reality) 游戏，由任天堂、口袋妖怪公司授权，Niantic,Inc.负责开发运营。这游戏一经推出就火到不行啊，刚推出的时候在国外大街小巷都能找到几个人举着手机扔出精灵球来抓捕野生的小精灵。是不是听起来很有趣？但不幸的是由于游戏玩家太多，运营商的游戏服务器没有完全准备好，所以他们采取了按地理位置锁区的办法来限制玩家，其中中国的一大片区域就属于锁区范围内。见下图，方框内的地区属于锁区内。


![Locked Area](http://i.imgur.com/B1O2zfd.jpg)


所以地图上在新疆北部、黑龙江和吉林等地是可以玩 Pokémon GO 的。


但如果有想要在锁区内提前体验 Pokémon GO 的 Android 玩家，下面介绍一种方法让你可以足不出户就可以玩上 Pokémon GO。

如果我们能欺骗 Pokémon GO，告诉 Pokémon GO 一个假的锁区外的地址，那么我们就能定位到锁区外的地点来玩 Pokémon GO 了。这就是基础的想法，我们继续。
Android 的开发者选项中有一项 **选择模拟位置信息应用** 的选项，可以选择能够模拟 GPS 位置信息的应用来为系统提供虚拟的 GPS 信号，有了这个功能我们就能实现用假的 GPS 位置信息来欺骗 Pokémon GO 了。但是**选择模拟位置信息应用**这个选项，在程序开发的时候，是可以通过系统服务接口得知是否打开这个选项了。Pokémon GO 为了防止作弊，在程序内部检测了这个选项是否打开，如果你就是简单的使用了这个选项来指定一个可以模拟 GPS 信号的应用来模拟 GPS 信号，那么 Pokémon GO 检测后，虽然位置定位过去了，但是会一直显示 “failed to detect location”，并且你在地图上是发现不了任何东西的。

那么现在的问题就是，能不能即用模拟 GPS 信号的应用，而又不让 Pokémon GO 检测到**选择模拟位置信息应用**选项被使用了？思路就是这样, 但这样不能靠简单的应用就能搞定，这就涉及到修改系统层面的东西了，那么使用 Xposed 这个神器是能办到的。


**目前这个方法是有前提的**:

- 你的能解锁安装第三方 recovery
- 你的 Android 手机得有 root 权限
- 你的手机能运行 Xposed


其实你手机如果满足这几个条件的话，你基本上可以在你手机上干任何事情。

其中 recovery 和 root 权限都是用来安装 Xposed 的，[Xposed](http://repo.xposed.info/) 是一个很强大的工具或者称之为框架，希望进一步了解请 Google 之。


**然后是玩游戏需要的应用**:

- Netfits云墙: [下载](https://netfits.net/?185217850)
- Lockito: [下载](https://play.google.com/store/apps/details?id=fr.dvilleneuve.lockito)
- Pokémon GO: [下载](https://play.google.com/store/apps/details?id=com.nianticlabs.pokemongo)

如图：

![Imgur](http://i.imgur.com/f3wQY5Ul.png)

  
**Netfits云墙** 是用于游戏加速，尤其是国外游戏你了解的。**Lockito** 是用来虚拟GPS位置信息的。


## Root 你的手机

每个型号的手机都不同，所以请大家去各大论坛搜索你手机型号对应的 Root 方法。

提供一个比较常用的:  [CF-Auto-Root](https://autoroot.chainfire.eu/)

里面提供一些手机型号，大家可以前往查看。


## 安装第三方 Recovery

这个也是一样的，每个 Android 手机可能不同，需要自己去网上搜索自己手机型号可用的 Recovery，推荐去搜索 TeamWin 的。

TeamWin - TWRP 官网: [https://twrp.me/](https://twrp.me/)

里面提供一些手机型号，大家可以前往查看。


## 安装 Xposed

### Android 4 及以下版本

来到 Xposed 的[官网安装页面](http://repo.xposed.info/module/de.robv.android.xposed.installer)，如果你的手机是 Android 5.0 以下的系统请直接用官网提供的 [下载地址](http://dl-xda.xposed.info/modules/de.robv.android.xposed.installer_v33_36570c.apk) 链接下载。

### Android 5 及以上版本

如果是 Android 5.0 (包含)以上的手机，可以按照 [这里](http://forum.xda-developers.com/showthread.php?t=3034811) 的说明进行安装。

#### 大致流程是：

先从链接的帖子附件中下载 Xposed 安装程序，[XposedInstaller_3.0-alpha4.apk](http://forum.xda-developers.com/attachment.php?attachmentid=3383776&d=1435601440) 安装到你的手机中

在 [http://dl-xda.xposed.info/framework/](http://dl-xda.xposed.info/framework/) 下载对应版本，通过 recovery 来安装。下载目录里 SDK21 是 Android 5.0 (Lollipop), SDK22 是 Android 5.1 (Lollipop), SDK23 是 Android 6.0 (Marshmallow). 同时注意自己手机的 cpu 架构，一般是 arm 。

如何通过 recovery 安装？使用前面步骤中已经安装好的 recovery。进入 recovery，通常是关机状态下按 **电源键** 和 **音量减** 等待3到4秒后即可进入。如图：

TWRP 界面

![Imgur](http://i.imgur.com/uUPsXHul.png)

选择 Install 来安装你从 http://dl-xda.xposed.info/framework/ 下载下来的 zip 文件，这里我的设备对应的 zip 文件是： xposed-v86-sdk23-arm.zip 。再次提醒一下一定要是自己手对应的，要不然会安装失败。

![Imgur](http://i.imgur.com/Ef9xY0Wl.png)
![Imgur](http://i.imgur.com/q8sWTJYl.png)

确认好了以后滑动底部的滑块来确认安装。

安装完成后选择 Reboot System 正常启动系统，这个时候启动系统会和升级系统一样，看前桌面前会有一系列的优化应用中，可能需要等待一段时间。

![Imgur](http://i.imgur.com/k37jIVal.png)

进入了系统后，找到最开始下载的 Xposed 的 apk 安装包，进行安装。看装后看到图标：

![Imgur](http://i.imgur.com/XUfdwR4m.png)
![Imgur](http://i.imgur.com/WIVfjxBm.png)

到这里 Xposed 算安装完成了，但我们还需要一个模块。这个模块叫 “Mock Mock Locations”，这个模块的作用就是前面思路里的，让应用检测不到使用了开发者选项里的**选择模拟位置信息应用**功能。

点击 Xposed installer 应用里面的 **下载**，搜索 “Mock Mock Locations” 然后下载安装最新版本。

![Imgur](http://i.imgur.com/p4q7Zbrm.png)
![Imgur](http://i.imgur.com/7wVkAw6m.png)

安装完成后在 **模块** 中点击勾选上这个模块就能启用了，如果提示需要重启手机，这时候重启手机即可。

![Imgur](http://i.imgur.com/ad28DXal.png)

就这样，Xposed 的安装配置就完成了。

## 安装 云墙、Lockito、Pokémon GO

这几个都能在 Google Play 里面找到，或者到各自的官方网站进行下载。

云墙使用来游戏加速的，比如你玩 Ingress 和 Pokémon GO 都能用上它，而且速度非常快。

Lockito 是用来模拟 GPS 位置信息的，而且还可以自动模拟 GPS 动态路径哦，说白了就是让你坐着也能跑。

![Imgur](http://i.imgur.com/xZ0BUOCm.png)
![Imgur](http://i.imgur.com/LKMtHmsm.png)

## 开启

所有的流程都准备好以后，先开启 **云墙** 并连接，确保加速已经启动。在设置中的**位置信息**里把模式设置成仅设备。来到开发者选项中设置**选择模拟位置信息应用**为Lockito(有了 Mock Mock Location 这个模块 Pokémon GO 就不知道这个功能被开启了)，然后打开 **Lockito** 定位一个可以玩的地方，比如哈尔滨。并且记得点击开启模拟按钮哦！

最后打开 Pokémon GO 你就可以愉快的玩耍了。

如下图，Pokémon GO 里面显示的就是上面截图定位的地址。

![Imgur](http://i.imgur.com/Cma3qp1l.png)
![Imgur](http://i.imgur.com/RHhyW3el.png)

# 最后

这个方法还是比较复杂的，仅限于想要体验的用户，作弊不好，还是等官方正真支持中国地区吧！！！
