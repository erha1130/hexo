---
title: 物理机ping不通虚拟机
date: 2022-10-12 12:14:17
tags: errors
categories: errors
---

#### 物理机ping不通虚拟机，一般是网卡的问题，如果使用的是nat模式，就将VMnet8网卡禁用，让后重新启用即可解决问题

![linux44.png](https://s2.loli.net/2022/10/12/Rg72v16PquiFplf.png)