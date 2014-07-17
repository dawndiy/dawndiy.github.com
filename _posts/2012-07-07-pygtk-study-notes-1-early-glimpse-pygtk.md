---
title: 'PyGtk学习笔记(1)-初窥PyGtk'
author: DawnDIY
layout: post
permalink: /archives/72
categories:
  - PyGtk
  - Python
tags:
  - PyGtk
  - Python
---
# 

之前一直在学习Python，Python确实是一门非常简练的编程语言。然而学习了一段时间后，用Python怎么做GUI程序呢，那这里有一个很好的GUI库可以使用，就是PyGtk。PyGtk是Gtk 的Python绑定版本，使用这个GUI可以方便的设计GUI程序。而且对比我之前用过的Java-Gnome，这个方便多了。所以要做GUI程序，值得学习一下PyGtk的使用，在此记录下自己的学习笔记分享给大家。

## 1.PyGtk简介

[PyGtk][1]是一套用[Python][2]封装的GTK 的图形库，通过Python编程语言使用PyGtk图形库可以轻松的写出GUI程序。它是GNOME项目的一部分。PyGTK是基于LGPL许可之下的免费软件。其原始作者是James Henstridge。PyGTK非常容易使用,对于速成原型法,它是相当理想的。普遍地认为,PyGTK是最流行的GTK 库封装中的一种。

 [1]: http://zh.wikipedia.org/wiki/PyGTK "PyGtk Wiki"
 [2]: http://zh.wikipedia.org/wiki/Python "Python Wiki"

其中PyGtk包含几个模块：GObject、ATK、GTK、Pango、Cairo、Clade  
GObject是基类,它为PyGTK所以类提供通用的属性和函数。

*   ATK 是一个提供辅助功能的工具包。该工具包提供了帮助残障人士使用计算机的各种工具。
*   GTK 是用户界面模块。
*   Pango是一个用于处理文本和国际化的库。
*   Cairo是一个用于创建2D矢量模型的库。
*   Glade是用来从XML描述中构建GUI界面。

如果你是Linux用户的话，不必担心安装配置问题，目前大部分Linux发行版中都包含了Python、PyGtk，所以直接用就行了。

## 2.从一个简单示例开始

先上图，接下来的程序效果如下图：

[![][4]][4]

 []: http://www.dawndiy.com/wp-content/uploads/2012/07/2012-07-06-234811的屏幕截图.png

很简单的一个窗口，下面是实现它的完整代码：


```python
    #!/usr/bin/python
    #-*- encoding:utf-8 -*-
    #建立一个窗口
    
    import gtk
    class PyApp(gtk.Window):
    	def __init__(self):
    		super(PyApp, self).__init__()
    		self.set_title("PyGtk")
    		self.set_size_request(250, 150)
    		self.set_position(gtk.WIN_POS_CENTER)
    
    		self.connect("destroy", gtk.main_quit)
    
    		self.show()
    
    	def main(self):
    		gtk.main()
    
    print __name__
    if __name__ == "__main__":
    	pyapp = PyApp()
    	pyapp.main()
```

使用PyGtk当然要有一定的Python基础，把上述代码保存为pygtkwin.py，在控制台执行如下命令就能看到一个窗口了。

    python pygtkwin.py

简单分析一下代码：

    import gtk

这里是导入PyGtk的gtk模块。

    self.set_title("PyGtk")
    self.set_size_request(250, 150)
    self.set_position(gtk.WIN_POS_CENTER)

这里的PyApp继承至GTK的窗口类，即gtk.Window。上面的set分别是设置窗口标题、窗口尺寸、窗口位置。

    self.connect("destroy", gtk.main_quit)

这里的connect是把该类的destroy事件绑定到gtk.main_quit方法上。效果就是点击窗口的关闭按钮，就会销毁整个装口。

    self.show()

用来现实这个窗口。

    gtk.main()

使用于启动GTK的循环，来保持窗口的运行。

到此，你就算初识PyGtk了。我也是在学习的过程，记录下自己的学习笔记和大家一起分享学习。待续…

一些有用的网站：  
Python官网：  
PyGtk官网：

**以后 PyGtk/PyGObject 学习笔记的代码全部在 github 上管理，地址：**

 
