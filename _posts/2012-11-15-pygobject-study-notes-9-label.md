---
title: PyGObject 学习笔记(9)-标签
author: DawnDIY
layout: post
tags:
  - Label
  - PyGObject
  - PyGtk
  - Python
---

Label (标签)可以说是任何应用中最常见的控件了，使用标签是在窗口中显示不可编辑的文字的最常用方法。简单讲就是用来显示信息的。我们从最简单的 Hello World 例子看一下，如图：

![][1]

 [1]: http://i.imgur.com/BmAKT.png "Label_hello_world"

就像上面的图一样，就是显示一个简单的信息，完整代码如下：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Label Example
# PyGObject Study Notes By DawnDIY
# http://dawndiy.com

from gi.repository import Gtk

class LabelWindow(Gtk.Window):

    def __init__(self):
        Gtk.Window.__init__(self, title="Label Example")

        label = Gtk.Label("Hello World!")

        self.add(label)

win = LabelWindow()
win.connect("delete-event", Gtk.main_quit)
win.show_all()
Gtk.main()
```

是不是很简单，**但是，不是仅仅如此哦。**其实 Label 也有挺多显示方法的，如果选择合适的显示，会给你的窗口添加不少亮点，下面我们就详细看一下。



先从 Label 对象看起：

```python
class Gtk.Label([text ])
```

在新建一个 Label 时，可以直接附上 text 的内容即可，如果没有值即空。

常用方法：

```python
static new_with_mnemonic(text)
```

这是一个静态方法，返回的也是一个Label。但是有所不同的是，这个方法可以设置类似快捷键的事件，通过下划线(\_)来指定快捷键字母，当然这个方法要配合 set\_mnemonic\_widget(widget) 来使用，通过键盘上按下 Alt [指定字母] 来激活 set\_mnemonic_widget(widget) 绑定的控件的事件。详情看后面例子。

```python
set_mnemonic_widget(widget)
```

设置前面指定快捷键激活的控件事件。如果为空或者没有使用该方法，则默认为 Label 本身。

```python
set_justify(justification)
```

使用这个方法设置文字的对齐方式。

**`justification`**

可以是：Gtk.Justification.LEFT, Gtk.Justification.RIGHT, Gtk.Justification.CENTER,  
Gtk.Justification.FILL. 分别是左对齐，右对齐，居中和填充。不过这个方法对尽有一行文字的Label是无效的。

```python
set_line_wrap(wrap)
```

这个是控制内容换行的。当 *wrap* 的值为 True ，如果一行内容超过了 Label 控件的大小，那么将内容换行显示。当 *wrap* 的值为 False ，如果一行内容超过了 Label 控件的大小，内容将被剪切掉。

```python
set_markup(markup)
```

这个是好东西，可以让你的 Label 显示更加丰富，通过该方法让 Label 的内容支持标记输出，其中的标记必须符合 Pango 的文本标记语言，如, & 字符都要用 < > &amp 来替换。下面的完整例子中会用到。

```python
set_selectable(selectable)
```

这个方法设置文本内容是否可选择，*selectable *默认是 False，即不可选，为 Ture 时则可以供用户选择用户复制粘贴。

```python
set_text(text)
```

这个就不用多说了，看它就知道是干嘛的啦，使用它可以随时更改 Label 的内容。

好的，我们现在汇总一下看一个比较长的例子，上图先，呵呵：

![][2]

 [2]: http://i.imgur.com/4o88f.png "label_example"

这个例子包含了上面我讲到的常用方法，和一些 Label 的用法，看看完整的源代码和 Label 的内容你就懂啦~

完整代码：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# Label Example
# PyGObject Study Notes By DawnDIY
# http://dawndiy.com

from gi.repository import Gtk

class LabelWindow(Gtk.Window):

    def __init__(self):
        Gtk.Window.__init__(self, title="Label Example")
        hbox = Gtk.Box(spacing=10)
        hbox.set_homogeneous(False)
        vbox_left = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=10)
        vbox_left.set_homogeneous(False)
        vbox_right = Gtk.Box(orientation=Gtk.Orientation.VERTICAL, spacing=10)
        vbox_right.set_homogeneous(False)
        hbox.pack_start(vbox_left, True, True, 0)
        hbox.pack_start(vbox_right, True, True, 0)
        label = Gtk.Label("这是一个普通 label")
        vbox_left.pack_start(label, True, True, 0)
        label = Gtk.Label()
        label.set_text("这是一个左对齐的 label。n包含多行。")
        label.set_justify(Gtk.Justification.LEFT)
        vbox_left.pack_start(label, True, True, 0)
        label = Gtk.Label("这是一个右对齐的 label。n包含多行。")
        label.set_justify(Gtk.Justification.RIGHT)
        vbox_left.pack_start(label, True, True, 0)
        label = Gtk.Label("这是一个多行显示的 label 示例。它"
            "不是占据所有能容纳下它的"
            "宽度，而是自动的换行调整适应。n"
            "并且它支持多段落正确的显示，"
            "正确的补充额外的空间。")
        label.set_line_wrap(True)
        vbox_right.pack_start(label, True, True, 0)
        label = Gtk.Label("这是一个多行显示的 label 示例，填充式 label 。"
            "它会占据所有能容纳下它的宽度。 "
            "好，来几个句子证明我的说法。"
            "这又是一个句子。又来一个句子，巴拉巴拉巴拉。n"
            "这是一个新段落~n"
            "好吧，这又是一个扯淡的段落，扯点"
            "什么呢？元芳，你怎么看啊？呵呵~")
        label.set_line_wrap(True)
        label.set_justify(Gtk.Justification.FILL)
        vbox_right.pack_start(label, True, True, 0)
        label = Gtk.Label()
        label.set_markup("文本内容可以 小, 大, "
            "粗体, 斜体 甚至可以是超链接 "
            " 网络.")
        label.set_line_wrap(True)
        vbox_left.pack_start(label, True, True, 0)
        label = Gtk.Label.new_with_mnemonic("按下 Alt   P 来选择右边的按钮 (_P)")
        vbox_left.pack_start(label, True, True, 0)
        label.set_selectable(True)
        button = Gtk.Button(label="点一下试一试")
        label.set_mnemonic_widget(button)
        vbox_right.pack_start(button, True, True, 0)
        self.add(hbox)

window = LabelWindow()
window.connect("delete-event", Gtk.main_quit)
window.show_all()
Gtk.main()
```

好啦~ Label 挺简单的。。。 用好 mark 会表现的更出色的。 See you next time~

现在开始，之前的代码都传到 github 上去了。  
文中的例子在这里：  
[github](https://github.com/dawndiy/PyGTK-PyGObject-Study-Notes/blob/master/label.py)
