---
title: Git实用教程
date: 2017-08-05 19:07:14
tags: [git]
categories: git
top: 
---

---

写git系列的文章，我注重于实用的教程，解决一些实际的问题。

git是现在最先进，流行的分布式版本管理系统，毫无疑问是有着更多的的

<!--more-->

### git安装



[git for Windows下载地址](https://git-scm.com/downloads)

### 配置你的git

安装完成以后，在你的电脑的空白位置，或者是文件夹里面， 点击你鼠标的右键会看到

`git Bash`

`git GUI`

git Bash就是直接用命令行的方式去操作git，git GUI就是图形化的方式去操作，在这里我们建议使用git bash的方式，因为本质上是一样的，使用命令行的方式来操作的话，更geek。

接下来我们就需要配置个人信息，输入(一行一行的输入)：

```bash
$ git config --global user.name "xxxxxx"

$ git config --global user.email "xxxxx@xxx.xxx"
```



使用global配置以后，所有的项目均使用此身份进行相关操作。配置完成以后，你可以使用如下命令查看你配置的个人信息。

```bash
$ git config --global user.name

$ git config --global user.email
```

这样的话，git就配置好了。

### 初始化版本库

我们就在桌面开始初始化我们的版本库，在桌面的空白位置，点击 右键 ->git Bash ，命令框就出来，依次输出一下命令。

```bash
$ mkdir learngit
$ cd learngit
$ pwd
```

细心的你，一定发现了，在你的桌面上，多了一个learngit文件夹，没错喔，`mkdir`的作用就是创建一个文件夹，`cd`的命令就是进入到跟随的这个文件夹里面，相当于是双击进入了learngit文件夹。

`pwd`的命令就是显示出当前git命令行的根目录是在哪。

下一步，正式开始初始化我们的第一个版本库了。

```bash
$ git init
Initialized empty Git repository in C:/Users/Archer/Desktop/learngit/.git/
```

git很人性化，给了一句提示，已经初始化一个空的git repository在根目录下。

好奇的你，一定会想着进去看看这个repository在哪，点进去以后，发现并没有，其实并不是没有创建，而是被隐藏了。这里就要介绍一下下一个命令了

```bash
$ ls -la
total 16
drwxr-xr-x 1 Archer 197609 0 8月   5 11:54 ./
drwxr-xr-x 1 Archer 197609 0 8月   5 11:52 ../
drwxr-xr-x 1 Archer 197609 0 8月   5 11:54 .git/
```

`ls -la` 命令的意思就是列出该文件夹下的所有文件。如果你想删除本地的repository，只需要删除`drwxr-xr-x 1 Archer 197609 0 8月   5 11:54 .git/`这个隐藏文件即可。

到这一步，初始化版本库就算完成了。

### 提交文件



第一步，创建添加一个ReadMe.txt文件。

使用add命令添加进版本库

```bash
$ git add ReadMe.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

如果放进去的文件很多的话，那岂不是要添加多少次了。所以执行的是：

```bash
$ git add .
```

`add .`注意有个点。

第二步，使用`git commit - m "wrote a ReadMe.txt"`

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

```bash
$ git log 
```

`git log`命令可以查看你提交的log的信息，

### git连接Github

1. 新建一个代码库
2. 在本地的库与之关联    `git remote add origin git@github.com:YanHuiLi/FirstCodeAndroid.git` 
3. 在确保工作区clean的情况下 执行 `git push origin master`
4. git pull origin master







### 删除.git文件

`find . -name ".git" | xargs rm -Rf`

使用这个命令以后，就相当于删除了git对文件的跟踪，如果需要重新git,重新初始化add，commit即可


### 添加.gitignore文件

因为是在windows平台下，无法通过新建文件的形式解决，所以要先打开git bash命令行输入命令

最好是在.git的根目录下一起创建。

```bash
touch .gitignore   
```

这样就就会生成一个.gitignore  的文件，向里面添加要忽略的内容即可。

分享一个github开源库，里面给出了，大部分提交代码需要忽略的文件，自己去找即可。

[gitignore](https://github.com/github/gitignore)

