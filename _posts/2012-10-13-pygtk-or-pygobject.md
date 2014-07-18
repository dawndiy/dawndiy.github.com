---
title: PyGTK or PyGObject
author: DawnDIY
layout: post
permalink: /archives/373
categories:
  - 个人日记
tags:
  - Java-Gnome
  - PyGObject
  - PyGtk
---
# 

本来是写应该开始用PyGObject来代替PyGTK的，结果扯了很多自己的经历，呵呵，我的博客不能只有技术文章啊，也要点扯淡的东西！  
——————————–

最初是在Linux的GNOME桌面下认识了GTK ，后来为了能在Linux桌面下开发桌面应用就一直在学习GTK ，起初我只会C/C 和Java。经过一番查阅对于GTK 的开发还是C的文献比较多，那就看吧、学吧。。。后来看的没心思啦，可能本来我C/C 用的就比较少，好多库不熟悉，后来就没看了。想在Linux桌面开发的愿望也就搁置在一旁啦。

但是后来我发现了 Java-Gnome ，也就是GTK 的Java绑定版本，不过一样中文文献资料非常少，那就英文的吧，又不是啃啃看不懂。刚好那时候Java天天在手上用，非常习惯，在加上之前看过的GTK 的一些教程文章，我又燃起了当初开发桌面应用的热情，刚好那时候还自己学了一项新技术，就是面向对象的数据库–db4o，然后一起结合起来完成了我的第一个正式的Linux的桌面应用。



在Linux下开发当然想做跨平台嘛，不过悲剧来了，我这才发现使用 Java-Gnome 开发的程序不能直接在 Windows 平台上使用。后来的后来越来越发现 Java-Gnome 的一些缺点。首先开源软件的通病–更新缓慢，官方现在 Java-Gnome 的最新版本是 4.1.2 。而且按照官方的说法要到 4.2 才算正式版。而且现在已经 GTK 3 了，Java-Gnome 还只是支持GTK 2 。我也一直没有看到有什么要更新的动静。所以我之前还在抱怨我用Glade3做的UI文件用Java-Gnome解析不了，非要用Glade2来做UI才能用。这一点也证实了 Java-Gnome不支持GTK 3 。所以我的桌面应用又搁浅啦~

在大二的时候，老师出了一道有意思的ACM题目给我们做，当时记得我用C 写了几十行的代码才解决，结果老师介绍有一个叫Python的语言，5行搞定，我当时眼前就一亮，马上去找资料学习，发现 Python 真是方便，而且是解释型的语言，调试起来非常方便。对于我来说更重要的是学习这类语言让我想起来我学的第一门编程语言–QBasic。所以Python让我找到了当时最初学习编程的热情，后来就怀抱着这个热情学习Python到现在。

当然，Python让我相见恨晚，非常喜欢这种风格和简洁的语言，后来同样学习了 PyGTK 。所以有了我的PyGTK学习笔记，我想认真学习一下，然后重写我那一波三折的项目，等到成熟了我就会正是发布。学习嘛，我当然是看官网资料咯，但是刚开始去了解 PyGTK的时候我也注意到了 PyGObject ，但是我当时没有放在心上，因为目前 PyGTK 的资料还是挺多的，所以让我忽视了 PyGObject 但是不断学习，就会不断发现。现在正的了解到确实向PyGObject会成为未来的替代品，官方就是这么说的，而且目前的PyGTK是只支持GTK 2的，想要用 Python 来写 GTK 3的应用应该用 PyGObject ，而且在 Gnome 上翻译的时候也发现所有的代码都是用”from gi.repository import Gtk”，而不是以前的”import gtk”了，看到这里我自己想想应该向前看看嘛~所以呢，以后，当然我的 **学习笔记** 会继续随笔写一写，以后的都会围绕 PyGObject 来写。而且看到 Fedora 早就说要在最新发行版里面默认Python3了，不仅如此，Ubuntu 也宣布在最新的发行版里会默认安装Python3 。 这样更说明我要多多向前看啊。所以以后的学习Python/PyGTK/PyGObject的笔记都会用python3 。

在这里先写一个简单的使用Python开发GTK 3的示例：
{% highlight python %}
#!/usr/bin/python
from gi.repository import Gtk

class MyWindow(Gtk.Window):

    def __init__(self):
        Gtk.Window.__init__(self, title="Hello World")

        self.button = Gtk.Button(label="Click Here")
        self.button.connect("clicked", self.on_button_clicked)
        self.add(self.button)

    def on_button_clicked(self, widget):
        print "Hello World"

win = MyWindow()
win.connect("delete-event", Gtk.main_quit)
win.show_all()
Gtk.main()
{% endhighlight %}
图片这次不上了，因为自己试过一遍才是最好的体会！！！！

More: https://python-gtk-3-tutorial.readthedocs.org/en/latest/index.html (English)

因为这段时间一直准备那个什么xxx考试，每天也只能睡觉前看看自己感兴趣的东西，而且偷个懒上个网更新一下博客~呵呵~

PS：本来就是要说一下以后用PyGObject和Python3的，结果扯了这么一大堆！！！ over~

 
