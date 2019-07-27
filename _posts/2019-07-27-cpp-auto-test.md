---
layout: post
title:  "C++11 auto实践"
date:   2019-07-27 12:44:54
categories: c++
tags: c++11 auto
---

* content
{:toc}

* 测试一
```
#include <iostream>
#include <typeinfo>

using namespace std;

int main()
{
	auto x = 5;
	std::cout << typeid(x).name() << std::endl;

	auto pi = new auto(1);
	std::cout << typeid(pi).name() << std::endl;

	const auto *v = &x, u = 6;
	std::cout << typeid(v).name() << std::endl;
	std::cout << typeid(u).name() << std::endl;

	static auto y = 0.0;
	std::cout << typeid(y).name() << std::endl;

	// auto int r; // error

	// auto s; // error

    return 0;
}

output：
int
int *
int const *
int
double
```

* 测试二
```
#include <iostream>
#include <typeinfo>

int main()
{
    int x = 0;
    const auto *v = &x, u = 6.0;
    std::cout << typeid(v).name() << std::endl;
    std::cout << typeid(u).name() << std::endl;
    return 0;
}
output:
autoTest.cpp: In function ‘int main()’:
autoTest.cpp:7:29: error: inconsistent deduction for ‘const auto’: ‘int’ and then ‘double’
     const auto *v = &x, u = 6.0;
```

* 测试三
```
#include <iostream>
#include <typeinfo>

int main()
{
    int x = 0;
    auto *a = &x;
    std::cout << typeid(a).name() << std::endl;

    auto b = &x;
    std::cout << typeid(b).name() << std::endl;

    auto &c = x;
    std::cout << typeid(c).name() << std::endl;

    auto d = c;
    std::cout << typeid(d).name() << std::endl;

    const auto e = x;
    std::cout << typeid(e).name() << std::endl;

    auto f = e;
    std::cout << typeid(f).name() << std::endl;

    const auto &g = x;
    std::cout << typeid(g).name() << std::endl;

    auto &h = g;
    std::cout << typeid(h).name() << std::endl;
}
output:
int *
int *
int
int
int
int
int
int
```

* 测试四（auto限制）
```
#include <iostream>
#include <typeinfo>

void func(auto a = 1) {} // error:auto 不能用于函数参数

struct Foo
{
	auto var1_ = 0; // error:auto 不能用于非静态成员函数
	static const auto var2_ = 0;
};

template <typename T>
sturct Bar{};

int main()
{
	int arr[10] = { 0 };
	auto aa = arr;

	auto rr[10] = arr; // error
	Bar<int> bar;
	Bar<auto> bb = bar; // error

	return 0
}
```
