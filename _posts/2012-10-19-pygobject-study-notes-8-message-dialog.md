---
title: PyGObject 学习笔记(8)-消息对话框
author: DawnDIY
layout: post
permalink: /archives/378
categories:
  - PyGObject
  - Python
tags:
  - MessageDialog
  - PyGObject
  - PyGtk
  - Python
---
# 

写在前面，先扯点无聊的。这几天回家才有时间用电脑，刚好昨天升级了一下 Ubuntu 12.10 。然后当然也要更新一下学习笔记，学习在于坚持嘛~ 可能发现标题不同了，不错，我在前几天谈到我的学习心得和经历让我做出了选择，一定要向前，用GTK 3 所以选择PyGObject。可以看我写的[《PyGtk or PyGObject》][1]，也扯了一些有的没的。

 [1]: http://www.dawndiy.com/archives/373 "PyGTK or PyGObject"

好了，正题开始，今天在一点点的码自己的项目的时候要用到一个很重要的东西，那就是“消息对话框”，即 Message Dialog 。这个是什么了，如果你不知道那就快回火星吧！上图！

![][2]

 [2]: http://i.imgur.com/u9Xjl.png "Info_message_dialog"

对，就是上面这个图，你见过吧。消息对话框在程序中随处可见，他的作用就是提示或弹出一个信息让用户得知，所以消息对话框在交互方面和用户体验方面也是挺重要的。



在GTK 3中，构建消息对话框的类是**MessageDialog**。下面来看一下这个类：

    class Gtk.MessageDialog(parent=None, flags=0, type=Gtk.MESSAGE_INFO, buttons=Gtk.BUTTONS_NONE, message_format=None)

  **`parent`** :

父控件，一般是窗体

**`flags`** :

对话框标识，可以是 Gtk.DialogFlags.MODAL，`Gtk.DIALOG_DESTROY_WITH_PARENT` 或 0

**`type`** :

消息的类型，可以是 `Gtk.MESSAGE_INFO`, `Gtk.MESSAGE_WARNING`, `Gtk.MESSAGE_QUESTION` 或`Gtk.MESSAGE_ERROR`.

**`buttons`** :

可以使用预定义的按钮： `Gtk.BUTTONS_NONE`, `Gtk.BUTTONS_OK`, `Gtk.BUTTONS_CLOSE`, `Gtk.BUTTONS_CANCEL`, `Gtk.BUTTONS_YES_NO`, `Gtk.BUTTONS_OK_CANCEL 等
`

**`message_format`** :

消息的内容

*Returns* :

 `G``tk.MessageDialog`

上面就是构造消息对话框的 MessageDialog 类啦。

现在看一下我们最上面那个图的代码是怎么样的：

    dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.INFO,Gtk.ButtonsType.OK, "这是一个信息消息对话框")
    dialog.format_secondary_text("这里是副文本用于说明信息。")
    dialog.run()
    dialog.destroy()

这是一个消息对话框的部分代码。是不是很简单，然而上面多了几个函数下面解释：

    dialog.format_secondary_text("这里是副文本用于说明信息。")

这个看图也知道啊，就是现实在下面的副文本，可以用来显示解释性文字。

    dialog.run()
    dialog.destroy()

run执行后，对话框就会显示了，这时候对话框不会马上去执行后面的destroy来销毁，而是等待我们的一个按键响应再来销毁，所以这就是消息对话框有按钮的原因了。

上面的例子我们没有或得到按键响应，其实我们的按键响应就是由 **dialog.run()** 来返回的，所以中断在它这里嘛~那我们再看一个能获取按键的例子，惯例，上图先：

![][3]

 [3]: http://i.imgur.com/0qnC7.png "question_message_dialog"

看吧，这样的消息对话框就有两个按钮分别让你选择。看下局部代码：

    dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.QUESTION, Gtk.ButtonsType.YES_NO, "这是一个询问消息对话框")
    dialog.format_secondary_text("这里是副文本用于说明信息。")
    response = dialog.run()
    if response == Gtk.ResponseType.YES:
    	print("YES button is clicked")
    elif response == Gtk.ResponseType.NO:
    	print("NO button is clicked")
    dialog.destroy()

看到代码中我们定义了一个名为 response 的变量来接受 dialog.run() 的返回，然后再做出判断分别处理就OK啦。。。

好了，差不多就这些吧~大家可以试一试，我把写好的一个完整的 Demo 贴出来：

    #!/usr/bin/env python
    # -*- coding: utf-8 -*
    # MessageDialog
    # PyGtk Study Notes By DawnDIY
    # http://www.dawndiy.com 
    
    from gi.repository import Gtk
    
    class MessageDialogWindow(Gtk.Window):
    
    	def __init__(self):
    		Gtk.Window.__init__(self, title="MessageDialog Example")
    
    		box = Gtk.Box(spacing=6)
    		self.add(box)
    
    		button1 = Gtk.Button("信息")
    		button1.connect("clicked", self.on_info_clicked)
    		box.add(button1)
    
    		button2 = Gtk.Button("错误")
    		button2.connect("clicked", self.on_error_clicked)
    		box.add(button2)
    
    		button3 = Gtk.Button("警告")
    		button3.connect("clicked", self.on_warn_clicked)
    		box.add(button3)
    
    		button4 = Gtk.Button("询问")
    		button4.connect("clicked", self.on_question_clicked)
    		box.add(button4)
    
    	def on_info_clicked(self, widget):
    		dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.INFO,
    		Gtk.ButtonsType.OK, "这是一个信息消息对话框")
    		dialog.format_secondary_text(
    		"这里是副文本用于说明信息。")
    		dialog.run()
    		print "INFO dialog closed"
    
    		dialog.destroy()
    
    	def on_error_clicked(self, widget):
    		dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.ERROR,
    		Gtk.ButtonsType.CANCEL, "这是一个错误消息对话框")
    		dialog.format_secondary_text("这里是副文本用于说明信息。")
    		dialog.run()
    		print "ERROR dialog closed"
    
    		dialog.destroy()
    
    	def on_warn_clicked(self, widget):
    		dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.WARNING,
    		Gtk.ButtonsType.OK_CANCEL, "这是一个警告消息对话框")
    		dialog.format_secondary_text(
    		"这里是副文本用于说明信息。")
    		response = dialog.run()
    		if response == Gtk.ResponseType.OK:
    			print "OK button is clicked"
    		elif response == Gtk.ResponseType.CANCEL:
    			print "CANCEL button is clicked"
    
    		dialog.destroy()
    
    	def on_question_clicked(self, widget):
    		dialog = Gtk.MessageDialog(self, 0, Gtk.MessageType.QUESTION,
    		Gtk.ButtonsType.YES_NO, "这是一个询问消息对话框")
    		dialog.format_secondary_text(
    		"这里是副文本用于说明信息。")
    		response = dialog.run()
    		if response == Gtk.ResponseType.YES:
    			print "YES button is clicked"
    		elif response == Gtk.ResponseType.NO:
    			print "NO button is clicked"
    
    		dialog.destroy()
    
    win = MessageDialogWindow()
    win.connect("delete-event", Gtk.main_quit)
    win.show_all()
    Gtk.main()

效果自己去运行就知道了~~~OK~最近好忙啊，好不容易偷个懒上下网~~~下回继续…….

 

 

 

 