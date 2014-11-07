---
title: PyGtk学习笔记(4)–布局管理(Alignment)
author: DawnDIY
layout: post
permalink: /archives/127
categories:
  - PyGtk
  - Python
tags:
  - Alignment
  - PyGtk
  - Python
---

前面我们学习了HBox和VBox这对灵活的布局器，今天再来学习一个非常灵活的布局管理器——Alignment 。Alignment名为对齐布局管理，顾名思义，它是主要是用来管理部件对齐和子部件大小的。Alignment在对于需要更具Window等部件大小变化而变化的子部件对齐来说，使用非常方面。所以对于制作一款体验较好的应用程序来说，值得学习使用Alignment来管理子部件的对齐和大小。

## 1.介绍

这次先不上图了，先来讲解一下Alignment的相关功能。  
使用Alignment的时候应该注意，Alignment只能为其添加一个子部件，也就是说只能add一个widget。那一个部件怎么对齐？所以，我们通常配合VBox和HBox（[PyGtk学习笔记(3)–布局管理(VBox, HBox)][1]）来使用。详细说，就是我们把需要的部件添加Box中后，再把Box交给Alignment来管理它的子部件的对齐和大小。这样就可以构建美观的UI了。

 [1]: http://www.dawndiy.com/archives/107 "PyGtk学习笔记(3)–布局管理(VBox, HBox)"

Alignment类的概要：

    class gtk.Alignment(gtk.Bin):
        gtk.Alignment(xalign=0.0, yalign=0.0, xscale=0.0, yscale=0.0)
    
        def set(xalign, yalign, xscale, yscale)
    
        def set_padding(padding_top, padding_bottom, padding_left, padding_right)
    
        def get_padding()

 Alignment的继承关系：

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Bin
               -- gtk.Alignment

构造函数：

    gtk.Alignment(xalign=0.0, yalign=0.0, xscale=0.0, yscale=0.0)

**`xalign`** :

数值表示子部件左边的空闲位置占全部水平空闲位置的百分比。数值从0.0到1.0，0表示子部件左对齐，1表示子部件右对齐。默认0.0。

**`yalign`** :

数值表示子部件上边的空闲位置占全部垂直空闲位置的百分比。数值从0.0到1.0，0表示子部件顶对齐，1表示子部件底对齐。默认0.0。

**`xscale`** :

数值表示子部件占全部水平位置的百分比。数值从0.0到1.0，0表示子部件最小宽度，1表示子部件最大宽度。默认0.0。

**`yscale`** :

数值表示子部件占全部垂直位置的百分比。数值从0.0到1.0，0表示子部件最小高度，1表示子部件最大高度。默认0.0。

*Returns* :

一个新的Alignment对象。

其他函数：

    set_padding(padding_top, padding_bottom, padding_left, padding_right)

这个函数是用来设置边距的，也就是说Alignment管理的子部件和外出部件的上、下、左、右的边距。

## 2.实例详解

看实在的，来举一个例子看一看。先上图：

[![][3]][3]如图所示，上面的按钮是使用了Alignment布局管理来控制其大小和对齐的。下面是完整代码：

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-014432的屏幕截图.png

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # Alignment layout container
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class AlignLC:
    	def __init__(self):
    		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.window.set_title("Alignment Layout Container")
    		self.window.set_size_request(300,250)
    		self.window.set_position(gtk.WIN_POS_CENTER)
    
    		btn1 = gtk.Button("btn1")
    		btn2 = gtk.Button("btn2")
    		btn3 = gtk.Button("btn3")
    
    		vbox = gtk.VBox(True,0)
    		vbox.add(btn1)
    		vbox.add(btn2)
    		vbox.add(btn3)
    
    		align = gtk.Alignment(0.5,1,0.7,0)	# 子部件左边空余50%的空闲空间（居中），子部件以最大高度占满垂直空间，子部件宽度为水平空间的70%
    
    		align.add(vbox)
    		self.window.add(align)
    		self.window.connect("destroy",gtk.main_quit)
    		self.window.show_all()
    
    if __name__ == "__main__":
    	al = AlignLC()
    	gtk.main()

因为Alignment只能为其添加一个子部件的特性，所以我们配合使用了VBox。在VBox一个共有3个Button，然后把VBox添加至Alignment内。其中的关键是Alignment实例化的时候其中的参数。参照前面讲的构造函数参数的介绍和代码中我注释的内容，第一个 0.5 表示VBox中的三个Button左边的空间占全部空闲空间的50%,这样也达到了居中的效果。第二个 1 表示的是，垂直方向上三个Button顶部的空间为全部空闲空间，也就是说底部距离为0。第三个 0.7 表示三个Button的总宽度为水平宽度的70%。最后一个 0 表示三个Button的高度为其最小高度。  
而且这里使用Alignment布局管理来处理的大小和对齐方案都是适应窗口变化的，即使窗口重新调整，这些布局也能按照设置来适应当前变化。

## 3.其他示例

上面一个例子可能不好理解，下面我诺列一下参数变化后不同的效果：

gtk.Alignment(0,0,0,0) 效果：

[![][4]][4]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-020507的屏幕截图.png

gtk.Alignment(0.5,0,0,0) 效果：

[![][5]][5]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-020537的屏幕截图.png

gtk.Alignment(0.5,0,0.5,0) 效果：

[![][6]][6]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-020604的屏幕截图.png

gtk.Alignment(0.5,0.8,0.5,0) 效果：

[![][7]][7]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-020712的屏幕截图.png

gtk.Alignment(0.5,0.8,0.5,0.5) 效果：

[![][8]][8]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-020745的屏幕截图.png

gtk.Alignment(0,0,1,1) 效果：

[![][9]][9]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-021025的屏幕截图.png

gtk.Alignment(0,0,1,1)  
align.set_padding(50,30,10,2)  
效果：

[![][10]][10]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-29-021121的屏幕截图.png

前面没有讲到set_padding这个方法，这个方法也就设置边距，如上图就是设置顶部边距为50，底部边距为30，左边距为10，右边距为2的效果。

Alignment是一个灵活的布局管理器，主要用于管理其子部件的大小尺寸和对齐方案。配合使用其他的布局管理，你可以用它来构造体验有好的UI。

未完，待续……

 

 

 
