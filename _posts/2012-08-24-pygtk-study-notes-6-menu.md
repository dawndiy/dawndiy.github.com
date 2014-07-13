---
title: PyGtk学习笔记(6)–菜单
author: DawnDIY
layout: post
permalink: /archives/290
categories:
  - PyGtk
  - Python
tags:
  - Menu
  - MenuBar
  - MenuItem
  - PyGtk
  - Python
---
# 

前面一直都在讲布局，布局学的差不多了，使用前面讲的所有布局管理已经可以设计出复杂的UI了。那么接下来来学习一下GUI程序中最常用的控件之一——菜单。

## 1.介绍

菜单栏(Menubar)是GTK中使用最多的控件之一，在菜单栏上(Menubar)上我们可以添加各式各样的菜单(Menu)和菜单项(MenuItem)。其中菜单项大致分为这几种：普通菜单项(MenuItem)、带图标的菜单项(ImageMenuItem)、带复选框的菜单项(CheckMenuItem)、分级菜单项 和 分割线(SeparatorMenuItem)。

先上一张图，看一看一个最普通的菜单栏：

[![][2]][2]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/2012-08-24-125955的屏幕截图.png

上面图就是一个普通的菜单栏，我们在菜单栏上面设置了四个菜单，接下来就分别详细介绍一下常用的几种菜单项。



## 2.普通菜单

普通菜单就是形式只有文字的最基本菜单，如图：

[![][3]][3]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/Menu_007.png

如图，我们在菜单栏上设置了一个File1菜单，然后我们在File1下设置了3个菜单项和一个分割线。这就是一个最基本的菜单了。下面我们来看看它的代码。

**部分代码：**

    Mb = gtk.MenuBar()
    
    		# 普通菜单
    		filemenu = gtk.Menu()
    		filem = gtk.MenuItem("File_1")
    		filem.set_submenu(filemenu)
    
    		new = gtk.MenuItem("New")
    		filemenu.append(new)
    
    		open = gtk.MenuItem("Open")
    		filemenu.append(open)
    
    		sep = gtk.SeparatorMenuItem()
    		filemenu.append(sep)
    
    		exit = gtk.MenuItem("Exit")
    		exit.connect("activate", gtk.main_quit)
    		filemenu.append(exit)
    
    		Mb.append(filem)

如上面代码 首先新建一个菜单栏(MenuBar)

    filemenu = gtk.Menu()

接下来要设置一个显示在菜单栏上的菜单(Menu)和菜单项(MenuItem)

    filemenu = gtk.Menu()  # 菜单
    filem = gtk.MenuItem("File_1")   # 菜单项，并添加快捷键(ALT 1)
    filem.set_submenu(filemenu)    # 将菜单项设置到菜单上，显示在菜单栏上

然后我们就要为这个菜单添加更多的菜单项了

    new = gtk.MenuItem("New")
    filemenu.append(new)   # 把菜单项添加到菜单下
    
    open = gtk.MenuItem("Open")
    filemenu.append(open)

添加了菜单项，我们的目的当然是要使得点击菜单能够**触发事件**，那么我们就可以用connect

    exit = gtk.MenuItem("Exit")
    exit.connect("activate", gtk.main_quit)   # 为exit菜单项添加事件
    filemenu.append(exit)

当然，为了很好的把菜单项分类，我们可以使用分割线

    sep = gtk.SeparatorMenuItem()
    filemenu.append(sep)

一个最普通的菜单就是这样的~

## 3.带图标的菜单

在GUI程序里，图标往往给人带来请切感，也使得菜单更加美观。惯例，先上个图：

[![][4]][4]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/Menu_008.png

这些丰富的图标都是GTK自带的，GTK为菜单、按钮、工具栏等等都提供了一套原生的图标，所以可以很方便的使用。下面我们来看一下带图标的菜单是如何用代码实现的。

**部分代码：**

    Mb = gtk.MenuBar()
    		# 带图标的菜单
    		imagemenu = gtk.Menu()
    		filei = gtk.MenuItem("File_2")
    		filei.set_submenu(imagemenu)
    
    		newi = gtk.ImageMenuItem(gtk.STOCK_NEW)
    		newi.set_label("New")
    		imagemenu.append(newi)
    
    		openi = gtk.ImageMenuItem(gtk.STOCK_OPEN)
    		openi.set_label("Open")
    		imagemenu.append(openi)
    
    		sep2 = gtk.SeparatorMenuItem()
    		imagemenu.append(sep2)
    
    		exiti = gtk.ImageMenuItem(gtk.STOCK_QUIT)
    		exiti.set_label("Exit")
    		exiti.connect("activate", gtk.main_quit)
    		imagemenu.append(exiti)
    
                    Mb.append(filei)

看到代码，我们发现其实就是使用了 **gtk.ImageMenuItem** 这个函数。

首先我们新建带图标的菜单项，并且给它设置图标

    newi = gtk.ImageMenuItem(gtk.STOCK_NEW)

其中的 **gtk.STOCK_NEW** 是GTK预设的 NEW 图标。**有关所有的GTK预设的 STOCK 请看这里：[Stock Items][4]**

 [4]: http://developer.gnome.org/pygtk/stable/gtk-stock-items.html "Gtk Stock Items"

然后是设置菜单项显示的文字(标签)，并添加至菜单中

    newi.set_label("New")
    imagemenu.append(newi)

这就是带图标的菜单，让你的菜单更美观~

## 4.带复选框的菜单

在GUI程序中常常用菜单选项来对程序进行设置，那么带复选框的菜单就可以完成这样的功能，先上图：

[![][6]][6]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/Menu_009.png

如图，New选项目前是勾选上的，其他的都没有勾选，这样的菜单我们就能够拿来当设置选项来用。下面看一下代码有什么不同。

**部分代码：**

    Mb = gtk.MenuBar()
    		# 带复选框的菜单项
    		checkmenu = gtk.Menu()
    		filec = gtk.MenuItem("File_3")
    		filec.set_submenu(checkmenu)
    
    		newc = gtk.CheckMenuItem("New")
    		newc.set_active(True)		# 激活复选框
    		checkmenu.append(newc)
    
    		openc = gtk.CheckMenuItem("Open")
    		openc.set_active(False)	
    		checkmenu.append(openc)
    
    		sep3 = gtk.SeparatorMenuItem()
    		checkmenu.append(sep3)
    
    		exitc = gtk.CheckMenuItem("Exit")
    		checkmenu.append(exitc)
    
    		Mb.append(filec)

如上面的代码，我们通过以下代码来新建菜单项，这就是和别的菜单项不同的地方

    newc = gtk.CheckMenuItem("New")

接下来是设置**复选框的状态**，默认复选框是没有被选中的。

    newc.set_active(True) # 激活复选框

同样，选中后触发的实际依然是用**connect**。

## 5.分级的菜单项

在菜单下的菜单项非常多的时候我们往往使用分割线来把不同类型的菜单项分隔开来，但是更多的菜单项的时候就会显得一个菜单下面老长一条的，显得没有调理，这时候我们可以使用分级菜单，按照菜单项分类，把同一类的归于一个菜单项的子项。这样就既有调理，又美观。看一个分级菜单的图先(图没截好，但你懂得~ ^.^)。

[![][7]][7]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/Menu_012.png

其实分级的菜单也就是菜单一层套一层实现的。

**部分代码：**

    # 多级菜单
    		topmenu = gtk.Menu()	
    		files = gtk.MenuItem("File_4")
    		files.set_submenu(topmenu)
    
    		submenu = gtk.Menu()	# 上层
    
    		news = gtk.MenuItem("New")
    		news.set_submenu(submenu)
    
    		sub1 = gtk.MenuItem("Sub1")		# 子层
    		sub2 = gtk.MenuItem("Sub2")
    		sub3 = gtk.MenuItem("Sub3")
    
    		submenu.append(sub1)
    		submenu.append(sub2)
    		submenu.append(sub3)
    
    		topmenu.append(news)
    
    		self.append(filem)
    		self.append(filei)
    		self.append(filec)
    		self.append(files)

在代码中我们能够发现的是，其实就是在普通的菜单的基础上再套了一层菜单。通俗的讲，首先我们建立菜单，然后将菜单项添加至菜单，随后再一个个向其中添加菜单项(MenuItem)，然后分级菜单在这里不同的是，本要一个个添加菜单项(MenuItem)的时候这里添加的确实菜单(Menu)，然后在向这个子菜单(Menu)添加菜单项(MenuItem)，这样就达到了多级菜单的效果了。

## 6.菜单快捷键

在前面普通菜单的地方，我们提到了菜单的快捷键，在菜单项标签设置的时候，在字母或数字前加“_”即可，这样的话，在当前活动窗口按下 **ALT** 键不放，下划线就会出现，这时你再按下相应的字母和数字就能激活该菜单了。设置如下：

    files = gtk.MenuItem("File_4")

值得注意的是，要是把这种下划线的快捷方式用在菜单里面的每个菜单项也能用吗？答案是当然能用，但是不同的是，你首先要使用 **ALT 字母** 先激活打开菜单，然后再 **按下相应字母** 才能选中该菜单项，**而不能**在当前窗口之间按 **ALT 字母** 来触发。

那有什么方法能够是的在整个窗口全局的情况下，不用进入菜单直接按快捷方式就能完成点击某个菜单子项的功能吗？答案依然是可以的，使用** add_accelerator** 这个函数就能实现。

先看效果图：

[![][8]][8]

 []: http://www.dawndiy.com/wp-content/uploads/2012/08/Menu_013.png

如图我们设置的快捷键是 **Ctrl Q** ,同时Exit的标签我们也用“_Exit”设置过了，放在一起对比一下，下面是**该例子的完整代码：**

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # Menu_accelerator
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class MenuTest(gtk.Window):
    	def __init__(self):
    		super(MenuTest, self).__init__()
    
    		self.set_title("Menu")
    		self.set_size_request(300,250)
    		self.set_position(gtk.WIN_POS_CENTER)
    
    		mb = gtk.MenuBar()
    
    		filemenu = gtk.Menu()
    		filem = gtk.MenuItem("_File")
    		filem.set_submenu(filemenu)
    
    		agr = gtk.AccelGroup()
    		self.add_accel_group(agr)
    
    		exit = gtk.MenuItem("_Exit", agr)
    
    		key, mod = gtk.accelerator_parse("Q")
    		exit.add_accelerator("activate", agr, key, mod, gtk.ACCEL_VISIBLE)
    		exit.connect("activate", gtk.main_quit)
    		filemenu.append(exit)
    
    		mb.append(filem)
    
    		vbox = gtk.VBox(False, 2)
    		vbox.pack_start(mb, False, False, 0)
    
    		self.add(vbox)
    		self.connect("destroy", gtk.main_quit)
    		self.show_all()
    
    if __name__ == "__main__":
    	mt = MenuTest()
    	gtk.main()

看到代码中，比我们前面讲的多了一些东西。

为了使用快捷键,我们先创建了一个全局的AccelGroup对象,它将在之后被使用

    agr = gtk.AccelGroup()
    self.add_accel_group(agr)

在菜单项建立的时候，添加该菜单项的快捷方式属于那个AccelGroup对象

    exit = gtk.MenuItem("_Exit", agr)

然后就是设置具体的快捷键了。

    key, mod = gtk.accelerator_parse("Q")
    exit.add_accelerator("activate", agr, key, mod, gtk.ACCEL_VISIBLE)

**详细说一下几个函数:**

### gtk.accelerator_parse

    def gtk.accelerator_parse(accelerator)

  **`accelerator`** :

一个表示快捷键的字符串

*Returns* :

一个包含快捷方式的 键值 和 修饰符 的 2元组

### gtk.Widget.add_accelerator

    def add_accelerator(accel_signal, accel_group, accel_key, accel_mods, accel_flags)

  **`accel_signal`** :

快捷键激活的控件信号

**`accel_group`** :

控件快捷方式的组，加入它的顶层

**`accel_key`** :

快捷方式的键值 如(‘q’)

**`accel_mods`** :

快捷键的修饰符

**`accel_flags`** :

快捷键标识, 如 `gtk.ACCEL_VISIBLE`

运行上面的代码后你会发现，在当前活动的窗口下，直接按 ALT E 是没有效果的，窗口不会关闭，非要先按 Alt E 激活菜单再按E键就能关闭窗口，这是第一种快捷方式。然而我们加入了第二种后，不管有没有激活菜单，直接按下 Ctrl Q 即可关闭窗口，这就是两种不同的快捷方式的设置。

## 7.完整代码

**下面是前面讲到的四中菜单示例的完整代码：**

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # Menu
    # PyGtk Stady Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    # 菜单条
    class Mb(gtk.MenuBar):
    	def __init__(self):
    		super(Mb,self).__init__()
    
    		# 普通菜单
    		filemenu = gtk.Menu()
    		filem = gtk.MenuItem("File_1")
    		filem.set_submenu(filemenu)
    
    		new = gtk.MenuItem("New")
    		filemenu.append(new)
    
    		open = gtk.MenuItem("Open")
    		filemenu.append(open)
    
    		sep = gtk.SeparatorMenuItem()
    		filemenu.append(sep)
    
    		exit = gtk.MenuItem("Exit")
    		exit.connect("activate", gtk.main_quit)
    		filemenu.append(exit)
    
    		# 带图标的菜单
    		imagemenu = gtk.Menu()
    		filei = gtk.MenuItem("File_2")
    		filei.set_submenu(imagemenu)
    
    		newi = gtk.ImageMenuItem(gtk.STOCK_NEW)
    		newi.set_label("New")
    		imagemenu.append(newi)
    
    		openi = gtk.ImageMenuItem(gtk.STOCK_OPEN)
    		openi.set_label("Open")
    		imagemenu.append(openi)
    
    		sep2 = gtk.SeparatorMenuItem()
    		imagemenu.append(sep2)
    
    		exiti = gtk.ImageMenuItem(gtk.STOCK_QUIT)
    		exiti.set_label("Exit")
    		exiti.connect("activate", gtk.main_quit)
    		imagemenu.append(exiti)
    
    		# 带复选框的菜单项
    		checkmenu = gtk.Menu()
    		filec = gtk.MenuItem("File_3")
    		filec.set_submenu(checkmenu)
    
    		newc = gtk.CheckMenuItem("New")
    		newc.set_active(True)		# 激活复选框
    		checkmenu.append(newc)
    
    		openc = gtk.CheckMenuItem("Open")
    		openc.set_active(False)	
    		checkmenu.append(openc)
    
    		sep3 = gtk.SeparatorMenuItem()
    		checkmenu.append(sep3)
    
    		exitc = gtk.CheckMenuItem("Exit")
    		checkmenu.append(exitc)
    
    		# 多级菜单
    		topmenu = gtk.Menu()	
    		files = gtk.MenuItem("File_4")
    		files.set_submenu(topmenu)
    
    		submenu = gtk.Menu()	# 上层
    
    		news = gtk.MenuItem("New")
    		news.set_submenu(submenu)
    
    		sub1 = gtk.MenuItem("Sub1")		# 子层
    		sub2 = gtk.MenuItem("Sub2")
    		sub3 = gtk.MenuItem("Sub3")
    
    		submenu.append(sub1)
    		submenu.append(sub2)
    		submenu.append(sub3)
    
    		topmenu.append(news)
    
    		self.append(filem)
    		self.append(filei)
    		self.append(filec)
    		self.append(files)
    class Win:
    	def __init__(self):
    		self.win = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.win.set_title("Menu")
    		self.win.set_size_request(300,250)
    		self.win.set_position(gtk.WIN_POS_CENTER)
    
    		mb = Mb()
    		vbox = gtk.VBox(False, 2)		
    		vbox.pack_start(mb, False, False, 0)
    
    		self.win.add(vbox)
    
    		self.win.connect("destroy", gtk.main_quit)
    		self.win.show_all()
    
    if __name__ == "__main__":
    	win = Win()
    	gtk.main()

OK~菜单基本上就是这些了，今天学习到这里。。。待续。。。

 

 

 

 

 

 

 

 

 