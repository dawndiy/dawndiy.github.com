---
title: 'PyGObject 学习笔记(10)-GMenu &#038; GSimpleAction'
author: DawnDIY
layout: post
permalink: /archives/442
categories:
  - PyGObject
  - PyGtk
  - Python
tags:
  - GMenu
  - GSimpleAction
  - PyGObject
  - PyGtk
keywords: PyGObject, PyGtk, GMenu, GSimpleAction, 学习笔记
---
# 

有段时间没更新了，一直想着想重新改一下Bolg，所以没怎么写了。好~二话不说，现在继续。今天来讲一个新的控件，一看标题，对！又是菜单，但这次菜单和之前的[《PyGtk学习笔记(6)–菜单》][1]的菜单是不同，自从转向学习PyGObject了，会有很多GTK 3的特性，所以，这次的 GMenu 和以往的菜单不同。所以老规矩，看图吧。

 [1]: http://www.dawndiy.com/archives/290 "PyGtk学习笔记(6)–菜单"

![][2]

 [2]: http://i.imgur.com/sNe0XA5.jpg "GMenu"

上面的图就是 GMenu 在原生 Gnome3 桌面里面显示的效果。好了，开始介绍。

## 一. 介绍

Ubuntu 的 Unity 桌面使用了全局菜单，为应用程序增加了很多可视空间。当然，在 Gnome3 里面同样可以省去传统的 MenuBar ，根据 Gnome3 的新特性，增加了 GMenu 。就像上面的图一样，将菜单功能集成在 Gnome 的任务栏中，同样为用户挤出了更多的空间，而且现在很多 Gnome3 的应用也开始使用这一特性了，如：Empathy。当然，刚开始很多童鞋找不到菜单在最顶上，呵呵，不过发现了以后还是会比较惊喜，这样会省出一个 Bar 的空间。

### 1.类结构
```
    GObject
        ----GMenuModel
              ----GMenu
```
要注意的是，GMenu 是在 GIO 中，而不是在 GTK 中，所以应该导入 Gio 模块
<!-- more -->

``` python
    from gi.repository import Gio
```
###  2.常用方法
``` python
    insert(position, label, detailed_action)
    append(label, detailed_action)   # 加到最后
    prepend(label, detailed_action)  # 加到最前
```
在菜单中插入一个选项  
*position: 位置*  
*label: 显示标签*  
*detailed_action: 动作名，以 app. 为前缀*
``` python
    insert_section(position, label, section)
    append_section(label, section)
    prepend_section(label, section)
```
在菜单中插入一个区域，上下会有分割线隔开  
*position: 位置*  
*label: 区域名*  
*section: 可以是一个 GMenu*
``` python
    insert_submenu(position, label, section)
    append_submenu(label, section)
    prepend_submenu(label, section)
```
在菜单中插入一个子菜单  
*position: 位置*  
*label: 区域名*  
*section: 可以是一个 GMenu*
``` python
    remove(postion)    # 删除选项
    set_label(label)   # 设置显示标签
```
###  2.代码分析

以下是是想最上面图中的菜单的效果：
``` python
    #!/usr/bin/env python
    # -*- coding: utf-8 -*-
    
    # GMenu & SimpleActions
    # PyGObject Study Notes By DawnDIY
    # http://dawndiy.com
    
    from gi.repository import Gtk
    from gi.repository import Gio
    import sys
    
    class MyWindow(Gtk.ApplicationWindow):
        def __init__(self, app):
            Gtk.Window.__init__(self, title="GMenu Example", application=app)
    
    class MyApplication(Gtk.Application):
        def __init__(self):
            Gtk.Application.__init__(self)
    
        def do_activate(self):
            win = MyWindow(self)
            win.show_all()
    
        def do_startup (self):
            # 启动应用
            Gtk.Application.do_startup(self)
    
            # 建立顶层菜单
            menu = Gio.Menu()
            # 新建三个菜单选项
            item_new = Gio.MenuItem.new("New", "app.new")
            item_about = Gio.MenuItem.new("About", "app.about")
            item_quit = Gio.MenuItem.new("Quit", "app.quit")
    
            # 建立菜单，作为子菜单
            submenu = Gio.Menu()
            # 直接添加2个选项
            submenu.append("sub_New", "app.new")
            submenu.append("sub_About", "app.about")
    
            # 建立菜单，作为父菜单
            menu2 = Gio.Menu()
            # 将子菜单添加进来
            menu2.append_submenu("Sub", submenu)
            menu2.append("exit", "app.quit")
    
            # 把选项、父菜单都加入到顶层菜单中
            menu.append_item(item_new)
            menu.append_item(item_about)
            menu.append_section("", menu2)  # 父菜单添加为不同菜单区域
            menu.append_item(item_quit)
    
            # 将顶层菜单作为应用程序的菜单
            self.set_app_menu(menu)
    
            # 为菜单 new 选项添加动作
            new_action = Gio.SimpleAction.new("new", None)
            # 连接到回调函数 new_cb
            new_action.connect("activate", self.new_cb)
            # 将动作添加到应用中 
            self.add_action(new_action)
    
            # about 选项
            about_action = Gio.SimpleAction.new("about", None)
            about_action.connect("activate", self.about_cb)
            self.add_action(about_action)
    
            # quit 选项 
            quit_action = Gio.SimpleAction.new("quit", None)
            quit_action.connect("activate", self.quit_cb)
            self.add_action(quit_action)
    
        # new 的回调函数 
        def new_cb(self, action, parameter):
            print("This does nothing. It is only a demonstration.")
    
        # about 的回调函数 
        def about_cb(self, action, parameter):
            print("No AboutDialog for you. This is only a demonstration.")
    
        # about 的回调函数 
        def quit_cb(self, action, parameter):
            print("You have quit.")
            self.quit()
    
    app = MyApplication()
    exit_status = app.run(sys.argv)
    sys.exit(exit_status)
```
代码中我已经注释的很详细了，相信认真看都能看懂的！下面我就补充一下前面提到的 GSimpleAction 。

GSimpleAction 继承自 GObject， 用于建立一个独立的动作，使用它可以连接你的回调函数。

常用函数有：
``` python
    action = Gio.SimpleAction.new("name", parameter_type)
```
*新建一个动作*  
*name: 动作名*  
*parameter_type: 参数类型，可以为 None*
``` python
    set_enabled(True)
```
*设置是否启用*

### 3.总结

好吧，又写了一篇。希望感兴趣的朋友们能一起来交流！我刚看到 GMenu 的时候是用 Empathy 的时候发现它的菜单变化了，不过第一次用确实没找到菜单，呵呵，最后才知道这样的设计其实挺好的，节省了可视空间，尤其是在你的应用有很复杂的UI的时候，最后还是谈到用户体验了。

PS：这次我学 GMenu 的时候其实我也找了很多资料，不过 PyGObject 的资料不多，最后还是拿着 C 的 GTK 开发手册（就是那个devhelp）一个个函数看的。所以，还是要多看看 API 文档。

待续。。。

 
