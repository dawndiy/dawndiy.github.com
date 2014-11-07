---
title: Linux 下的 Chrome 中 Flash 中文显示方框解决办法
author: DawnDIY
layout: post
permalink: /archives/415
categories:
  - Linux
  - 解决Linux疑难杂症
tags:
  - Chrome
  - Flash
  - Linux
---

先强烈吐嘈一下：以后大家尽量用 Html5 来淘汰 Adobe Flash 吧，一直以来各种系统漏洞很多情况都是来自 Flash ，而且 Flash 也有着各种局限性，虽然现在 Html5 还不成熟，但它正走向成熟。尤其在 Linux 桌面系统中，Flash 也不进行更新了(只进行安全更新)，同时“号称”跨平台的 Adobe AIR 也不开发 Linux 版本了。我们只能用旧的…

对于 Linux 用户来说，Adobe 已经不更新 Flash 了，最后一个版本是 11.2 。除非你使用 Google 的 Chrome 浏览器，Chrome 中内置了最新版本的 Flash ，最新版本是 11.5 。也就是说以后有人想在 Linux 下用最新的 Flash 那么就只能用 Chrome 了，不过我觉得这没必要， Flash 迟早也是会淘汰的，只是时间问题，就和 IE6 一样。但现在在国内的网络状况中，你真的不能不没有 Flash ~ 汗~~ 比如在线看个视频、听个音乐、等等，都需要 Flash 的支持。

而对于 Linux 下的 Flash 来说，貌似它对本地中文字库支持的不够好，或者换句话说，目前很多 Linux 发行版中的中文字库不够全。这样以来有一些包含 Flash 的网站，中文字只能变身成为 “口口口”了！

好了，前面说了一些废话，但这些都是一直我想说的。。。**下面是正题：**

**问题来源：**

之前我都是主用 Firefox ，我的系统是已经安装了 Flash 的，即 11.2 版本。而且因为我用的是 Ubuntu ，系统中已经包含文泉驿、方正等中文字体。所以在 Firefox 中使用 Flash 是可以正常显示中文字的。今天，我安装了 Chrome ，接着刚好在 Chrome 中打开了 [三国杀Online][1] ，接着就开杀啦。。呵呵，玩着正欢的时候突然发现聊天的框中，中文字全变成“口口口”了，如图：

 [1]: http://web.sanguosha.com "三国杀Online"

![][2]

 [2]: http://i.imgur.com/f96FE.png "sanguosha_1"

看到上面的图你什么感觉？唉~可能别人骂你你都不知道呢~

**问题原因：**



出现这样的情况，无非就是 Flash 找不到对应的字体，可能是你系统中没有安装这种字体，或者就是你安装了这种字体，Flash 没办法调用它。Linux 相关字体的配置，在这个地方 /etc/fonts/conf.d ，但是不建议修改，因为现在的情况不是配置的原因使得出现“口口口”的。因为此时，我的 Firefox 中能够正常显示 Flash 中的中文字，而 Chrome 中不能正常显示。到这里，原因很简单了，是 Chrome 中内置的 Flash 的问题。PS：我在网上搜索，有人说是 Chrome 内置的 Flash 没有修改字体的接口。

**解决办法：**

其实问题就是这样，知道原因了就好解决了。是 Chrome 内置 Flash 出现的问题，那么我们就不用它内置的就行咯~我们系统中本身自己就安装好了 11.2 版本的 Flash ，那么我们只要使 Chrome 能够调用系统中安装的 Flash 就行了。

步骤如下：

1.打开 Chrome ，在地址栏输入  进入插件页面，把详细信息展开 。

2.找到 Adobe Flash Player  (2 files)，你此时会看到后面有个 (2 files)，这就对了，一个是内置的，一个是系统里安装的。从两个文件的路径也能看出来，一个是： /opt/google/chrome/PepperFlash/libpepflashplayer.so  ，即 Chrome 内置的；还一个是： /usr/lib/flashplugin-installer/libflashplayer.so  ，即我们系统里安装的。

3.Chrome 里在有两个 Flash 的时候是优先使用内置的，所以这里只要 **停用** 内置的 Flash 就可以了~

停用后你在打开 Flash 就能看到正常的中文字了，点击鼠标右键，你也会发现不是之前的 11.5 版本了，而是系统的 11.2 版本了~

OK~问题解决，留下此文~

 

 

 
