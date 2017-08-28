---
title: Hexo框架下Next主题的优化技巧
date: 2017-05-02 20:53:46
tags: [Next优化]
categories: Hexo
comments:
---
好记性不如烂笔头，我深刻的体会了这句话的含义。
不小心的把之前搭好的博客框架覆盖了，重新记录一下。
<!--more-->

## Next主题下为每一篇文章统计字数和总字数
在这里，我采用的是wordcount的插件来完成的，打开你的git执行以下代码安装wordcount插件。

``` bash
npm install hexo-wordcount --save
```

安装完成以后，开始我们第一步。


### 第一步:找到post.swig文件

```
themes\next（主题根目录下）\layout\_macro\post.swig
```

我先解释以下，这个post文件夹就是用来定义你的每篇文章的那些自定义的内容栏的，比如阅读时长，阅读数目，认真看里面的代码就会发现，next主题其实已经帮我们集成了很多的你可能会用来的模块。

### 第二步：定位到如下代码：
```html
{# LeanCould PageView #}
          {% if theme.leancloud_visitors.enable %}
             <span id="{{ url_for(post.path) }}" class="leancloud_visitors" data-flag-title="{{ post.title }}">
               <span class="post-meta-divider">|</span>
               <span class="post-meta-item-icon">
                 <i class="fa fa-eye"></i>
               </span>
               <span class="post-meta-item-text">{{__('post.visitors')}} </span>
               <span class="leancloud-visitors-count"></span>
              </span>
          {% endif %}
     
```
### 第三步：添加如下代码

```html
<span class="post-time">
     &nbsp; | &nbsp;
     <span class="post-meta-item-icon">
    <i class="fa fa-calendar-o"></i>
    </span>
<span class="post-meta-item-text">字数统计:</span>
    <span class="post-count">{{ wordcount(post.content) }}字</span>
</span> 

<span class="post-time">
          &nbsp; | &nbsp;
     <span class="post-meta-item-icon">
         <i class="fa fa-calendar-o"></i>
     </span>
    <span class="post-meta-item-text">阅读时长:</span>
     <span class="post-count">{{ min2read(post.content) }}分</span>
</span>
```
### 第四步：打开wordcount

在next主题下的_config.yml文件，定位到wordcount。
```
WordCount: true
```

### 第五步：在博客下面显示总共字数

 定位到footer.swig
```
D:\Hexo\themes\hexo-theme-next-master\layout\_partials\footer.swig
```
在文段最后添加如下代码
```html
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">共{{ totalcount(site) }}字</span>
</div>
```

这样的话，我们就配置好了，赶快测试一下吧。

## 接入cnzz数据分析

### 部署cnzz服务器端
首先当然是申请一个cnzz得账号，现在被友盟整合以后，连接如下
[cnzz申请](https://web.umeng.com "cnzz申请")
选择**U—Web网站统计**，然后点击**立即使用**，添加一个站点，我就不贴图了，贴图麻烦，一般就是个人得博客，非常容易，添加成功以后，cnzz服务器端口得操作就完成了。

### 打开cnzz_siteid
在next主题下的_config.yml,ctrl+f 搜索cnzz_sited.


```
cnzz_siteid:
```
关于这个cnzz_siteid:怎么获得？点开你部署好的站点，在你的服务器端的URL里面会出现一串数字**siteid=1260855711**（比如我的），截取出来填入进去。

### 站长统计
定位到一下文件：

```
D:\Hexo\themes\hexo-theme-next-master\layout\_partials\footer.swig
```
添加如下代码：
```html
{% if theme.cnzz_siteid %}
   <span style="margin-left:8px;">
    <div class="powered-by"></div>
   <script src="http://s6.cnzz.com/stat.php?id={{ theme.cnzz_siteid }}&web_id={{ theme.cnzz_siteid }}" type="text/javascript"></script>
   </span>
{% endif %}
```
赶快去测试一下吧！

## 告别多说，拥抱disqus

由于多说关闭了，只能转战disqus，disqus的后台的bug更少一些，最主要的是，作为一个合格的程序员，你应该要学会科学上网。才能拥抱世界。

### 申请disqus账号
我看了一下，如果你之前有google账号的话，它会默认登陆你的google账号，没有的话，就申请一个吧，会方便不少的。

### 申请自己的website

点击**GET STARTED**->**I want to install Disqus on my site**上个截图
 
 ![imge_20170623224031-min.png](http://ogtmd8elu.bkt.clouddn.com/2017/06/23/2161f0a6d4821e1085571da78c7146b6.png "imge_20170623224031-min.png")
 
 顺着点击确认就行了，disqus后台就算处理完毕了。
 
 ### 配置config文件的disqus设置
 回到NEXT主题下的_config.yml文件找到，把你刚刚申请的Website Name填入，就大功告成了。代码如下：
 ```
disqus_shortname: ArcherBlog
```
 ## 接入不蒜子
 用不蒜子来记录博客的从点击量和到站人数，还是挺方便的。
 
 ### 定位到footer.swig
 
 ```
 D:\Hexo\themes\hexo-theme-next-master\layout\_partials\footer.swig
 ```

### 初始化不蒜子

这段代码放在顶部

```xml
<script async src="//dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js">
</script>
```
这段代码用来初始化不蒜子插件的。

### 添加总共点击量和到站人数

在同样的footer.swig文件下面底部 插入下面两段代码

```html
<span id="busuanzi_container_site_pv">
<div class="powered-by"></div>
      本站总访问量<span id="busuanzi_value_site_pv"></span>次
  </span>

  <span id="busuanzi_container_site_uv">
  <div class="powered-by"></div>
    本站访客数<span id="busuanzi_value_site_uv"></span>人次
  </span>
```

## favicon.ico的设置

如果不设置的这个favicon.ico 的话，看着自己的网站页面总有种怪怪的感觉，推荐一个制作favicon的网站 [比特虫](http://www.bitbug.net/ "比特虫")。

将做好的图片放在站点（注意是站点的，并不是主题的）下的source文件夹下，并在站点的_config.yml里面添加如下代码即可：

    favicon: /favicon.ico（你的favicon的全称）

