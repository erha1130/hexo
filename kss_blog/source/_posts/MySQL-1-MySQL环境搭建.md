---
title: MySQL环境搭建
date: 2022-09-11 08:44:31
tags: MySQL
categories: MySQL
---
## 1. MySQL的卸载
### 停止MySQL服务
按键盘上的“Ctrl + Shift + Esc”组合键，打开“任务管理器”对话框，可以在“服务”列表找到“MySQL8.0”的服务，如果现在“正在运行”状态，可以右键单击服务，选择“停
止”选项停止MySQL8.0的服务，如图所示。
![mysql1.png](https://s2.loli.net/2022/09/19/UltRA8sqrwaBWoH.png)
### 软件的卸载
#### 方式1：通过控制面板方式
直接在“控制面板”选择“卸载程序”，并在程序列表中
找到MySQL8.0服务器程序，直接双击卸载即可，如图所示。这种方式删除，数据目录下的数据不会跟着
删除。
![mysql2.png](https://s2.loli.net/2022/09/19/Cyfk4o9xuY7K6FJ.png)
#### 方式2：通过360或电脑管家
#### 方式3：通过安装包提供的卸载功能卸载
通过安装向导程序进行MySQL8.0服务器程序的卸载。
1. 再次双击下载的mysql-installer-community-8.0.26.0.msi文件，打开安装向导。安装向导会自动检测已
安装的MySQL服务器程序。
2. 选择要卸载的MySQL服务器程序，单击“Remove”（移除），即可进行卸载
![mysql3.png](https://s2.loli.net/2022/09/19/6givzRGQLtZwKFC.png)
找不到路径可以用[everthing](https://www.voidtools.com/zh-cn/)搜索
### 残余文件的清理
（1）服务目录：mysql服务的安装目录  
（2）数据目录：默认在C:\ProgramData\MySQL  
如果自己单独指定过数据目录，就找到自己的数据目录进行删除即可。
### 清理注册表
打开注册表编辑器：在系统的搜索框中输入 regedit
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQL服务 目录删除  
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\MySQL服务 目录删除  
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQL服务 目录删除  
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\MySQL服务 目录删除  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQL服务 目录删除  
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL服务删除  
notice:**注册表中的ControlSet001,ControlSet002,不一定是001和002,可能是ControlSet005、006之类**
### 删除环境变量配置
找到path环境变量，将其中关于mysql的环境变量删除  
![mysql4.png](https://s2.loli.net/2022/09/19/XiRKz5b4NCrvxVD.png)
## 2.MySQL的下载安装配置
* **MySQL Community Server 社区版本**，开源免费，自由下载，但不提供官方技术支持，适用于
大多数普通用户。
* **MySQL Enterprise Edition 企业版本**，需付费，不能在线下载，可以试用30天。提供了更多的
功能和更完备的技术支持，更适合于对数据库的功能和可靠性要求较高的企业客户。
* **MySQL Cluster 集群版**，开源免费。用于架设集群服务器，可将几个MySQL Server封装成一个
Server。需要在社区版或企业版的基础上使用。
* **MySQL Cluster CGE 高级集群版**，需付费。
### 软件的下载
1. 下载地址  
https://www.mysql.com/  
2. 打开官网，点击DOWNLOADS然后，点击 MySQL Community(GPL) Downloads
![mysql5.png](https://s2.loli.net/2022/09/19/PqYcaGs1uV9rh45.png)
3. 点击 MySQL Community Server  
Windows平台下提供两种安装文件：MySQL二进制分发版（.msi安装文件）和免安装版（.zip压缩文件）。使用二进制分发版，因为该版本提供了图形化的安装向导过程。  
在Windows 系统下推荐下载 MSI安装程序 ；点击 Go to Download Page 进行下载即可
![mysql6.png](https://s2.loli.net/2022/09/19/cTNbA4pHrP1QhuF.png)
* Windows下的MySQL8.0安装有两种安装程序
  - mysql-installer-web-community-8.0.26.0.msi 下载程序大小：2.4M；安装时需要联网安
装组件。
  - mysql-installer-community-8.0.26.0.msi 下载程序大小：450.7M；安装时离线安装即
可。推荐。   
* 如果安装MySQL5.7版本的话，选择 Archives ，接着选择MySQL5.7的相应版本即可。这里下载最近
期的MySQL5.7.34版本。
### MySQL8.0 版本的安装
* 步骤1：双击下载的mysql-installer-community-8.0.26.0.msi文件，打开安装向导。
* 步骤2：打开“Choosing a Setup Type”（选择安装类型）窗口，这里选择“Custom（自定义安装）”类型按钮，单击“Next(下
一步)”按钮。
![mysql8.png](https://s2.loli.net/2022/09/19/1AYSRjVhXpyLnZt.png)
* 步骤3：打开“Select Products” （选择产品）窗口，可以定制需要安装的产品清单。例如，选择“MySQL
Server 8.0.26-X64”后，单击“→”添加按钮，即可选择安装MySQL服务器，如图所示。采用通用的方法，可
以添加其他你需要安装的产品。
![mysql9.png](https://s2.loli.net/2022/09/19/P6lUjpcG3vf1hAD.png)  
此时如果直接“Next”（下一步），则产品的安装路径是默认的。如果想要自定义安装目录，则可以选中
对应的产品，然后在下面会出现“Advanced Options”（高级选项）的超链接  
![mysql10.png](https://s2.loli.net/2022/09/19/dGT6ISCskKQmJML.png)
![mysql11.png](https://s2.loli.net/2022/09/19/FD12tjzRLTASneP.png)
![mysql12.png](https://s2.loli.net/2022/09/19/C7Fpqzd1lEgGx4Q.png)
![mysql13.png](https://s2.loli.net/2022/09/19/BHXIwf4pylTtb32.png)
### 配置MySQL8.0
* 步骤1：单击“Next”（下一步）按钮，进入MySQL服务器类型配置窗口，如图所示。端口号一般选择默认
端口号3306。
![mysql14.png](https://s2.loli.net/2022/09/19/6Wt3uLS1niFHEkY.png)
其中，“Config Type”选项用于设置服务器的类型。单击该选项右侧的下三角按钮，即可查看3个选项，如
图所示
![mysql15.png](https://s2.loli.net/2022/09/19/YWa16hSIlZvHqOD.png)
  - **Development Machine（开发机器）** ：该选项代表典型个人用桌面工作站。此时机器上需要运行
多个应用程序，那么MySQL服务器将占用最少的系统资源。
  - **Server Machine（服务器）** ：该选项代表服务器，MySQL服务器可以同其他服务器应用程序一起
运行，例如Web服务器等。MySQL服务器配置成适当比例的系统资源。
  - **Dedicated Machine（专用服务器）** ：该选项代表只运行MySQL服务的服务器。MySQL服务器配置
成使用所有可用系统资源。
* 步骤2：单击“Next”（下一步）按钮，打开设置授权方式窗口。其中，上面的选项是MySQL8.0提供的新的
授权方式，采用SHA256基础的密码加密方法；下面的选项是传统授权方法（保留5.x版本兼容性）。
![mysql16.png](https://s2.loli.net/2022/09/19/mlrRXutWxwzTZ8K.png)
* 步骤3：单击“Next”（下一步）按钮，打开设置服务器root超级管理员的密码窗口，如图所示，需要输入
两次同样的登录密码。也可以通过“Add User”添加其他用户，添加其他用户时，需要指定用户名、允许
该用户名在哪台/哪些主机上登录，还可以指定用户角色等。此处暂不添加用户，用户管理在MySQL高级
特性篇中讲解。
* 步骤4：单击“Next”（下一步）按钮，打开设置服务器名称窗口.
![mysql17.png](https://s2.loli.net/2022/09/19/R9jMkySmnhiJQB3.png)
* 步骤5：单击“Next”（下一步）按钮，打开确认设置服务器窗口，单击“Execute”（执行）按钮。
* 步骤7：完成配置，如图所示。单击“Finish”（完成）按钮，即可完成服务器的配置。
### 配置MySQL8.0 环境变量
* 找到MySQL安装目录下的bin目录 复制路径
* 在系统环境变量中添加此路径
![mysql18.png](https://s2.loli.net/2022/09/19/olNgIB5rWFQqOJ7.png)