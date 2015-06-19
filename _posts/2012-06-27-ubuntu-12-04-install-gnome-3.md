---
title: Ubuntu 12.04 安装 Gnome 3 桌面
author: DawnDIY
layout: post
tags:
  - Gnome
  - Linux
  - Ubuntu
  - Untiy
---

Ubuntu，这个Linux发行版就不用我再介绍了，目前的Ubuntu默认使用的是Unity桌面，Unity项目发起于2010年，第一次应用在Ubuntu 11.04 上面，起初这个桌面颇受争议，但是时隔一年，现在已经非常稳定了。但是萝卜青菜各有所爱，对于热衷于Gnome的用户来说Ubuntu就不能使用Gnome吗？当然，Linux的世界里永远没有不能，而且说到底Unity也是基于Gnome的。现在Gnome3的时代，Gnome桌面也有了不一样的用户体验。


下面DawnDIY来教大家如何安装Gnome桌面：

首先说明的是，Ubuntu从11.10版本就已经在默认的软件源里面添加了Gnome3，这样一来使得我们安装变得非常方便了，直接apt-get。

    sudo apt-get install gnome-shell

如图：  
![][1]
 [1]: http://i.imgur.com/0mpS3.png "安装Gnome3"
 

然后看到的就是安装的提示信息，选择Y进行安装。  
如图：  
![][2]
 [2]: http://i.imgur.com/0sdvH.png "安装Gnome3提示信息"

等待安装完成以后，可以输入

```bash
gnome-shell--version
```

**来察看你所安装的版本，如图：**  
![][3]

 [3]: http://i.imgur.com/tCRb1.png "安装Gnome3版本察看"

随后你注销系统，重新登入选择Gnome3桌面就可以看到Gnome3桌面了，为了配合一下Gnome3，我换了一张经典的Gnome壁纸，如果：  
![][4]

 [4]: http://i.imgur.com/kECFx.jpg "Gnome3桌面1"

![][5]

 [5]: http://i.imgur.com/eEruV.jpg "Gnome3桌面2"

![][6]

 [6]: http://i.imgur.com/J9WJo.png "Gnome3桌面3"

到这里，Gnome3就算安装完了，但是你会发现你一时也不习惯，因为Gnome默认窗口只有关闭按钮，连最大化最小化都没有了。确实不方便，那么我们需要对Gnome3进行配置。没有一款很好的原生的桌面配置工具是各大桌面的通病，所以我们需要如Ubuntu-Tweak 之类的第三方软件来帮我们配置，当然你可以直接到Ubuntu-Tweak的官方网站去下载（[http://ubuntu-tweak.com/][7]）。这里的话，我们介绍一个Gnome3原生的一款简单的配置工具—- Gnome-Tweak-Tool。我们可以使用下面的命令直接安装。

 [7]: http://ubuntu-tweak.com/ "ubuntu-tweak"

```bash
sudo apt-get install gnome-tweak-tool
```

如图：  
![][8]

 [8]: http://i.imgur.com/hfuls.png "安装Gnome-tweak-tool"

安装完成后在菜单中找到 Advanced Settings（高级设置）如图：  
![][9]

 [9]: http://i.imgur.com/0EbN3.png "gnome-tweak-tool"

然后你就可以在里面设置一些基本功能了，例如在shell里面设置titlebar，显示**最大最小化按钮**，如图：  
![][10]

 [10]: http://i.imgur.com/H045V.png "gnome-tweak-tool设置"

这样设置，**最大最小化按钮**就出现了！之后你可以自己发现更多的设置！

最后，可能还有很多人问Gnome3的**关机按钮**到哪里去了？只能找到**注销按钮**是吧？找到注销按钮，然后再按 **ALT** 键，**关机按钮** 就出现了。
