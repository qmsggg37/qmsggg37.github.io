---
layout: post
title:  "linux显示cpu是几核"
categories: linux
tags:  linux cpu
author: qmsggg37
---

* content
{:toc}

`$(cat /proc/cpuinfo | grep "cpu cores" | wc -l)`
