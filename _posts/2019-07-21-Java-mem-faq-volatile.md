---
layout: post
title:  "Java内存模型FAQ（十）volatile是干什么用的"
date:   2019-07-17 20:14:54
categories: java
tags: java volatile
---

* content
{:toc}

# `Java`内存模型`FAQ`（`十`）`volatile`是干什么用的
`Volatile`字段是用于线程间通讯的特殊字段。每次读`volatile`字段都会看到其它线程写入该字段的最新值；实际上，程序员之所以要定义`volatile`字段是因为在某些情况下由于缓存和重排序所看到的陈旧的变量值是不可接受的。编译器和运行时禁止在寄存器里面分配它们。它们还必须保证，在它们写好之后，它们被从缓冲区刷新到主存中，因此，它们立即能够对其他线程可见。相同地，在读取一个`volatile`字段之前，缓冲区必须失效，因为值是存在于主存中而不是本地处理器缓冲区。在重排序访问`volatile`变量的时候还有其他的限制。

在旧的内存模型下，访问`volatile`变量不能被重排序，但是，它们可能和访问非`volatile`变量一起被重排序。这破坏了`volatile`字段从一个线程到另外一个线程作为一个信号条件的手段。

在新的内存模型下，`volatile`变量仍然不能彼此重排序。和旧模型不同的时候，`volatile`周围的普通字段的也不再能够随便的重排序了。写入一个`volatile`字段和释放监视器有相同的内存影响，而且读取`volatile`字段和获取监视器也有相同的内存影响。事实上，因为新的内存模型在重排序`volatile`字段访问上面和其他字段（`volatile`或者非`volatile`）访问上面有了更严格的约束。当线程A写入一个`volatile`字段`f`的时候，如果线程`B`读取`f`的话 ，那么对线程`A`可见的任何东西都变得对线程`B`可见了。

如下例子展示了`volatile`字段应该如何使用：
```
class VolatileExample {
  int x = 0;
  volatile boolean v = false;
  public void writer() {
    x = 42;
    v = true;
  }

  public void reader() {
    if (v == true) {
      //uses x - guaranteed to see 42.
    }
  }
}
```
假设一个线程叫做“`writer`”，另外一个线程叫做“`reader`”。对变量`v`的写操作会等到变量`x`写入到内存之后，然后读线程就可以看见`v`的值。因此，如果`reader`线程看到了`v`的值为`true`，那么，它也保证能够看到在之前发生的写入`42`这个操作。而这在旧的内存模型中却未必是这样的。如果`v`不是`volatile`变量，那么，编译器可以在`writer`线程中重排序写入操作，那么`reader`线程中的读取`x`变量的操作可能会看到`0`。

实际上，`volatile`的语义已经被加强了，已经快达到同步的级别了。为了可见性的原因，每次读取和写入一个`volatile`字段已经像一个半同步操作了

重点注意：对两个线程来说，为了正确的设置`happens-before`关系，访问相同的`volatile`变量是很重要的。以下的结论是不正确的：当线程`A`写`volatile`字段`f`的时候，线程`A`可见的所有东西，在线程`B`读取`volatile`的字段`g`之后，变得对线程`B`可见了。释放操作和获取操作必须匹配（也就是在同一个`volatile`字段上面完成）。
