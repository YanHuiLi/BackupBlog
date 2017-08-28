---
title: AndroidStudio——Freeline的使用
date: 2016-11-26 10:56:49
tags: [Freeline,技巧]
categories: AndroidStudio
top: 30
---

---
Freeline是蚂蚁金服旗下一站式理财平台蚂蚁聚宝团队在Android平台上的量身定做的一个基于动态替换的编译方案，稳定性方面：完善的基线对齐，进程级别异常隔离机制。
性能方面：内部采用了类似Facebook的开源工具buck的多工程多任务并发思想, 并对代码及资源编译流程做了深入的性能优化。
<!--more-->

# Freeline是什么？

Freeline是蚂蚁金服旗下一站式理财平台蚂蚁聚宝团队15年10月在Android平台上的量身定做的一个基于动态替换的编译方案，5月阿里集团内部开源，稳定性方面：完善的基线对齐，进程级别异常隔离机制。性能方面：内部采用了类似Facebook的开源工具buck的多工程多任务并发思想：端口扫描，代码扫描，并发编译，并发dx，并发merge dex等策略，在多核机器上有明显加速效果，另外在class及dex,resources层面作了相应缓存策略，做到真正增量开发，另外引入并优化buck的部分加速组件dx，DexMerger，资源编译方面，深入改造了Aapt资源编译流程，当资源发生改变时候，秒级完成增量包编译，其中增量包仅含最小的变更集合（10Kb～数百Kb内），后期也被运用到线上进行资源/代码动态替换。相比目前instant-run，buck，layoutcast等方案快数倍速度。

**简单的说：Freeline是一个能让你快速调试android程序的插件，速度有多快呢？官方的说法是可以让你的APP编译加速十倍，很吸引人吧，听说比jrebel还快，不过我没用过jrebel这个付费的产品。**

# Freeline源码

[Freeline源码](https://github.com/alibaba/freeline?spm=5176.100239.blogcont59122.9.V9lOHm "Freeline源码")

# Freeline原理

[Freeline原理](https://yq.aliyun.com/articles/59122?spm=5176.8091938.0.0.1Bw3mU "Freeline原理")

# 如何使用Freeline

1. 先介绍官方的说法。
2. 爬坑之旅

## 配置build.gradle文件

配置project-level的build.gradle，加入freeline-gradle的依赖：（注意以最新版本为准）

```javascript
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.antfortune.freeline:gradle:0.8.2' // 最新版本为准
    }
}
```

然后，在你的主module的build.gradle中，应用freeline插件的依赖：
```javascript
apply plugin: 'com.antfortune.freeline'

android {
    ...
}
```

## 执行运行代码

* Windows[CMD]: gradlew initFreeline -Pmirror
* Linux/Mac: ./gradlew initFreeline


对于国内的用户来说，如果你的下载的时候速度很慢，你也可以加上参数，执行gradlew initFreeline -Pmirror，这样就会从国内镜像地址来下载。

## 安装插件

Freeline最快捷的使用方法就是直接安装Android Studio插件。

在Android Studio中，通过以下路径Preferences → Plugins → Browse repositories，搜索“freeline”，并安装。
![687474703a2f2f7777342e73696e61696d672e636e2f6c617267652f3635653466316536677731663832656b6e616575646a3230746b30316f6d78652e6a7067-min.jpg](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/48cec4bfd819e6de22d701e6be6ace11.jpg)

## 实例
```
git clone git@github.com:alibaba/freeline.git
cd freeline/sample
./gradlew initFreeline   //mac
gradlew initFreeline -Pmirror  //windows
python freeline.py
```



# 爬坑之旅
以上官方给出的介绍和使用，当你真正开始使用的时候，坑就来了，不要紧，勇敢的踏过去。

## 找不到gradlew命令
如果你是windows平台，你打开cmd你会发现，当你输入gradlew initFreeline的时候，你会发现根本没有gradlew命令，那么官方是说毛线。

    'gradlew' 不是内部或外部命令，也不是可运行的程序
    或批处理文件。

**解决：看看代码的意思，官方的意思是要你初始化Freeline，那么gradlew这个命令在哪呢？打开你的项目的根目录，你就会发现gradlew文件，所以其实打开cmd以后，你还必须找到根目录再去运行gradlew initFreeline，其实你会发现在studio的Terminal里面，已经默认找到根目录，因此只需要在里面键入gradlew initFreeline命令行即可**

## gradlew initFreeline命令执行的太慢

当你运行gradlew initFreeline的时候，有时候会卡住，要求你下载完整的gradle的包
`
C:\Users\yourname\.gradle\wrapper\dists
`
![如图.PNG](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/6354dc3264cf2786bb50673fca9a4693.PNG "如图.PNG")

问题的关键就是在于，你在下载gradle包的时候被防火墙墙了。

注意一下你的studio里面用到的gradle，使用本地的：

![本地gradle.PNG](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/682a4ba47520288fa7abd8a28fbc6661.PNG "本地gradle.PNG")

**解决：去单独下载一个完整的gradle-2.14.1-all包**
[gradle-all 包下载地址](https://services.gradle.org/distributions "gradle-all 包下载地址")

自备梯子，或者是其他途径，总之你需要一个完整的包。
扔进2.14.1-all的根目录里面
![如图所示.PNG](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/065931da096b5bb1cea5021f9c2c237d.PNG "如图所示.PNG")

再次运行gradlew initFreeline -Pmirror命令，它自己首先会解压缩完整的gradle包，然后稍等一下，它会自己下载需要的所有插件，我在我的项目里面的OkHttp_Proj示范，结果如下：
![如图所示.PNG](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/f0e81f8671dbd6c68c6672ea832c8485.PNG "如图所示.PNG")


## Python 下载与配置

当我们的gradlew initFreeline 命令运行完毕以后，我们还需要下载python环境，因为脚本语言是用python写的。

[Python下载](https://www.python.org/downloads/ "Python下载")

同样是自备梯子，或者其他地方 下载
在这我建议直接装进C盘，然后添加环境变量。
打开你的cmd输入以下代码：

```
PATH=PATH;c:\python27  //根据你下载的版本判断
```

成功以后，cmd键入Python，会出现相关的版本信息。
如果你原来电脑上没有Python环境的话，Python安装配置以后，一定要重启studio，不然无法识别python关键字。



## 运行freeline.py文件

记住了：
*  安装了Python以后一定要重启studio。
*  同样是在项目的根目录以下运行  python freeline.py 代码。

![如图所示.PNG](http://ogtmd8elu.bkt.clouddn.com/2016/11/26/d7719fae6c090dccd3615d797a25b39d.PNG "如图所示.PNG")


# 建议


我建议最后再安装freeline插件，至少你先搞清楚，每一步的实现的机制，再使用freeline，会清晰很多。

最后这些环境都配置好了以后，下一次项目就不用那么麻烦了，直接点击freeline的那颗小按钮，它会自动给你添加各种配置并且自动运行的。

# 补充

## 下载freeline.zip太慢.

当我们配置好stuio以后，打开cmd找到对应项目的根目录以后，执行
```python
    gradlew initFreeline -Pmirror 
```
会发现一直卡在build构建的那里，看一眼提示，显示的正在下载freeline.zip，到根目录下，分析原因就是因为网络的问题，无法下载freeline.zip.我们通过dos下面，想做的无非就是下载这个freeline.zip的压缩包，然后解压进根目录完成快速编译。

**解决之路**：
打开浏览器，推荐使用chrome，打开
http://static.freelinebuild.com/freeline/0.8.4/all/freeline.zip
最新版本号为准，下载该文件，再到对应根目录下解压。

1. 如果再次执行gradlew initFreeline命令，还是会重新下载一个新的freeline.zip包，但是我们已经得到的是已经下载完毕并且解压完毕的了，因此我们一看根目录，已经有了我们想要执行的freeline.py的python文件，所以直接执行第二步： ``` python freeline.py```。

2. 却以外发现失败了，看一眼错误提示：

*  没有发现一个叫作freeline_project_description.json的文件，freeline很温馨的给出了解决的办法。


**解决：**执行下面的命令
*  Windows[CMD]: gradlew checkBeforeCleanBuild
*  Linux/Mac: ./gradlew checkBeforeCleanBuild

执行完毕以后，你们发现最后出现一个，
之前缺失的那个json文件保存进了根目录，而且已经构建成功了，也就是说，我们自己下载解压进根目录的freeline.zip包生效了

BUILD SUCCESSFUL 这行命令就是initFreeling最后成功的命令。

接下来再次执行 ```python freeline.py``` 构建就成功了。

**Enjoying it**








