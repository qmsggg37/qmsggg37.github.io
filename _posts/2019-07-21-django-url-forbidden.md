---
layout: post
title:  "(django)访问url报错Forbidden (CSRF cookie not set.): xxx"
categories: python
tags:  django
author: qmsggg37
---

* content
{:toc}

问题：页面访问时报错
```
Forbidden (CSRF cookie not set.): xxx
```
 
解决方法：
修改settings.py文件，注释掉
```
django.middleware.csrf.CsrfViewMiddleware',
```

![image](https://user-images.githubusercontent.com/28669743/38229592-0f9bc6fe-373c-11e8-851b-0305eb19841b.png)
