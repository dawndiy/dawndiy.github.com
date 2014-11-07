---
title: PyGtk学习笔记(7)–工具栏
author: DawnDIY
layout: post
permalink: /archives/336
categories:
  - PyGtk
  - Python
tags:
  - PyGtk
  - Python
  - Toolbar
---

在前面的Gtk学习中我们构造界面用到 Menu 菜单，菜单当然是让我们能方便的选择某一项功能，但是如果一个我们经常用到的菜单项在二级菜单下面，或者更深，这样一来就显得非常不方便啦。所以我们又有了更显而易见的控件– Toolbar （工具栏）。是的，工具栏通常把我们经常使用的按钮及其功能列出在一栏上面，这样就可以轻松通过鼠标的一次点击即可。就像IE、Firefox的前进和后退、地址栏、刷新等，这几个都是我们浏览网页最常用的几个功能，所以这写浏览器也会将这几个按钮列出在一栏里面供快捷使用。所以，如果你的软件功能繁多，但是想为用户提供几个常用到的快捷按钮，那就使用 Toolbar 吧。

## 一.介绍

一个简单的 Toolbar 就是这样的，如下图：[![][2]][2]

 []: http://www.dawndiy.com/wp-content/uploads/2012/09/2012-09-20-004607的屏幕截图.png

上面的 **工具栏** 中包含了4个按钮和一个分隔符，并且倒数第二个按钮是灰色的，不可用状态。OK，了解了上面的 Toolbar 长什么样，接下来我们就来实现它。



## 二.Toolbar（工具栏）

### Toolbar的继承关系：

    -- gobject.GObject
       -- gtk.Object
         -- gtk.Widget
           -- gtk.Container
             -- gtk.Toolbar

### Toolbar类的概要：

    class gtk.Toolbar(gtk.Container):
        gtk.Toolbar()
    
        def insert(item, pos)
    
        def get_item_index(item)
    
        def get_n_items()
    
        def get_nth_item(n)
    
        def get_drop_index(x, y)
    
        def set_drop_highlight_item(tool_item, index)
    
        def set_show_arrow(show_arrow)
    
        def get_show_arrow()
    
        def get_relief_style()
    
        def append_item(text, tooltip_text, tooltip_private_text, icon, callback, user_data=None)
    
        def prepend_item(text, tooltip_text, tooltip_private_text, icon, callback, user_data)
    
        def insert_item(text, tooltip_text, tooltip_private_text, icon, callback, user_data, position)
    
        def insert_stock(stock_id, tooltip_text, tooltip_private_text, callback, user_data, position)
    
        def append_space()
    
        def prepend_space()
    
        def insert_space(position)
    
        def remove_space(position)
    
        def append_element(type, widget, text, tooltip_text, tooltip_private_text, icon, callback, user_data)
    
        def prepend_element(type, widget, text, tooltip_text, tooltip_private_text, icon, callback, user_data)
    
        def insert_element(type, widget, text, tooltip_text, tooltip_private_text, icon, callback, user_data, position)
    
        def append_widget(widget, tooltip_text, tooltip_private_text)
    
        def prepend_widget(widget, tooltip_text, tooltip_private_text)
    
        def insert_widget(widget, tooltip_text, tooltip_private_text, position)
    
        def set_orientation(orientation)
    
        def set_style(style)
    
        def set_icon_size(icon_size)
    
        def set_tooltips(enable)
    
        def unset_style()
    
        def unset_icon_size()
    
        def get_orientation()
    
        def get_style()
    
        def get_icon_size()
    
        def get_tooltips()

###  构造方法：

    gtk.Toolbar()

*Returns* :

一个新的 Toolbar （工具栏）对象

### 常用方法：

### gtk.Toolbar.insert

    def insert(item, pos)

  **`item`** :

一个 `gtk.ToolItem` 对象

**`pos`** :

新项目的位置（0，1，2 …）

**注：**  
这个方法在PyGTK 2.4以及以上版本才可用。

通过 insert() 方法可以在工具栏中添加 ToolItem ，用 pos 来确定其位置，如果 pos 为0，则表示该 Item 在工具栏的起始位置。

### gtk.Toolbar.set_style

    def set_style(style)

**`style`** :

样式，包括：  `gtk.TOOLBAR_ICONS` （仅图标）, `gtk.TOOLBAR_TEXT`（仅文字）, `gtk.TOOLBAR_BOTH（全部）` or `gtk.TOOLBAR_BOTH_HORIZ（水平全部）`

通过过 `set_style`() 方法，我们可以为 Toolbar 设置显示样式。

### gtk.Toolbar.set\_icon\_size

    def set_icon_size(icon_size)

  **`icon_size`** :

图标的尺寸。

通过 `set_icon_size`() 方法可以设置显示在 Toolbar 上的图标的尺寸。icon_size 的值如下：

*   `gtk.ICON_SIZE_MENU`
*   `gtk.ICON_SIZE_SMALL_TOOLBAR`
*   `gtk.ICON_SIZE_LARGE_TOOLBAR`
*   `gtk.ICON_SIZE_BUTTON`
*   `gtk.ICON_SIZE_DND`, or
*   `gtk.ICON_SIZE_DIALOG`

## 三.完整代码

了解了上面的基本方法，我们就可以看实现前面的图的完整代码啦。

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    
    # Toolbar
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com
    
    import pygtk
    pygtk.require('2.0')
    import gtk
    
    class Toolbar:
    	def __init__(self):
    		self.win = gtk.Window(gtk.WINDOW_TOPLEVEL)
    		self.win.set_title("Toolbar")
    		self.win.set_size_request(300,250)
    		self.win.set_position(gtk.WIN_POS_CENTER)
    
    		toolbar = gtk.Toolbar()
    		#工具栏仅显示图标
    		toolbar.set_style(gtk.TOOLBAR_ICONS)
    
    		#工具栏图标的尺寸
    		toolbar.set_icon_size(gtk.ICON_SIZE_LARGE_TOOLBAR) 
    
    		#新建工具栏按钮
    		newtb = gtk.ToolButton(gtk.STOCK_NEW)
    		opentb = gtk.ToolButton(gtk.STOCK_OPEN)
    		sep = gtk.SeparatorToolItem()  #工具栏的分隔符
    		closetb = gtk.ToolButton(gtk.STOCK_CLOSE)
    		exittb = gtk.ToolButton(gtk.STOCK_QUIT)
    
    		exittb.connect("clicked", gtk.main_quit)
    
    		#设置CLOSE按钮不可用
    		closetb.set_sensitive(False)
    
    		#添加工具栏按钮
    		toolbar.insert(newtb, 0)
    		toolbar.insert(opentb, 1)
    		toolbar.insert(sep, 2)
    		toolbar.insert(closetb, 3)
    		toolbar.insert(exittb, 4)
    
    		vbox = gtk.VBox()
    		vbox.pack_start(toolbar, False, False, 0)
    		self.win.add(vbox)
    
    		self.win.connect("destroy", gtk.main_quit)
    		self.win.show_all()
    
    if __name__ == "__main__":
    	toolbar = Toolbar()
    	gtk.main()

按照我在代码中给出的注释就很容易懂啦。然后动手改一改 icon_size 、style 等等，你会找到你想要的效果的！打完手工，待续。。。。。。

 

 
