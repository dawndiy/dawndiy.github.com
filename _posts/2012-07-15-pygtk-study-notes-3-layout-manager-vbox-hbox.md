---
title: PyGtk学习笔记(3)–布局管理(VBox, HBox)
author: DawnDIY
layout: post
permalink: /archives/107
categories:
  - PyGtk
  - Python
tags:
  - HBox
  - PyGtk
  - Python
  - VBox
---
# 

前面一次学习了一个很简单的Fixed布局方式，这次和DawnDIY来学习一下最常用的Box布局管理。Box布局管理分为VBox和HBox两种，在GTK 3中都把这两个合并为Box一个部件了，但是目前来说PyGtk还是GTK 2的，所以DawnDIY还是建议还是把VBox和HBox单独用，不用统一成Box，这样写出来的程序兼容性更好。GTK 3中的Box可以等到PyGObject比较普及、稳定的时候在用。毕竟还是那句话，用最稳定的，不用最新的。

## 1.介绍

下面我们先介绍一下VBox和HBox。

VBox是一个垂直布局容器。这个容器里的部件都是以垂直排列的方式一个个竖直分布在容器中。  
HBox是一个水平布局容器。这个容器里的部件都是以水平排列的方式一个个横向分布在容器中。  
这两个布局容器是最常用的两个布局容器，我们可以配合使用他们来构建出你想要的UI。



## 2.VBox

先来看一个VBox的示例，上图先：

[![][2]][2]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-15-181439的屏幕截图.png

从上面的图中可以看到，有两个按钮，一个大一个小，而且是竖直排列的。small和big按钮都是放在VBox中，然后在把VBox添加到主窗口中就行了，这个示例很简单，下面是完整代码：

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # VBox layout container
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class VBoxLC:
    	def __init__(self):
    		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.window.set_title("VBox Layout Container")
    		self.window.set_size_request(300,250)
    		self.window.set_position(gtk.WIN_POS_CENTER)
    
    		self.window.connect("destroy", gtk.main_quit)
    
    		vbox = gtk.VBox(False, 5)    # 建立 VBox 布局容器，空间不均等分配，子部件间隔 5 像素
    		btn1 = gtk.Button("small")
    		btn2 = gtk.Button("Big")
    		btn2.set_size_request(300,200)
    
    		vbox.add(btn1)
    		vbox.add(btn2)
    		self.window.add(vbox)
    
    		self.window.show_all()
    
    	def main(self):
    		gtk.main()
    
    if __name__ == "__main__":
    	vbox = VBoxLC()
    	vbox.main()

在分析代码之前我们先来看一下VBox类的概要：

    class gtk.VBox(gtk.Box):
        gtk.VBox(homogeneous=False, spacing=0)

VBox的继承关系

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Box
               -- gtk.VBox

构造函数：

    gtk.VBox(homogeneous=False, spacing=0)

**`homogeneous`** :

如果为 True 所有的子部件都会被分配均等的空间

**`spacing`** :

垂直空间子部件之间的像素大小。

*Returns* :

一个新的 gtk.VBox

如上面的例子来说，我们建立了一个 window ，然后建立了一个子部件空间分配不均等、子部件间隔5像素的VBox，然后建立两个Button，并且添加到VBox中，最后在把VBox添加到window中。最后就完成了一个简单的垂直排列的布局了。

## 3.HBox

接下来同样介绍一下HBox，它和VBox十分相似，不过它用来管理水平排列的布局，来看一个相同的示例，先上图：

[![][3]][3]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-15-183711的屏幕截图.png

如上图，我们对比一下之前的图就很容易看出，HBox管理部件的水平排列布局，在很多应用程序的布局都是应用VBox和HBox配合使用来构建出丰富的UI来的。下面我们看一下完整代码，其实你很容易看出不同：

    #!/usr/bin/env python
    
    # HBox layout container
    # PyGtk Stady Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class HBoxLC:
    	def __init__(self):
    		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.window.set_title("HBox Layout Container")
    		self.window.set_size_request(300,250)
    		self.window.set_position(gtk.WIN_POS_CENTER)
    
    		self.window.connect("destroy", gtk.main_quit)
    
    		hbox = gtk.HBox(False, 5)
    		btn1 = gtk.Button("small")
    		btn2 = gtk.Button("Big")
    		btn2.set_size_request(200,150)
    
    		hbox.add(btn1)
    		hbox.add(btn2)
    		self.window.add(hbox)
    
    		self.window.show_all()
    
    	def main(self):
    		gtk.main()
    
    if __name__ == "__main__":
    	hbox = HBoxLC()
    	hbox.main()

下面同样是HBox类的概要和继承关系：

    class gtk.HBox(gtk.Box):
        gtk.HBox(homogeneous=False, spacing=0)

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Box
               -- gtk.HBox

##  4.总结

学习了一下VBox和HBox，非茶有用的两个布局管理部件，而且配合使用可以构建出非常复杂的UI，其实DawnDIY觉得VBox和HBox很容易让人想到HTML中的和标签，它们可以自由的相互嵌套来构建UI。所以你有好的设计，可以尝试一下用VBox和HBox把它设计出来吧。

今天学到这里，待续。。。

 