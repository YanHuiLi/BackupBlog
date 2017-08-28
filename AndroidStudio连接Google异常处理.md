---
title: AndroidStudio账户连接Google异常处理
date: 2016-11-21 15:32:44
tags: [异常处理]
categories: AndroidStudio
---



# AndroidStudio Google账号登陆异常

Access is allowed from event dispatch thread only.
当在android studio中出现这个异常的时候，我在网页端授权（xx-net）是成功的，但是回到studio里面，却出现这个异常，认真的看一下，大概意思是：
网页端的进程和studio端的进程不一致造成的这个异常。解决方式，在studio里面使用代理，上图：

![AndroidStudioCountExceptionSolve](http://ogtmd8elu.bkt.clouddn.com/AndroidStudioCountExceptionSolve-min.png)
