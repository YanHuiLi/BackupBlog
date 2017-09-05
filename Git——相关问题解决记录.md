---
title: git——相关问题解决记录
date: 2016-12-18 11:50:49
tags: [git]
categories: git
---

---
收集项目中遇到的git提交代码的相关问题
<!--more-->



### androidStudio提交代码异常

当我用git提交代码的时候，因为卡住了，所以强行退出以后，下一次打开再次提交代码的时候，抱了以下的异常

 ```Git - fatal: Unable to create '/path/my_project/.git/index.lock': File exists.```

 **解决：**

 找到项目的根目录，执行以下代码：

 ```rm -f ./.git/index.lock```



### warning: LF will be replaced by CRLF

解决办法：

```bash
git config --global core.autocrlf  false
```

