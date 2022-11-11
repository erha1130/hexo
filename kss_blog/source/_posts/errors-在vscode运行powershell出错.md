---
title: 在vscode运行powershell出错
date: 2022-09-22 17:23:10
tags: errors
categories: errors
---
问题描述：
```dotnetcli
hexo : 无法加载文件 C:\Users\HP\AppData\Roaming\npm\hexo.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 https:/go.microsoft.c
om/fwlink/?LinkID=135170 中的 about_Execution_Policies。
```
解决方案：  
在默认情况下，我们是无法执行powershell脚本的， 需要更改执行策略。
Restricted //不允许任何脚本运行
win10下更改执行策略：
![error1.png](https://s2.loli.net/2022/09/22/cDdGosyajlSJkqr.png)
![error.png](https://s2.loli.net/2022/09/22/lgWpO37Fw6xXd5I.png)
![error2.png](https://s2.loli.net/2022/09/22/ihPj2RWyZgLtFaJ.png)