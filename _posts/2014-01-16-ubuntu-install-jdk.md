---
layout:     post
title:      "Ubuntu系统下安装和配置JDK"
subtitle:   "如何在Ubuntu下，根据自己的需要来安装和配置JDK"
descripotion: "如何在Ubuntu下，根据自己的需要来安装和配置JDK"
date:       2014-01-16 21:05
author:     "wzystal"
header-img: "img/post-bg-ubuntu-logo.jpg"
tags:
    - Ubuntu
    - Java
---

> 目前Ubuntu12.04系统自带的JDK版本为OpenJDK，但有些时候我们需要用到ORACLE官方提供的JDK版本（比如编译Android文件系统），所以我们需要对其进行修改。本文主要为大家介绍Ubuntu下安装和配置JDK的一种常见方法。
     
废话不多说，上具体步骤：
1. 去ORACLE官网下载你想要安装的JDK版本（该版本还得与你当前使用的操作系统及位数相匹配）
官方链接如下：[www.oracle.com/technetwork/java/javase/downloads/index.html](www.oracle.com/technetwork/java/javase/downloads/index.html) 。
以Ubuntu12.04 64位系统下编译Android4.0.3文件系统为例，我们需要使用Linux x64的JDK6版本，官方链接：[www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR](www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase6-419409.html#jdk-6u45-oth-JPR)，在该页面选择jdk-6u45-linux-x64.bin进行下载（注：现在ORACLE官网下载JDK版本需要登录ORACLE帐号）。

2. 下载好对应的JDK版本以后，需要对其中的JDK文件进行提取，然后复制到合适的目录（建议放到/usr/local/目录下，该目录一般用于存储手动安装的程序）。
 对于JDK7版本（tar.gz文件），可直接解压得到JDK文件；
 对于JDK6版本（bin文件），需要赋予jdk-6u45-linux-x64.bin执行权限，然后执行该文件来得到JDK文件：
```
$ sudo chmod +x  jdk-6u45-linux-x64.bin  
$ sudo ./jdk-6u45-linux-x64.bin
```
得到JDK文件以后即可复制到相关目录：`$ sudo cp -r jdk1.6.0_45 /usr/local/`

3. 配置环境变量（建议在用户级环境变量中进行配置，这样不会对其他用户造成影响）。
打开当前用户配置文件：`$ sudo vi ~/.profile`   
在配置文件末尾加上：`export JAVA_HOME=/usr/local/jdk1.6.0_45`  
保存并关闭配置文件，使用source命令更新一下即可：`$ source ~/.profile`  
使用env命令可以查看所有的环境变量，输入$ env ，如果结果中有JAVA_HOME=/usr/local/jdk1.6.0_45，说明配置成功！
     
4. 修改系统的默认JDK版本为当前版本。
```
$ sudo update-alternatives --install /usr/bin/java java /usr/local/jdk1.6.0_45/bin/java 300  
$ sudo update-alternatives --install /usr/bin/javac javac /usr/local/jdk1.6.0_45/bin/javac 300  
$ sudo update-alternatives --config java   
$ sudo update-alternatives --config javac  
```
5. 修改完毕以后，在终端输入：java -version，出现对应的Java版本信息，说明JDK的安装和配置成功了！
以jdk1.6.0_45为例：     
```
java version "1.6.0_45"  
Java(TM) SE Runtime Environment (build 1.6.0_45-b06)  
Java HotSpot(TM) 64-Bit Server VM (build 20.45-b01, mixed mode)  
```