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
* 第一步、在本地生成id_rsa.pub文件
* 第二步、将id_rsa.pub文件中的内容添加到github上的Add SSH key页面即可
     [https://github.com/settings/keys](https://github.com/settings/keys)
* 第三步、[必要]使用ssh地址而非https地址的方式git clone，或者改变remote远程url为ssh
   - 一种方式就是在git clone的时候使用ssh方式
   两个方式的url地址不同，认证方式也不同。使用ssh时保存密钥对以后可以不再输入帐号密码，
而https却不能。
   - 另一种方式就是改变remote远程url，如下：
    ```
    $ git remote rm origin  
    $ git remote add origin git@github.com:cld378632668/utils.git
    ```
