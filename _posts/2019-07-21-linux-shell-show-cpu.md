---
layout: post
title:  "Linux显示cpu是几核"
categories: Linux
tags:  Linux cpu
author: qmsggg37
---

* content
{:toc}

`$(cat /proc/cpuinfo | grep "cpu cores" | wc -l)`
