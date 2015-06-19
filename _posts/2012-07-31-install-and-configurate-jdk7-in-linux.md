---
title: Linux 下安装配置 JDK8
author: DawnDIY
layout: post
tags:
  - Java
  - JDK
  - Linux
  - 开发
---

自从从Oracle收购Sun近三年来，已经有很多变化。早在8月，甲骨文将“Operating System Distributor License for Java”许可证终结，这意味着第三方将不可以依据这一许可分发他们的软件包。  
　　因此Ubuntu Linux已经开始禁用所有机器上的Oracle JDK浏览器插件，并很快会从档案中删除软件包。  
公司指出，禁用Oracle的插件将可以帮助提高安全性，因为这些插件已经被证实包含许多漏洞，虽然这是一个事实，但真正的原因恐怕是Sun的 JDK在升级时会清理掉用户机器上自认为不安全的软件，大多数PC用户认为这样很安全，但通常基于UNIX系统的用户并不这么认为。  
Oracle的JDK被废弃后，OpenJDK将取代它的位置在Ubuntu及其它Linux中默认安装。

虽然很多Linux发行版现在已经自带OpenJDK，但是在开发过程中与Oracle-JDK(SUN-JDK)还是略有不同。通常，Java开发人员还是以Oracle-JDK为标准来进行开发。  
下面介绍一下Linux下的JDK安装与配置，这里使用的Linux发行版是Ubuntu 12.04。



## 1.下载JDK

目前最新的JDK版本是：Java SE Development Kit 8u20

下载地址：[http://download.oracle.com/otn-pub/java/jdk/8u20-b26/jdk-8u20-linux-x64.tar.gz]

查看最新：[http://www.oracle.com/technetwork/cn/java/javase/downloads/index.html]

## 2.解压安装

我们把JDK安装到这个路径：/usr/lib/jvm  
如果没有这个目录（第一次当然没有），我们就新建一个目录

```bash
cd /usr/lib
sudo mkdir jvm
```

建立好了以后，我们来到刚才下载好的压缩包的目录，解压到我们刚才新建的文件夹里面去，并且修改好名字方便我们管理

```bash
sudo tar zxvf ./jdk-8u20-linux-x64.tar.gz  -C /usr/lib/jvm
cd /usr/lib/jvm
sudo mv jdk1.8.0_20/ jdk8
```

##  3.配置环境变量

```bash
gedit ~/.bashrc
```

在打开的文件的末尾添加

```bash
export JAVA_HOME=/usr/lib/jvm/jdk8
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

保存退出，然后输入下面的命令来使之生效

```bash
source ~/.bashrc
```

##  4.配置默认JDK

由于一些Linux的发行版中已经存在默认的JDK，如OpenJDK等。所以为了使得我们刚才安装好的JDK版本能成为默认的JDK版本，我们还要进行下面的配置。  
执行下面的命令：

```bash
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk8/bin/java 300
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk8/bin/javac 300
```

 注意：如果以上两个命令出现找不到路径问题，只要重启一下计算机在重复上面两行代码就OK了。

执行下面的代码可以看到当前各种JDK版本和配置：

```bash
sudo update-alternatives --config java
```

##  5.测试

打开一个终端，输入下面命令：

```bash
java -version
```

显示结果：

```bash
java version "1.8.0_20"
Java(TM) SE Runtime Environment (build 1.8.0_20-b26)
Java HotSpot(TM) 64-Bit Server VM (build 25.20-b23, mixed mode)
```

这表示java命令已经可以运行了。

 
