---
layout: post
title:  "Git 免密码推送代码"
date:   2019-07-17 20:14:54
categories: git
tags: git tools
---

* content
{:toc}

本文我将讲述使用ssh生成秘钥，免密码向github推送代码。

## 核心步骤
* 第一步、在本地生成`id_rsa.pub`文件
* 第二步、将`id_rsa.pub`文件中的内容添加到`github`上的`Add SSH key`页面即可
     [https://github.com/settings/keys](https://github.com/settings/keys)
* 第三步、[必要]使用`ssh`地址而非`https`地址的方式`git clone`，或者改变`remote`远程`url`为`ssh`
   - 一种方式就是在`git clone`的时候使用`ssh`方式
   两个方式的`url`地址不同，认证方式也不同。使用`ssh`时保存密钥对以后可以不再输入帐号密码，
而`https`却不能。
   - 另一种方式就是改变`remote`远程`url`，如下：
    ```
    $ git remote rm origin  
    $ git remote add origin git@github.com:cld378632668/utils.git
    ```
