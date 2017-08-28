---
title: hexo博文置顶技巧
date: 2016-11-21 15:34:36
tags: [Hexo技巧]
categories: Hexo
top:
---

---
Hexo博客文章置顶技巧

<!--more-->

# Hexo置顶问题解决
将node_modules/hexo-generator-index/lib/generator.js的文件内容替换成一下内容
```
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

在新建的md日志文件种加入top初始化命令
比如说：

```

title: Hexo置顶文章
date: 2016-11-11 23:26:22
tags:[置顶]
categories: Hexo
top: 0 # 0或者1
```

当top的值取0的时候，表示默认排序，即是按照时间顺序来排序。

当top的值取1到无为置顶文件限大的时候，值最高的md文件即是置顶文章