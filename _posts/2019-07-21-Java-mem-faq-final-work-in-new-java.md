---
layout: post
title:  "Java内存模型FAQ（九）在新的Java内存模型中，final字段是如何工作的"
date:   2019-07-17 20:14:54
categories: java
tags: java final
---

* content
{:toc}

# `Java`内存模型`FAQ`（`九`）在新的`Java`内存模型中，`final`字段是如何工作的
一个对象的`final`字段值是在它的构造方法里面设置的。假设对象被正确的构造了，一旦对象被构造，在构造方法里面设置给`final`字段的的值在没有同步的情况下对所有其他的线程都会可见。另外，引用这些`final`字段的对象或数组都将会看到`final`字段的最新值。

对一个对象来说，被正确的构造是什么意思呢？简单来说，它意味着这个正在构造的对象的引用在构造期间没有被允许逸出。（参见安全构造技术）。换句话说，不要让其他线程在其他地方能够看见一个构造期间的对象引用。不要指派给一个静态字段，不要作为一个listener注册给其他对象等等。这些操作应该在构造方法之后完成，而不是构造方法中来完成。
```
class FinalFieldExample {
  final int x;
  int y;
  static FinalFieldExample f;
  public FinalFieldExample() {
    x = 3;
    y = 4;
  }

  static void writer() {
    f = new FinalFieldExample();
  }

  static void reader() {
    if (f != null) {
      int i = f.x;
      int j = f.y;
    }
  }
}
```
上面的类展示了`final`字段应该如何使用。一个正在执行`reader`方法的线程保证看到`f.x`的值为`3`，因为它是`final`字段。它不保证看到`f.y`的值为`4`，因为`f.y`不是`final`字段。如果`FinalFieldExample`的构造方法像这样：
```
public FinalFieldExample() { // bad!
  x = 3;
  y = 4;
  // bad construction - allowing this to escape
  global.obj = this;
}
```
那么，从`global.obj`中读取`this`的引用线程不会保证读取到的`x`的值为`3`。

能够看到字段的正确的构造值固然不错，但是，如果字段本身就是一个引用，那么，你还是希望你的代码能够看到引用所指向的这个对象（或者数组）的最新值。如果你的字段是`final`字段，那么这是能够保证的。因此，当一个`final`指针指向一个数组，你不需要担心线程能够看到引用的最新值却看不到引用所指向的数组的最新值。重复一下，这儿的“正确的”的意思是“对象构造方法结尾的最新的值”而不是“最新可用的值”。

现在，在讲了如上的这段之后，如果在一个线程构造了一个不可变对象之后（对象仅包含`final`字段），你希望保证这个对象被其他线程正确的查看，你仍然需要使用同步才行。例如，没有其他的方式可以保证不可变对象的引用将被第二个线程看到。使用`final`字段的程序应该仔细的调试，这需要深入而且仔细的理解并发在你的代码中是如何被管理的。

如果你使用`JNI`来改变你的`final`字段，这方面的行为是没有定义的。
