---
title: PyGtk学习笔记(2)–布局管理(Fixed)
author: DawnDIY
layout: post
permalink: /archives/97
categories:
  - PyGtk
  - Python
tags:
  - Fixed
  - PyGtk
  - Python
---
# 

前面介绍了一下PyGtk，使用它可以做Python的GUI程序。在前面的例子中我们只实现了一个简单的窗口，那么要在窗口里面加内容怎么办？一个add就行了，但是如果想要做出包含丰富的元素，那么就要使用布局管理了。写过GUI的人都知道布局管理是多么重要的事情。虽然现在有很好的Glade工具来实现直接拖拽的方式来进行UI设计，但是毕竟学习的过程还是要从最基本的学起，所以和我一起来学习PyGtk的布局管理吧。

为了组织我们的部件,我们使用专门的不可见部件,其被称为布局容器 (layout containers)。PyGtk常用的有Alignment,Fixed,VBox和Table这四种布局容器(layout containers)。

这里先学习一下简单的Fixed布局，Fixed容器将放置位置固定和尺寸固定的子部件。这个容器不进行自动的布局管理。在大多数的程序中,我们不用这种容器。但是在一些专门的领域,我们会用它。例如游戏,一些工作在图表中的专门程序,那些能被移动可变化尺寸的组件(就想在电子表格程序中的一个chart表一样),小型的学习示例等。

A picture is worth a thousand words:

[![][2]][2]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-07-201352的屏幕截图.png

如上图，我用了Fixed的布局方式，顺带展示一下Button的几种显示，完整代码如下：



    #!/usr/bin/env python
    
    # Fixed layout container
    # PyGtk Study Notes By DawnDIY
    # http://dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class FixedLC:
    	def __init__(self):
    		self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.window.set_title("Fixed Layout Container")
    		self.window.set_size_request(300,250)
    		self.window.set_position(gtk.WIN_POS_CENTER)
    
    		btn1 = gtk.Button("Button1")
    		btn2 = gtk.Button("Button2")
    		btn3 = gtk.Button("Button3")
    		btn4 = gtk.Button(stock = gtk.STOCK_CLOSE)
    
    		btn2.set_sensitive(False)
    		btn3.set_size_request(80,40)
    
    		fixed = gtk.Fixed()
    
    		fixed.put(btn1, 30, 30)
    		fixed.put(btn2, 150, 30)
    		fixed.put(btn3, 30, 130)
    		fixed.put(btn4, 150, 130)
    
    		btn4.connect("clicked", gtk.main_quit)
    		self.window.connect("destroy", gtk.main_quit)
    
    		self.window.add(fixed)
    		self.window.show_all()
    
    	def main(self):
    		gtk.main()
    
    if __name__ == "__main__":
    	fixedLC = FixedLC()
    	fixedLC.main()

分析上面的程序，我们设置了4个Button：btn1是一个普通的Button，btn2是一个不敏感的Button，btn3是一个自定义大小的Button，而btn4是根据GTK中的ITEM选择的一些常用Button。在窗口上，用Fixed容器来布局，调用put方法来向其添加部件，然后记得带上坐标就OK了，所以这种方法很简单，但是这中方法不通用，只能用在一些特殊界面中。

下面详细说一下Fixed：

Fixed的继承关系：

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Fixed

Fixed的概要：

    class gtk.Fixed(gtk.Container):
        gtk.Fixed()
    
        def put(widget, x, y)
    
        def move(widget, x, y)
    
        def set_has_window(has_window)
    
        def get_has_window()

这里列出的Fixed的函数。

    def put(widget, x, y)

添加一个widget，x、y分别是该widget在fixed中的横、纵坐标。（只要是x、y都是指widget的左上角）

    def move(widget, x, y)

移动一个widget，值得注意的是这里的widget一定要是已经添加的fixed中的widget，如果不是已经添加的，这个函数将不起任何作用。x、y分别是这个子widget需要移动到的坐标

    def set_has_window(has_window)

`set_has_window`()方法指定根据has\_window的值来决定是否创建一个单独的window。如果has\_window的值为Ture，则fixed会创建它自己的单独的window。在默认情况下，has_window的值是False并且fixed会被建立在一个没有独立的window中。这个方法必须使用在fixed还没有实现的时候，例如，在window创建后立即使用。

    def get_has_window()

如果fixed拥有自己独立的window则返回True，否则返回False。

 

这就是Fixed布局容器，只用坐标定位的简单布局，虽然不常用，但也是一个简单的布局，适用于一些小型示例中。这次就先学这么多，和大家分享这写。

待续布局容器。。。

 