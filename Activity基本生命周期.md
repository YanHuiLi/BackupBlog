---
title: Activity基本生命周期
date: 2016-11-21 15:31:25
tags: [生命周期,Android四大组件]
categories: Android
top: 30
---

---
现在讨论activity生命周期的文章已经很多了，但是有时候看得太多反而会觉得思绪很乱。
这篇博的目的就是帮助你快速的理清思路，也是自己学习的一些总结。
<!--more-->



## 首先什么是Activity？
在很多的书籍里面将其直译为活动。因为像比如service之类的在后台跑着的服务，也可以称作是活动的一种，言下之意是同一个活动里面，有前台的部分和后台的部分（通常就是service）。
因此我认为恰当的翻译应该是：直接和用户交互的组件。在这里的话，我就尽量只用activity而不使用活动作为描述。

## Activity的生命周期
先上一个最经典的Activity生命周期的图片：

![Activity生命周期](http://ogtmd8elu.bkt.clouddn.com/activity_lifecycle-min.png)

### onCreate()
在onCreate方法里面，表示的的是一个activity正在被创建，这是activity启动的第一个方法，在这个方法里面，我们一般完成一些界面布局文件的初始化，绑定layout，setContentView，findviewbyid的初始化工作。

### onStart()
onStart方法在活动由不可见到可见的时候调用，在实际的开发过程中，onCreate方法执行过一次就不再执行了，因此我的经验而言，会在onStart方法里面更新适配器，重新拿到数据，更新UI界面。

### onResume()
当这个方法调用的时候，说明activity已经等待和用户发生交互了，并出于运行的状态，位于返回栈顶部。

### onPause()
当系统正在启动或者恢复另一个activity的时候，这个方法得到执行。在这个方法里面，我们通常保存一些重要的数据，释放一些资源，以便以后恢复的时候能完善用户体验

### onStop()
这个方法在activity变得完全不可见的时候调用，如果只是得到一个透明的对话框，提示框的话，onPause方法会得到执行，onStop方法不会执行。

### onDestroy()
这个方法在activity被摧毁之前调用，之后activity处于销毁状态，我们一般在这里，unregisterBroadcast，否则会抛出一个异常。

### onRestart()
当activity从停止状态变为运行状态（重新启动）的时候，此方法得到调用。

## 完整的生命周期
完整的一个activity的生命周期，指的是从onCreate方法到ondestroy方法结束的整个过程。

## 前台生存期
activity从onRemuse方法到onPause方法之间的过程，整个过程是用户可见，可交互的。

## 总结
当一个activity（A）启动的时候：

    onCreate()->onStart()->onResume().
以上三个方法会率先执行，等待用户操作。

启动第二个Activity（B）并且完全遮挡住A的时候。A会先执行
```java
onPause()->onStop()
```
如果此时点击back返回按钮，A会执行
```
onRestart()->onStart()->onResume().
```

当启动的第二个activity（B）没有完全遮挡住A的时候，A只会执行onPause方法，比如说弹出一个对话框，dialog之类的。当用户点击返回的时候，A会执行onResume方法，如图所示，执行。
```
onPause()->onResume()
```

有一点需要特别注意的，当正在处于onPause或者onStop阶段的activity有时候会遇到更高优先级的另外的activity启动，或者内存不够，所以系统会回收内存，随即把我们的activity回收掉。当我们返回原activity在时候，就需要从onCreate方法开始重新创建。

## 活动回收了怎么办

如果很不凑巧，在系统回收的activity里面恰好有存放的数据，我们注意到onSaveInstanceState（Bundle savedInstanceState），有个bundle对象，这个bundle对象就是用来存储数据的。
```java
    @Override
   protected void onSaveInstanceState(Bundle outState) {
       super.onSaveInstanceState(outState);
   
         String tempData= "some data you need store";
            output.putstring("data",tempdata);
   }
```

采用这个方法用来保存零时的数据，用键值对的方式来保存。然后在oncreate方法种来恢复。
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);


      if(savedInstanceState!=null){
       String  tempData=savedInstanceState.getString("data");
 }
```
如果savedInstanceState的值不为空的话，即可取出值。


## 参考资料：
《第一行代码》 郭霖。
