---
title: PyGtk学习笔记(5)–布局管理(Table)
author: DawnDIY
layout: post
permalink: /archives/186
categories:
  - PyGtk
  - Python
tags:
  - PyGtk
  - Python
  - Table
---
# 

这次学习一下一个简单的布局管理–Table。Table我说他简单是因为是一个“方方正正”的容器，和HTML中的标签类似，它使用于一些平面需要行和列排列的UI中。

## 1.介绍

Table可以用来很好的管理行和列对齐和布局的UI中，比如《计算器》、《五子棋》、《扫雷》等，这些程序的主界面都是许许多多的子部件方方正正的行列对齐地排列在一起的。说道这几个程序，你应该对Table有一定的猜想了吧。下面开始介绍。

### Table类的概要：

    class gtk.Table(gtk.Container):
        gtk.Table(rows=1, columns=1, homogeneous=False)
    
        def resize(rows, columns)
    
        def attach(child, left_attach, right_attach, top_attach, bottom_attach, xoptions=gtk.EXPAND|gtk.FILL, yoptions=gtk.EXPAND|gtk.FILL, xpadding=0, ypadding=0)
    
        def set_row_spacing(row, spacing)
    
        def get_row_spacing(row)
    
        def set_col_spacing(column, spacing)
    
        def get_col_spacing(column)
    
        def set_row_spacings(spacing)
    
        def get_default_row_spacing()
    
        def set_col_spacings(spacing)
    
        def get_default_col_spacing()
    
        def set_homogeneous(homogeneous)
    
        def get_homogeneous()

### 

### Table的继承关系：

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Table

###  构造方法：

    gtk.Table(rows=1, columns=1, homogeneous=False)

参数：

**`rows`** :

行数

**`columns`** :

列数

**`homogeneous`** :

如果为True，所有的单元格都与最大的单元格尺寸相同

*Returns* :

`一个新的 gtk.Table 对象`

如果 rows 和 columns 没有赋值，默认为 1 。

**注意：**  
这里的rows和columns意思是行数和列数，但在布局的时候我们如果把它们理解成分割线的话这样更好理解。因为下面attach这个方法中的一些参数是通过起始和结束来确定一个子控件的位置的，所以我们把行列抽象理解成分割线的话更容易理解构建。比如 rows =2 , columns = 2 , 那么它的布局应该是这样的：

0               1                 2  
0 ———- ———-   
|                  |                 |  
1 ———- ———-   
|                  |                 |  
2 ———- ———- 

### 主要方法：

**gtk.Table.attach**

    def attach(child, left_attach, right_attach, top_attach, bottom_attach, xoptions=gtk.EXPAND|gtk.FILL, yoptions=gtk.EXPAND|gtk.FILL, xpadding=0, ypadding=0)

**`child`** :

需要添加的控件

**`left_attach`** :

子控件左部起始列号。（**可以用上面的分割线来理解**）

**`right_attach`** :

子控件右部结束列号。

**`top_attach`** :

子控件顶部起始列号。

**`bottom_attach`** :

子控件底部结束列号。

**`xoptions`** :

当 table 水平调整大小时，用于指定子控件的属性。默认值 `gtk.FILL`|`gtk.EXPAND`

**`yoptions`** :

当 table 垂直调整大小时，用于指定子控件的属性。默认值 `gtk.FILL`|`gtk.EXPAND`

**`xpadding`** :

添加控件的左侧和右侧的填充量，默认值 0

**`ypadding`** :

添加控件的上侧和下侧的填充量，默认值 0

*`xoptions`* 和 *`yoptions`* 确定控件在水平和垂直方向上的扩展属性，默认值是：`gtk.FILL`|`gtk.EXPAND`

  `gtk.EXPAND`

table单元格将扩展占据分配的所有空闲空间。

`gtk.SHRINK`

控件随着table单元格收缩而收缩。

`gtk.FILL`

空间将会填充所有table单元格分配的空间。

## 2.示例

上面都是不实在的东西，下面来看一个例子，有图有真相，先上图：

[![][2]][2]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-31-163045的屏幕截图.png

如图所示，你应该想到Table是怎么一个布局容器吧。接下来看一下完整代码：

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # Table layout container
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class TableLC:
    	def __init__(self):
    		self.win = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.win.set_title("Table Layout Container")
    		self.win.set_size_request(300,250)
    		self.win.set_position(gtk.WIN_POS_CENTER)
    
    		table = gtk.Table(4, 3, True)	# 4行3列
    		table.attach(gtk.Button("1"), 0, 1, 0, 1)
    		table.attach(gtk.Button("2"), 1, 2, 0, 1)
    		table.attach(gtk.Button("3"), 2, 3, 0, 1)
    		table.attach(gtk.Button("4"), 0, 1, 1, 2)
    		table.attach(gtk.Button("5"), 1, 2, 1, 2)
    		table.attach(gtk.Button("6"), 2, 3, 1, 2)
    		table.attach(gtk.Button("7"), 0, 1, 2, 3)
    		table.attach(gtk.Button("8"), 1, 2, 2, 3)
    		table.attach(gtk.Button("9"), 2, 3, 2, 3)
    		table.attach(gtk.Button("0"), 1, 2, 3, 4)
    
    		self.win.add(table)
    		self.win.connect("destroy", gtk.main_quit)
    		self.win.show_all()
    
    if __name__ == "__main__":
    	tab = TableLC()
    	gtk.main()

在这个例子中，我们主要看的还是Table中子控件的布局方式，结合前面介绍的 table.attach 方法，我们首先通过 gtk.Table(4, 3, True)来建立了一个 4行 3列 的 table 。  
那么这些数字是怎么布局的呢？  
就添加数字“1”来说明，table.attach(gtk.Button(“1″), 0, 1, 0, 1) ，首先第一个参数就不说了；第二个参数 “0”表示控件从第0列起始；第三个参数“1”表示控件结束于第2列前；第四个参数“0”表示控件开始于第0行；第五个参数表示控件结束于第1行前。这样就确定了数字“1”的位置以及所占行列了。  
其他的也就都是一样的。

那么我们多举些例子，改一下代码看看有什么效果。

如果把数字“0”的布局改成：

    table.attach(gtk.Button("0"), 1, 3, 3, 4)

效果如下：

[![][3]][3]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-31-170305的屏幕截图.png

因为我将第二个参数和第三个参数改成了 1 , 3 。也就是说数字“0”是从第1列起始结束于第3列前。这样它就占据了2列的位置了。

table.attach 还有四个参数没有讲到，下面我们继续修改一下代码。

我们先说一下我们接下来要修改的目的：  
1.数字“1”上下分别设置边距为5，左右边距分别为10。  
2.数字“2”垂直方向设置为FILL，水平方向设置为EXPAND。  
3.数字“3”水平垂直方向均设置为`SHRINK`。

那么我们修改的局部代码为：

    table.attach(gtk.Button("1"), 0, 1, 0, 1, gtk.EXPAND|gtk.FILL, gtk.EXPAND|gtk.FILL, 10, 5)
    		table.attach(gtk.Button("2"), 1, 2, 0, 1, gtk.EXPAND, gtk.FILL)
    		table.attach(gtk.Button("3"), 2, 3, 0, 1, gtk.SHRINK, gtk.SHRINK)

效果：

[![][4]][4]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-31-171421的屏幕截图.png

就是这样的。table.attach 后面几个参数，对于动态调整窗口时，控件的布局非常有用。认真参考上面我给出的 table.attach 的介绍，是能够理解的。

OK，今天学到这里，使用这些布局已经能够构建很多UI了，下次学学别的，未完，待续…………..

 

 

 