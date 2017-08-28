---
title: Android核心基础开山篇（一）
date: 2016-11-21 15:29:57
tags: [Android基础]
categories: Android
---


# Android 核心基础

1G-4G的发展起源

G：generation —》大哥大（信号不好，无法短信，土豪可以使用）

2G: 小灵通 gsm —》最早是是美国军方的一种通讯标准，可打电话和发短信。

3G： 联通 沃 最高速度 7.2M/s。

4G： lte 通讯革命 100M/s。

5G： 华为投资6亿美金 最高传输速度 10G/s。

小公司卖产品，大公司卖版权（标准）。

# Android系统介绍

Android并非是Google的原创产品，而是由Andy Robin发明，于2005年被google收购。

T-mobile G1 是第一款搭载Android系统的只能手机，目前Android的市场占有率应该是目前最高的，大概在60%-70%之间。

Android进化史

# Androdi体系架构

由上到下：

应用层
Application frameWork 应用的框架层
函数库层 （c或者c++编写）
linux

# jvm和dvm的区别
dvm把所有的class文件变成了一个dex文件，传输的速度大大加快。
基于的架构不同，jvm基于栈，dvm基于寄存器。
由于版权的原因，Google不得不自己开发一个dvm。

# ART模式

ART模式：Android Runtime 的简称。4.4以后才有ART模式，运行的更流畅，更快。

ART模式之所以比Dalvik模式更快，是因为ART模式有一个预编译的过程，但是因此要更费一些内存空间。

# 开发环境

*  Android studio
* Eclipse（google已经停止支持）
* 搭建完成的时候测试一下的代码

# java -version
javac
adb

# SDKManager介绍

dx.bat–》 把所有的.class文件变成dex文件
adb 调试工具
intel：生成CPU 主要针对 pc机器或者笔记本
arm 生成标准，cpu的体系架构 x86的速度是最快的
# 真机调试
第一次真机调试的话，通过第三方软件（手机助手），给你自动匹配驱动，才能进行真机调试。

# 模拟器的简介

2.3—》API 10
3.0—》API 11
4.0—》API 14
…..

#Android 常见分辨率

320*480
480*800
1280*720
ROM与RAM

ROM：只读存储器，相当于电脑的一块微小的硬盘，断电以后数据不丢失。
RAM：相当于电脑的内存条，断电数据丢失
