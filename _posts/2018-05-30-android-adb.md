---
layout:     post  
title:      "ADB实用命令大全"  
subtitle:   "ADB实用命令大全"  
description: "汇总安卓ADB中那些实用命令"
date:       2018-05-30 11:07  
author:     "wzystal"  
header-img: "img/post-bg-android-logo.jpg"  
tags: [Android]
---



### 清除应用数据
> adb shell pm clear com.xx.yy

### 查看前台Activity
> adb shell dumpsys activity activities | grep ResumedActivity

### 屏幕截图
> adb shell screencap -p /sdcard/xx.png

### 录制屏幕
> adb shell screenrecord --bit-rate 16m /sdcard/test.mp4

### 传输文件到电脑
> adb pull /sdcard/xx ~/tmp，不指定电脑路径的话，默认传输到当前目录

### 传输文件到手机
> adb push xx /sdcard/

### 列出所有应用
> adb shell pm list packages，-s 系统应用，-3 三方应用

### 向指定receiver发送广播
> adb shell am broadcast -a android.intent.action.xxx -n com.xx.yy/.ZZReceiver

### 强制停止应用
> adb shell am force-stop com.xx.yy

### 查看设备信息
> adb shell getprop ro.product.model

### 查看系统属性
> adb shell cat /system/build.prop

### 查看系统版本
> adb shell getprop ro.build.version.release

### 查看屏幕分辨率
> adb shell wm size

### 查看屏幕密度
> adb shell wm density