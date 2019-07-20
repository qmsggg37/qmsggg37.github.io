---
layout: post
title:  "Python的描述符（descriptor)"
categories: python
tags:  python descriptor
author: qmsggg37
---

* content
{:toc}

```
# -*- coding: utf-8 -*-
# @Time    : 2018/3/27 22:48
# @Author  : qmsggg
# @Email   : luohao0414@163.com
# @File    : Movie.py
# @Software: PyCharm

class Movie(object):
    def __init__(self, title, rating, runtime, budget, gross):
        self._budget = None
        self.title = title
        self.rating = rating
        self.runtime = runtime
        self.gross = gross
        self.budget = budget

    @property
    def budget(self):
        return self._budget

    @budget.setter
    def budget(self, value):
        if value < 0:
            raise ValueError("negative value not allowed: %s" % value)
        self._budget = value

    def profit(self):
        return self.gross - self.budget

if __name__ == '__main__':
    m = Movie('Casablanca', 97, 102, 964000, 13000000)
    print m.budget

    try:
        m.budget = -100
    except ValueError, err:
        print "Woops. Not allowed: %s" % err.message

    m.budget = 10000
```
## 疑问
```
_budget==budget
```
>经调试，证明这两个变量的值是保持一致的
![image](https://user-images.githubusercontent.com/28669743/37976467-2411b2f6-3214-11e8-8204-055b58a9fee7.png)

```
# -*- coding: utf-8 -*-
# @Time    : 2018/3/27 23:18
# @Author  : qmsggg
# @Email   : luohao0414@gmail.com
# @File    : NonNegative.py
# @Software: PyCharm

from weakref import WeakKeyDictionary


class NonNegative(object):
    """A descriptor that forbids negative values"""

    def __init__(self, default):
        self.default = default
        self.data = WeakKeyDictionary()

    def __get__(self, instance, owner):
        # we get here when someone calls x.d, and d is a NonNegative instance
        # instance = x
        # owner = type(x)
        return self.data.get(instance, self.default)

    def __set__(self, instance, value):
        # we get here when someone calls x.d = val, and d is a NonNegative instance
        # instance = x
        # value = val
        if value < 0:
            raise ValueError("Negative value not allowed: %s" % value)
        self.data[instance] = value


class Movie(object):
    # always put descriptors at the class-level
    rating = NonNegative(0)
    runtime = NonNegative(0)
    budget = NonNegative(0)
    gross = NonNegative(0)

    def __init__(self, title, rating, runtime, budget, gross):
        self.title = title
        self.rating = rating
        self.runtime = runtime
        self.budget = budget
        self.gross = gross

    def profit(self):
        return self.gross - self.budget


m = Movie('Casablanca', 97, 102, 964000, 1300000)
print m.budget  # calls Movie.budget.__get__(m, Movie)
m.rating = 100  # calls Movie.budget.__set__(m, 100)
try:
    m.rating = -1  # calls Movie.budget.__set__(m, -100)
except ValueError:
    print "Woops, negative value"
```

参考：

- [ ] [解密 Python 的描述符（descriptor）](http://python.jobbole.com/81899/)
