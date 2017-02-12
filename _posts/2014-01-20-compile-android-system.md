---
layout:     post
title:      "基于Chipsee资料编译Android系统"
subtitle:   "基于Chipsee提供的相关资料编译出Android 4.0.3文件系统，并移植到Pandaboard ES开发板上运行"
date:       2014-01-20 16:00
author:     "wzystal"
header-img: "img/post-bg-android-logo.jpg"
tags:
    - Android
---

> 本文旨在基于Chipsee提供的相关资料编译出Android 4.0.3文件系统，并移植到Pandaboard ES开发板上运行。

在编译之前，你需要先去Git网站上下载Andorid 4.0.3源码，由于时间问题具体的下载流程本文先不予描述，以后再写博客补上。So，现在我默认你已经拿到了一份Andorid 4.0.3的源码，接下来就等着开始我们的编译之旅了。
### 1. 搭建编译环境
这一步万分重要！！因为在编译过程中，我们遇到的很多错误往往就是编译环境的问题，比如说哪个需要的库没安装、或者安装了版本又不对等等（关于这些编译过程中的错误及解决方案，我会另外整理一篇博客供大家参考）。
关于编译环境，我建议大家参考官方给出的文档来搭建，链接地址如下：[http://source.android.com/source/initializing.html](http://source.android.com/source/initializing.html)。大家可以根据自己的具体情况，搭建好正确的编译环境。以我本人为例，我是在Ubuntu 12.04 LTS系统下进行编译的，系统自带的Python、GNU Make和Git都符合条件，所以不用再更新，只需安装其他的包即可：
Installing required packages (Ubuntu 12.04)  
```
$ sudo apt-get install git gnupg flex bison gperf build-essential \
  zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
  libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
  libgl1-mesa-dev g++-multilib mingw32 tofrodos \
  python-markdown libxml2-utils xsltproc zlib1g-dev:i386
$ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so
```
需要补充的一点是，官方文档中“Installing the JDK”部分的信息已经过期了（从"sun-java6-jdk"就可以看出端倪，SUN公司早在2009年就已经被ORACLE收购了，所以现在不可能还有"sun-java"的存在），我们需要通过其他方式来安装JDK6，大家可以参考我的另一篇博文《Ubuntu系统下安装和配置JDK 》来进行安装。

### 2. 交叉编译工具（toolchain）
搭建好本机的编译环境以后，我们需要下载对应的交叉编译工具，Chipsee提供的交叉编译工具为android-toolchain-eabi-linaro，官方提供的下载网址目前已经失效，有需要的朋友可以留言联系我。下载好交叉编译工具包以后，我们可以将其解压到适当的目录，然后在编译脚本中设置好环境变量，就可以在编译过程中使用了。在本例中，我直接将交叉编译工具解压到了Android 4.0.3源码的根目录下。

### 3. 编译Android文件系统
在确保完成了以上准备工作以后，我们就可以开始进行Android 4.0.3的编译了。
基于Chipsee提供的linaro_android_build_cmds.sh脚本，我自己改写了一个版本build.sh，具体代码如下：
```
#!/bin/bash 
export TARGET_PRODUCT=pandaboard
export TARGET_SIMULATOR=false
export CPUS=`grep -c processor /proc/cpuinfo`
export TARGET_TOOLS_PREFIX=android-toolchain-eabi/bin/arm-linux-androideabi-  #本例中，交叉编译工具就在源码根目录下，此处直接用了相对路径
# build the code
build/envsetup.sh
make -j${CPUS} HOST_CC=gcc-4.4 HOST_CXX=g++-4.4 HOST_CPP=cpp-4.4 boottarball systemtarball userdatatarball #此处需使用gcc-4.4/g++-4.4或者gcc-4.5/g++-4.5版本
```
将build.sh拷贝至在Android 4.0.3源码的根目录下，然后执行 sudo ./build.sh，编译工作就开始了，编译时间取决于机器的CPU执行效率，本人在双核CPU、8G内存的服务器上编译花费了10多分钟。编译过程中，可能会出现各种千奇百怪的问题，我个人总结了一些，后续会整理出来供大家参考。

### 4. 移植Android文件系统
编译过程顺利的话，源码的根目录下会生成out/target/product/pandaboard目录，该目录中包含有编译生成的Android文件系统：boot.tar.bz2、system.tar.bz2、userdata.tar.bz2。 
#### 4.1 boot文件的移植
编译完成以后，我们可以直接通过Chipsee提供的制作脚本mksdcard-chipsee-ics.sh（做卡过程中需要注意system目录的权限问题），将编译生成的文件系统写入SD卡中，然后将SD卡插到Pandaboard ES开发板上，在开发板上启动编译好的Android 4.0.3系统。通过minicom超级终端，我们可以看到，虽然系统可以启动成功，但是Pandaboard扩展板的LCD屏幕上没有任何系统界面输出。这是因为通过上述步骤编译的uImage(在boot.tar.bz2中)并未对Pandaboard ES的LCD屏幕驱动做移植，我们需要基于Chipsee提供的Kernel资料，编译出支持Pandaboard ES屏幕的uImage，同时为了保证与新uImage的兼容，我们需要使用Chipsee提供的MLO、u-boot.img和boot.scr文件。
#### 4.2 system文件的移植
除了boot文件的移植，Chipsee也对system目录下的某些文件进行了移植，移植文件列表如下：
```    
/system/vendor/
/system/bin/pvrsrvinit
/system/usr/keylayout/Generic.kl (system partition, ASCII)
/system/usr/idc/ads7846.idc (system partition, ASCII)
/system/usr/idc/ft5x0x.idc (system partition, ASCII)
/system/lib/hw/sensors.omap4.so (system partition, Binary only)
/init.omap4pandaboard.rc (Ramdisk, ASCII)
/init.chipsee.sh (Ramdisk, ASCII)
```
针对上述文件，Chipsee为我们提供了移植好的版本，我们只需一一对应替换掉编译版本中的文件即可。

#### 5. 启动Android系统
完成移植工作以后，重新制作SD卡并通电启动Pandaboard开发板，就可以成功启动自己编译好的Android系统，并能在扩展板的LCD屏幕上看到梦寐以求的系统界面了！


