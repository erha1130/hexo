---
title: errors-报错Bad owner or permissions on C:\Users\Administrator/.ssh/config
date: 2022-10-12 13:01:32
tags: errors
categories: errors
---

## 问题

**在使用powershell时遇到上面报错**

## 原因

**.ssh文件权限不够**

## 解决

**1.找到.ssh文件夹。它通常位于C:\Users，例如C:\Users\Akkuman。**
**2.右键单击.ssh文件夹，然后单击“属性”。**
**3.找到并点击“安全”标签。**
**4.然后单击“高级”。 单击“禁用继承”，单击“确定”。 将出现警告弹出窗口。单击“从此对象中删除所有继承的权限”。**
**5.你会注意到所有用户都将被删除。让我们添加所有者。在同一窗口中，单击“编辑”按钮。**
**6.接下来，单击“添加”以显示“选择用户或组”窗口。**
**7.单击“高级”，然后单击“立即查找”按钮。应显示用户结果列表。 选择您的用户帐户。**

————————————————

参考链接：https://blog.csdn.net/IT_Holmes/article/details/119364817

