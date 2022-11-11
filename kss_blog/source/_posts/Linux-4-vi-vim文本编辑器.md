---
title: vi/vim文本编辑器
date: 2022-09-18 14:59:43
tags: linux
categories: linux
---
vi是Unix操作系统和类Unix操作系统中最通用的文本编辑器。  
vim编辑器是从vi发展出来的一个性能更强大的文本编辑器。可以主动的以字体颜色辨别语法的正确性，方便程序设计。vim与vi编辑器完全兼容。  
添加中文拼音输入法
![linux31.png](https://s2.loli.net/2022/09/18/hUf5kKTQz3oJlWN.png) 
![linux32.png](https://s2.loli.net/2022/09/18/TofpJgr6bc28eAa.png)
![linux33.png](https://s2.loli.net/2022/09/18/FjEvXkiLMS9I5Qq.png)

快捷键:
* Tab 补全
* super+空格 切换输入法

模式转换
![linux34.png](https://s2.loli.net/2022/09/18/N6ATeJERpV2ayHO.png)  
+ 一般模式
  - yy 复制光标当前行
  - 8yy 复制从光标所在行往下数8行
  - y$ 复制 从光标位置开始到当前行结束
  - y^ 复制 从光标位置开始到当前行顶部
  - yw 复制 当前单词
  - w 跳到下一个单词的词头
  - e 跳到下一个单词的词尾
  - b 跳到上一个单词的词头
  - p 粘贴
  - dd 删除当前行
  - 8dd 删除8行
  - d$ 删除 从光标位置开始到当前行尾
  - d^ 删除 从光标位置开始到当前行头
  - dw 删除当前单词 光标要在单词最前边
  - u 撤销
  - x 剪切
  - shift+x 删除 光标之前的字符
  - r 替换光标所在字符
  - shift+r 替换光标之后的所有字符
  - shift+$ 移动到行尾
  - shift+^ 移动到行头
  - shift+h 移动到文本开头
  - shift+g 移动到文本末尾的行头
  - 3+shift+g 移动到第三行行头 
+ 编辑模式(插入模式)  
  - i 进入编辑模式 光标位置不变
  - a 进入编辑模式 光标向后移动一个字符
  - o 进入编辑模式 光标移动到下一行行头
  - shift+i 进入编辑模式 光标跳转到行头
  - shift+a 进入编辑模式 光标跳转到行尾
  - shift+o 进入编辑模式 光标移动到上一行行头
+ 命令模式
  - :set nu 显示出行号
  - :set nonu 隐藏行号
  - :w 保存
  - :q 退出
  - :wq 保存并退出
  - :q! 不保存强制退出
  - / 查找  
    * n 跳转下一个查找的单词
    * shift+n 跳转上一个查找的单词
    * :noh 取消高亮显示
    * :s/old/new 替换当前行匹配到的第一个old为new
    * :s/old/new/g 替换当前行匹配到的所有old为new
    * :%s/old/new 替换文档中每一行匹配到的第一个old为new
    * :%s/old/new/g 替换文档张所有old为new
  

  