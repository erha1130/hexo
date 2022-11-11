---
title: 网络配置和系统管理操作
date: 2022-09-22 17:21:23
tags: linux
categories: linux
---
## VMware的三种网络连接模式
* 桥接模式  
  虚拟机直接连接外部物理网络的模式，主机起到了网桥的作用。这种模式下，虚拟机可以直接访问外部网络，并且对外部网络是可见的。
  ![linux35.png](https://s2.loli.net/2022/09/22/Qm3EnUvMYqXWh5H.png)
* NAT模式(network address transition)
  虚拟机和主机构建一个专有网络，并通过虚拟网络地址转(NAT)设备对ip进行转换。虚拟机通过共享主机ip可以访问外部网络，但外部网络无法访问虚拟机。
  ![linux37.png](https://s2.loli.net/2022/09/22/7oDTzRY5sZ8GyNF.png)
* 仅主机模式
  虚拟机只与主机共享一个专有网络，与外部网络无法通信。
  ![linux36.png](https://s2.loli.net/2022/09/22/sCG2tbpmNzdarQc.png)
### 修改静态IP
#### 1.修改ifcfg-ens33文件
命令
```dotnetcli
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```
![linux38.png](https://s2.loli.net/2022/10/05/dIDP53rY7sNGhx4.png)
#### 2.执行service network restart重启网络

### 修改ip地址后可能会遇到的问题
（1）物理机能 ping 通虚拟机，但是虚拟机 ping 不通物理机,一般都是因为物理机的防火墙问题,把防火墙关闭就行  
（2）虚拟机能 Ping 通物理机,但是虚拟机 Ping 不通外网,一般都是因为 DNS 的设置有问题  
（3）虚拟机 Ping www.baidu.com 显示域名未知等信息,一般查看 GATEWAY 和 DNS 设置是否正确
（4）如果以上全部设置完还是不行，需要关闭NetworkManager 服务   
* systemctl stop NetworkManager 关闭  
* systemctl disable NetworkManager 禁用    

（5）如果检查发现 systemctl status network 有问题 需要检查 ifcfg-ens33

[（6）物理机ping不通虚拟机](https://www.shuaishuai.asia/2022/10/12/e45b0236feab/)

### 配置主机名
#### 修改主机名称
```命令
hostname
```
进入hostname文件
```dotnetcli
vim /etc/hostname
```
修改完成后重启生效
#### 修改Linux主机的hosts映射文件
1. 打开hosts
```dotnetcli
vim /etc/hosts
```
2. 添加如下内容
```dotnetcli
192.168.137.1(物理机的真实ip) physical_machine(域名)
```
3. 重启设备
#### 修改Windows主机的hosts映射文件
（1）进入 C:\Windows\System32\drivers\etc  
（2）打开 hosts 文件并添加如下内  
```dotnetcli
192.168.38.100(虚拟机的ip) erha(域名)
```
（3）修改后的文件覆盖