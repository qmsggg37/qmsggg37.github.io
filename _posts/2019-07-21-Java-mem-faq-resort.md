---
layout: post
title:  "Java内存模型FAQ（四）重排序意味着什么？"
date:   2019-07-17 20:14:54
categories: java
tags: java 内存模型
---

* content
{:toc}

# Java内存模型FAQ（四）重排序意味着什么？
在很多情况下，访问一个程序变量（对象实例字段，类静态字段和数组元素）可能会使用不同的顺序执行，而不是程序语义所指定的顺序执行。编译器能够自由的以优化的名义去改变指令顺序。在特定的环境下，处理器可能会次序颠倒的执行指令。数据可能在寄存器，处理器缓冲区和主内存中以不同的次序移动，而不是按照程序指定的顺序。

例如，如果一个线程写入值到字段`a`，然后写入值到字段`b`，而且`b`的值不依赖于`a`的值，那么，处理器就能够自由的调整它们的执行顺序，而且缓冲区能够在`a`之前刷新b的值到主内存。有许多潜在的重排序的来源，例如编译器，`JIT`以及缓冲区。

编译器，运行时和硬件被期望一起协力创建好像是顺序执行的语义的假象，这意味着在单线程的程序中，程序应该是不能够观察到重排序的影响的。但是，重排序在没有正确同步了的多线程程序中开始起作用，在这些多线程程序中，一个线程能够观察到其他线程的影响，也可能检测到其他线程将会以一种不同于程序语义所规定的执行顺序来访问变量。

大部分情况下，一个线程不会关注其他线程正在做什么，但是当它需要关注的时候，这时候就需要同步了。
