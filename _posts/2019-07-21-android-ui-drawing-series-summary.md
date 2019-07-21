---
layout: post
title:  "Android UI绘制系列总结"
date:   2019-07-21 15:14:54
categories: android
tags: java ui
---

* content
{:toc}

![image](https://user-images.githubusercontent.com/28669743/43357959-b6b7706a-92bc-11e8-9cd4-db17d7abed50.png)
DecorView是一个应用窗口的根容器，它本质上是一个FrameLayout。DecorView有唯一一个子View，它是一个垂直LinearLayout，包含两个子元素，一个是TitleView（ActionBar的容器），另一个是ContentView（窗口内容的容器）。关于ContentView，它是一个FrameLayout（android.R.id.content)，我们平常用的setContentView就是设置它的子View。上图还表达了每个Activity都与一个Window（具体来说是PhoneWindow）相关联，用户界面则由Window所承载。

## Window
Window即窗口，这个概念在Android Framework中的实现为android.view.Window这个抽象类，这个抽象类是对Android系统中的窗口的抽象。在介绍这个类之前，我们先来看看究竟什么是窗口呢？

实际上，窗口是一个宏观的思想，它是屏幕上用于绘制各种UI元素及响应用户输入事件的一个矩形区域。通常具备以下两个特点：

独立绘制，不与其它界面相互影响；
不会触发其它界面的输入事件；
在Android系统中，窗口是独占一个Surface实例的显示区域，每个窗口的Surface由WindowManagerService分配。我们可以把Surface看作一块画布，应用可以通过Canvas或OpenGL在其上面作画。画好之后，通过SurfaceFlinger将多块Surface按照特定的顺序（即Z-order）进行混合，而后输出到FrameBuffer中，这样用户界面就得以显示。

android.view.Window这个抽象类可以看做Android中对窗口这一宏观概念所做的约定，而PhoneWindow这个类是Framework为我们提供的Android窗口概念的具体实现。接下来我们先来介绍一下android.view.Window这个抽象类。

这个抽象类包含了三个核心组件：

WindowManager.LayoutParams: 窗口的布局参数；
Callback: 窗口的回调接口，通常由Activity实现；
ViewTree: 窗口所承载的控件树。

## PhoneWindow
PhoneWindow这个类是Framework为我们提供的Android窗口的具体实现。我们平时调用setContentView()方法设置Activity的用户界面时，实际上就完成了对所关联的PhoneWindow的ViewTree的设置。我们还可以通过Activity类的requestWindowFeature()方法来定制Activity关联PhoneWindow的外观，这个方法实际上做的是把我们所请求的窗口外观特性存储到了PhoneWindow的mFeatures成员中，在窗口绘制阶段生成外观模板时，会根据mFeatures的值绘制特定外观。

## ViewRootImp
首先我们需要知道是谁负责执行View绘制的整个流程。实际上，View的绘制是由ViewRoot来负责的。每个应用程序窗口的decorView都有一个与之关联的ViewRoot对象，这种关联关系是由WindowManager来维护的。

那么decorView与ViewRoot的关联关系是在什么时候建立的呢？答案是Activity启动时，ActivityThread.handleResumeActivity()方法中建立了它们两者的关联关系。

## 三个阶段
View的整个绘制流程可以分为以下三个阶段：

measure: 判断是否需要重新计算View的大小，需要的话则计算；
layout: 判断是否需要重新计算View的位置，需要的话则计算；
draw: 判断是否需要重新绘制View，需要的话则重绘制。
这三个子阶段可以用下图来描述：

![image](https://user-images.githubusercontent.com/28669743/43357995-34e6b432-92bd-11e8-9f59-d71a87421263.png)
