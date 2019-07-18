---
layout: post
title:  "Auto类型推导"
categories: C++11
tags:  C++11 C++ coding 深入应用C++11  
author: qmsggg37
---

* content
{:toc}


## 前言

一直都只是简单的使用`C++11`的`auto关键字`，没有去系统的学习`auto关键字`的正确使用方式，
目前希望通过`深入应用C++11:代码优化与工程级应用`这本书系统的学些一下`auto`相关的用法。

## auto关键字的新意义
auto变量能自动根据初始化的值推导出变量类型；
不同与Python等动态类型语言的运行时变量类型推导，隐式类型定义的类型推导发生在编译期。
它的作用是让编译器自动推断出这个变量的类型，而不需要显示指定类型。
auto基本用法：
```
auto x = 5; // OK: x是int类型
auto pi = new auto(1); // OK: pi被推导为int*
const auto *v = &x, u = 6; // OK: v是const int*类型，u是const int类型
static auto y = 0.0； // OK: y是double类型
auto int r; // error: auto不再表示存储类型指示符
auto s; // error: auto无法推导出s的类型
```
在上面的代码示例中：字面量5 是一个const int 类型，变量x 将被推导为int 类型（const
被丢弃，后面说明），并被初始化为5 ；pi 的推导说明auto 还可以用于new 操作符。在例子
中，new 操作符后面的auto(1) 被推导为int(1)，因此pi 的类型是int* ；接着，由&x 的类型
为int*，推导出const auto* 中的auto 应该是int，于是v 被推导为const int*，而u 则被推导
为const int
v 和u 的推导需要注意两点：

* 虽然经过前面 const auto*v=&x 的推导，auto 的类型可以确定为 int 了，但是 u 仍然必
须要写后面的“=6”，否则编译器不予通过。
* u 的初始化不能使编译器推导产生二义性。例如，把 u 的初始化改成“ u=6.0”，编译
器将会报错：
```
const auto *v = &x, u = 6.0;
error: inconsistent deduction for 'const auto': 'int' and then
'double'
```
由上面的例子可以看出来，auto 并不能代表一个实际的类型声明（如s 的编译错误），只
是一个类型声明的“占位符”。
使用auto 声明的变量必须马上初始化，以让编译器推断出它的实际类型，并在编译时将
auto 占位符替换为真正的类型

## auto的推导规则
eg:
```
int x = 0;
auto * a = &x; // a -> int*，auto 被推导为int
auto b = &x; // b -> int*，auto 被推导为int*
auto & c = x; // c -> int&，auto 被推导为int
auto d = c; // d -> int ，auto 被推导为int
const auto e = x; // e -> const int
auto f = e; // f -> int
const auto& g = x; // e -> const int&
auto& h = g; // f -> const int&
```
通过上面的一系列示例，可以得到下面这两条规则：
1）当不声明为指针或引用时，auto 的推导结果和初始化表达式抛弃引用和cv 限定符后
类型一致。
2）当声明为指针或引用时，auto 的推导结果将保持初始化表达式的cv 属性。
看到这里，对函数模板自动推导规则比较熟悉的读者可能会发现，auto 的推导和函数模板参数
的自动推导有相似之处。比如上面例子中的auto，和下面的模板参数自动推导出来的类型是一致的：
```
template <typename T> void func(T x) {} // T -> auto
template <typename T> void func(T * x) {} // T * -> auto *
template <typename T> void func(T & x) {} // T & -> auto &
template <typename T> void func(const T x) {} // const T ->
const auto
template <typename T> void func(const T * x) {} // const T * ->
const auto *
template <typename T> void func(const T & x) {} // const T & ->
const auto &
```
注意：auto 是不能用于函数参数的。上面的示例代码只是单纯比较函数模板参数推导和
auto 推导规则的相似处。
因此，在熟悉auto 推导规则时，可以借助函数模板的参数自动推导规则来帮助和加强理解。

## auto的限制
eg:
```
void func(auto a = 1) {} // error: auto 不能用于函数参数
struct Foo
{
    auto var1_ = 0; // error: auto 不能用于非静态成员变量
    static const auto var2_ = 0; // OK: var2_ -> static const int
};
template <typename T>
struct Bar {};
int main(void)
{
    int arr[10] = {0};
    auto aa = arr; // OK: aa -> int *
    auto rr[10] = arr; // error: auto 无法定义数组
    Bar<int> bar;
    Bar<auto> bb = bar; // error: auto 无法推导出模板参数
    return 0;
}
```
在Foo 中，auto 仅能用于推导static const 的整型或者枚举成员（因为其他静态类型在
C++ 标准中无法就地初始化），虽然C++11 中可以接受非静态成员变量的就地初始化，但却
不支持auto 类型非静态成员变量的初始化。
在main 函数中，auto 定义的数组rr 和Bar<auto>bb 都是无法通过编译的。

注意：main 函数中的aa 不会被推导为int[10]，而是被推导为int*。这个结果可以通过上一
节中auto 与函数模板参数自动推导的对比来理解。
  
## 什么时候用auto
在C++11 之前，定义了一个stl 容器以后，在遍历的时候常常会写这样的代码：
```
#include <map>
int main(void)
{
    std::map<double, double> resultMap;
    // ...
    std::map<double,double>::iterator it = resultMap.begin();
    for(; it != resultMap.end(); ++it)
    {
        // do something
    } 
    return 0;
}
```
观察上面的迭代器（iterator）变量it 的定义过程，总感觉有点憋屈。其实通过resultMap.
begin()，已经能够知道it 的具体类型了，却非要书写上长长的类型定义才能通过编译。
来看看使用了auto 以后的写法：
```
#include <map>
int main(void)
{
    std::map<double, double> resultMap;
    // ...
    for(auto it = resultMap.begin(); it != resultMap.end(); ++it)
    {
        // do something
    }
    return 0;
}
```
再次观察it 的定义过程，是不是感到清爽了很多？
再看一个例子，在一个unordered_multimap 中查找一个范围，代码如下：
```
#include <map>
int main(void)
{
    std::unordered_multimap<int, int>resultMap;
    // ...
    std::pair<std::unordered_multimap<int,int>::iterator,
    std::unordered_multimap<int, int>::iterator>
    range = resultMap.equal_range(key);
    return 0;
}
```
这个equal_range 返回的类型声明显得烦琐而冗长，而且实际上并不关心这里的具体类
型（大概知道是一个std::pair 就够了）。这时，通过auto 就能极大的简化书写，省去推导具体
类型的过程：
```
#include <map>
int main(void)
{
    std::unordered_multimap<int, int> map;
    // ...
    auto range = map.equal_range(key);
    return 0;
}
另外，在很多情况下我们是无法知道变量应该被定义成什么类型的
class Foo
{
public:
    static int get(void)
    {
        return 0;
    }
};
class Bar
{
public:
    static const char* get(void)
    {
        return "0";
    }
};
template <class A>
void func(void)
{
    auto val = A::get();
    // ...
}
int main(void)
{
    func<Foo>();
    func<Bar>();
    return 0;
}
```
在这个例子里，我们希望定义一个泛型函数func，对所有具有静态get 方法的类型A，
在得到get 的结果后做统一的后续处理。若不使用auto，就不得不对func 再增加一个模板参
数，并在外部调用时手动指定get 的返回值类型。

注意：auto 是一个很强大的工具，但任何工具都有它的两面性。不加选择地随意使用auto，
会带来代码可读性和维护性的严重下降。因此，在使用auto 的时候，一定要权衡好
它带来的“价值”和相应的“损失”。
