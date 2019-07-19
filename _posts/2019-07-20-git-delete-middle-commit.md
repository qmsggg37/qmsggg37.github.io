---
layout: post
title:  "git删除中间某个commit"
categories: git
tags:  git
author: qmsggg37
---

* content
{:toc}

## git删除中间某个commit

- 1.git log获取commit信息 
- 2.git rebase -i (commit-id) 
- commit-id 为要删除的commit的下一个commit号 
- 3.编辑文件，将要删除的commit之前的单词改为drop 
- 4.保存文件退出大功告成 
- 5.git log查看