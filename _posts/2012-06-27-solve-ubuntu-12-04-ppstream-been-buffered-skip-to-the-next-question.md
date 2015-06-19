---
title: 解决Ubuntu12.04下PPStream一直缓冲后跳到下一个问题
author: DawnDIY
layout: post
tags:
  - Linux
  - PPS
  - Ubuntu
---

首先吐槽一下PPS好不容易弄了一个Linux版，但各种古怪的问题。但是还是要鼓励一下PPS，开辟了Linux的市场，使得Linux桌面用户，尤其是国内Linux用户，终于有了可以使用的网络电视软件。


最近重新装了一下系统，顺带把PPS也装一下，然后发现装好以后PPS出现了奇怪的现象，就是播放的时候缓冲完毕后就跳到下一个继续缓冲，终而复始。以前装的时候没有出现这个问题啊，不过现在装的是Ubuntu12.04，之前是从11.10升级到12.04的PPS没有问题，然而现在直接在12.04上安装就出现问题了。

原因是在Linux桌面系统中PPS调用的是系统的MPlayer用于播放解码，和windows版本的PPS调用windows media player同一个道理。然而在Ubuntu12.04中MPlayer升级到了1.0~rc4版本，之前在Ubuntu11.10中MPlayer是1.0~rc3的，而且PPS也是基于1.0~rc3的，那么要把MPlayer降级吗？不需要，后来发现只是少了一个包： libjpeg62

```bash
sudo apt-get install libjpeg62
```

直接安装好后PPS就可以正常播放了！^_^
