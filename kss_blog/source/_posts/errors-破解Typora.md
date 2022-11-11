---
title: 关于破解Typora笔记软件总是被Windows安全中心删除破解文件
date: 2022-10-16 13:32:55
tags: errors
categories: errors

---

## 背景

**最近找了破解Typora笔记软件的教程，使用几天后就无法打开软件，我使用管理员权限打开后发现恢复正版了。**

## 原因

**原因是Typora文件夹里的破解文件winmm.dll被Windows安全中心删除**

## 解决

1. 将下载好的winmm.dll复制到Typora文件下
2. 进入Windows安全中心--->病毒和威胁防护--->"病毒和威胁防护"设置 下的 管理设置--->排除项 下的 添加或删除排除项--->把Typora文件夹添加进排除项
3. 重启Typora

> 注意：以上操作都是基于Windows10




