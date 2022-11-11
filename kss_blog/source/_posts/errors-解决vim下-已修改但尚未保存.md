---
title: errors-解决vim下[已修改但尚未保存]
date: 2022-10-05 23:06:07
tags: errors
categories: errors
---

# 解决vim下 [已修改但尚未保存] /bin/bash: q：未找到命令 Shell 已返回127 请按 ENTER 或其它命令继续
## 问题
```dotnetcli
[root@erha etc]# vim /etc/sysconfig/network-scripts/ifcfg-ens33  
/bin/bash: wq: 未找到命令

Shell 已返回127

请按 ENTER 或其它命令继续
```
## 原因
这时候发现不管怎么搞都会再次跳进文件并不能实现不保存退出操作。  
这里的原因可能是你输入是不是:!q？  
vim下不保存退出是:q!而不是:!q；  
使用:q!即可实现不保存退出返回目录页面；  
## 解决
按ESC键 跳到命令模式，然后输入：
```dotnetcli
:w - 保存文件，不退出 vim
:w file -将修改另外保存到 file 中，不退出 vim
:w! -强制保存，不退出 vim
:wq -保存文件，退出 vim
:wq! -强制保存文件，退出 vim
:q -不保存文件，退出 vim
:q! -不保存文件，强制退出 vim
:e! -放弃所有修改，从上次保存文件开始再编辑
```
