---
title: hexo云标签使用
date: 2016-11-21 15:35:58
tags: [Hexo,技巧]
categories: Hexo
---




# Hexo云标签和分类的使用。

第一步新建一个tags
新建一个tags的页面

            hexo new page tags

这样子的话在/hexo/source/tags/index.md文件里面可以设置一些基本的参数比如说在这个页面，我不喜欢看得到评论。

```

title: tags
date: 2016-11-10 21:08:22
type: "tags"
comments: false

```
将comments设置为flase的话，将不会展示评论界面。
注意这里，在上述的文件下面加入type: "tags"，当你点击主题上面的标签云的时候，就会自动分类你所添加的标签云。


# 第二步设置分类模板

首先，我们通过hexo n “name”命令来新建一个页面，在source/_posts目录下找到刚才新建的name.md文件，用记事本或者sublime text打开。
我们看到默认的页面是这样的：

title: name
date: 2014-08-05 11:15:00 

---

hexo->scaffolds/post.md

可以编辑标题、日期、标签和内容，但是没有分类的选项。我们可以手动加入categories:项，或者打开scaffolds/post.md文件，在tages:上面加入categories:,保存后，重新执行hexo n ‘name’命令，会发现新建的页面里有categories:项了。
scaffolds目录下，是新建页面的模板，执行新建命令时，是根据这里的模板页来完成的，所以可以在这里根据你自己的需求添加一些默认值。

````

title: Hexo的云标签使用
date: 2016-11-10 22:46:55
tags: [云标签,分类]
categories: Hexo
---
````

第三步设置分类
在我们编辑文章的时候，直接在categories:项填写属于哪个分类，但如果分类是中文的时候，路径也会包含中文。
比如分类我们设置的是：

categories: 编程
那在生成页面后，分类列表就会出现编程这个选项，他的访问路径是：

*/categories/编程

如果我们想要把路径名和分类名分别设置，需要怎么办呢？

打开根目录下（并非主题目录）的配置文件_config.yml（别打开错了），找到如下位置做更改（设置种类）：

````
# Category & Tag
default_category: uncategorized
category_map:
    编程: programming
    Hexo：Hexo
    生活: life
    其他: other
tag_map:
`````

在这里category_map:是设置分类的地方，每行一个分类，冒号前面是分类名称，后面是访问路径。
可以提前在这里设置好一些分类，当编辑的文章填写了对应的分类名时，就会自动的按照对应的路径来访问。

# 第四步设置标签
在编辑文章的时候，tags:后面是设置标签的地方，如果有多个标签的话，可以用下面两种办法来设置：

```
tages: [标签1,标签2,...标签n]
```
