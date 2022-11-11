---
title: Linux-7doc2-常用基本命令
date: 2022-10-06 22:49:19
tags: linux
categories: linux
---
# 常用基本命令
**Shell 可以看作是一个命令解释器，为我们提供了交互式的文本控制台界面。我们可以
通过终端控制台来输入命令，由 shell 进行解释并最终交给内核执行。 本章就将分类介绍
常用的基本 shell 命令。**  

## 帮助命令
### man 获得帮助信息
#### 基本语法
**man[命令或配置文件]  （功能描述：获得帮助信息）**  

#### 显示说明
![linux42.png](https://s2.loli.net/2022/10/06/HbX2qc4fDT1MruR.png)
#### 案例实操
(1)看ls命令的帮助信息  
[root@hadoop101 ~]#man ls
### help 获得 shell 内置命令的帮助信息
**一部分基础功能的系统命令是直接内嵌在 shell 中的，系统加载启动之后会随着 shell 一起加载，常驻系统内存中。这部分命令被称为“内置（built-in）命令”；相应的其它命令
被称为“外部命令”。**  
#### 基本语法
**help 命令   （功能描述：获得 shell 内置命令的帮助信息）**

#### 案例实操
(1)查看cd命令的帮助信息  
[root@hadoop101 ~]# help cd

### 常用快捷键
常用快捷键 | 功能
--- | ---
ctrl+c | 停止进程
ctrl+l | 清屏，等同于 clear；彻底清屏是：reset
善用tab键 | 提示(更重要的是可以防止敲错)
上下键 | 查找执行过的命令
### 文件目录类
#### pwd 显示当前工作目录的绝对路径
**pwd:print working directory 打印工作目录**

#### 基本语法
**pwd （功能描述：显示当前工作目录的绝对路径）**

###  ls 列出目录的内容
#### 基本语法
ls [选项][日录或是文件]
#### 选项说明

选项|功能
---|---
-a|全部的文件，连同隐藏档(开头为.的文件)一起列出来(常用)
-l|长数据串列出，包含文件的属性与权限等等数据;(常用)等价于“ll"
#### 显示说明
每行列出的信息依次是:文件类型与权限链接数文件属主文件属组文件大小用byte来表示建立或最近修改的时间名字
###  cd 切换目录
#### 基本语法
**cd[参数]**

#### 参数说明
参数|功能
---|---
cd绝对路径|切换路径
cd相对路径|切换路径
cd~或者cd|回到自己的家目录
cd -|回到上一次所在目录
cd ..|回到当前目录的上一级目录
cd -P|跳转到实际物理路径，而非快捷方式路径
### mkdir 创建一个新的目录
#### 基本语法
**mkdir[选项]要创建的目录**

#### 选项说明
**-p 创建多层目录**

#### 案例实操
创建一个多级目录  
**[root@hadoop101 ~]# mkdir -p xiyou/dssz/meihouwang**
### rmdir 删除一个空的目录
#### 基本语法  
**rmdir 要删除的空目录**

### touch 创建空文件
#### 基本语法  
**touch 文件名**

#### 案例实操

```sql
root@hadoop101 ~]# touch xiyou/dssz/sunwukong.txt
```



### cp 复制文件或目录
#### 基本语法
**cp[选项] source dest(功能描述:复制source文件到dest**

#### 选项说明
选项|功能
---|---
-r|递归复制整个文件夹
#### 参数说明
参数|功能
---|---
source|源文件
dest|目标文件

### rm删除文件或目录

#### 基本语法

**rm [选项] deletefile   (功能描述：递归删除目录中所有内容)**

#### 选项说明

| 选项 | 功能                                   |
| ---- | -------------------------------------- |
| -r   | 递归删除目录中所有内容                 |
| -f   | 强制执行删除操作，而不提示用于进行确认 |
| -v   | 显示指令的详细执行过程                 |

#### 案例实操

##### 删除目录中的某些内容

```sql
[root@erha ~]# rm kss/erha/e1.txt kss/erha/e2.txt
rm：是否删除普通空文件 "kss/erha/e1.txt"？y
rm：是否删除普通空文件 "kss/erha/e2.txt"？y
```

##### 删除文件夹erha和其中的所有内容

```sql
[root@erha kss]# rm -r erha/
rm：是否进入目录"erha/"? y
rm：是否删除普通空文件 "erha/erha.txt"？y
rm：是否删除普通空文件 "erha/erha1.txt"？y
rm：是否删除普通空文件 "erha/erha2.txt"？y
rm：是否删除目录 "erha/"？y
```

### mv 移动文件与目录或重命名

#### 基本语法

1. **mv  oldNameFile  newNameFile    (功能描述：重名名)**
2. **mv  /temp/movefile /targetFolder  (功能描述： 移动文件)**

#### 案例实操

##### 重命名

```sql
happy.cfg  hello2  hello3
[root@erha atguigu]# mv happy.cfg upset.cfg
[root@erha atguigu]# ls
hello2  hello3  upset.cfg
```

```sql
[root@erha temp]# mv movefile/file1.txt movefile/file2.txt
[root@erha temp]# ls movefile
file2.txt
```

移动文件到上一级目录

```sql
[root@erha /]# touch temp/movefile/file1.txt
[root@erha /]# mv temp/movefile/file1.txt ./
[root@erha /]# ls ../
bin   dev  etc  home   lib    media  opt   root  sbin  sys  temp  usr boot  diy  file1.txt  isdiy  lib64  mnt    proc  run   srv   targetFolder  tmp   var
[root@erha /]# ls ./
bin   dev  etc  home   lib    media  opt   root  sbin  sys  temp  usr boot  diy  file1.txt  isdiy  lib64  mnt    proc  run   srv   targetFolder  tmp   var
```

### cat 查看文件内容

#### 基本语法

**cat  [选项]  要查看的文件**

#### 选项说明

| 选项 | 功能描述                   |
| ---- | -------------------------- |
| -n   | 显示所有行的行号，包括空行 |

#### 案例实操

##### 查看文件内容并显示行号

```sql
[root@erha ~]# cat initial-setup-ks.cfg -n
```

### more 文件内容分屏查看器

**more 指令是一个基于 VI 编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件 的内容。more 指令中内置了若干快捷键，详见操作说明。**

#### 基本语法

**more 要查看的文件**

#### 操作说明

| 操作   | 功能说明                         |
| ------ | -------------------------------- |
| 空格键 | 向下翻一页                       |
| Enter  | 向下翻一行                       |
| q      | 立刻离开more，不再显示该文件内容 |
| Ctrl+F | 向下滚动一屏                     |

### less分屏显示文件内容

less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加 强大，支持各种显示终端。less 指令在显示文件内容时，并不是一次将整个文件加载之后 才显示，而是根据显示需要加载内容，**对于显示大型文件具有较高的效率**。

#### 基本语法

**less  要查看的文件**

#### 操作说明

| 操作       | 功能说明                                   |
| ---------- | ------------------------------------------ |
| 空白键     | 向下翻动一页                               |
| [pagedown] | 向下翻动一页                               |
| [pageup]   | 向上翻动一页                               |
| /字符串    | 向下搜寻字符串；n:向下查找；N：向上查找；  |
| ？字符串   | 向上搜寻字符串；n：向上查找；N：向下查找； |
| q          | 离开less这个程序；                         |

#### 案例实操

```sql
[root@hadoop101 ~]# less smartd.conf
```

### echo输出内容到控制台

#### 基本语法

**echo  [选项] [输出内容]**

#### 选项说明

| 选项 | 作用                     |
| ---- | ------------------------ |
| -e   | 支持反斜线控制的字符转换 |

| 控制字符 | 作用                |
| -------- | ------------------- |
| \\\      | 输出 \本身          |
| \n       | 换行符              |
| \t       | 制表符，也就是Tab键 |

#### 案例实操

```sql
[root@erha ~]# echo "hello\nworld"
hello\nworld
[root@erha ~]# echo -e "hello\nworld"
hello
world
```

##### 显示环境变量

```sql
echo $PATH
```

### >输出重定向 和 >>追加

#### 基本语法

* **ls -l  > 文件		(功能描述：列表的内容写入文件a.txt中(覆盖写))**
* **ls -a  >>  文件     (功能描述：列表的内容追加到文件aa.txt的末尾)**
* **cat  file1  >  file2      (功能描述：将file1的内容覆盖到file2)**
* **echo  "内容"  >>  file   （功能描述：将内容追加到file)**

#### 案例实操

1. **将ls查看信息写入到文件中**

   ```sql
   ls -l > houge.txt
   ```

2. **采用echo将hello单词追加到文件中**

   ```sql
   echo hello >> houge.txt
   ```


### ln 软链接

**软链接也称符号链接，类似于Windows里的快捷方式，有自己的数据块，主要存放了链接其他文件的路径。**

#### 基本语法

**ln -s [源文件或目录] [软连接名]** 

#### 经验技巧

* **删除软链接：rm -rf 软链接名，而不是rm -rf 软链接名/**
* **如果使用rm -rf 软链接名/  删除，会把软链接对应的真实目录下的内容删掉**
* **查询：通过ll就可以查看，列表属性第一位是1，尾部会有位置指向。**

#### 案例实操

##### 创建链接文件

```shell
[root@erha /]# ln -s /root/kss/erha/dog2.txt dog 
[root@erha /]# ll                                                             总用量 32                                                                     lrwxrwxrwx.   1 root root    7 9月  17 21:40 bin -> usr/bin                   lrwxrwxrwx.   1 root root   23 10月 20 17:51 dog -> /root/kss/erha/dog2.txt 
```

##### 创建链接目录（相当于挂载）

**无链接名时系统自动命名**

```shell
[root@erha /]# ln -s /root/kss/erha                                           [root@erha /]# ll                                                             lrwxrwxrwx.   1 root root    7 9月  17 21:40 bin -> usr/bin                    lrwxrwxrwx.   1 root root   14 10月 20 17:58 erha -> /root/kss/erha  
```

### history 查看已经执行过的历史命令

#### 基本语法

**history**

#### 案例实操

```shell
[root@erha /]# history                                                         1  ls                                                                         2  cd ..                                                                      3  ls                                                                         
```

**显示刚刚使用的10行命令**

```shell
[root@erha ~]# history 10                                                     106  date '+%y-%m-%d %H-%M-%S'                                                  107  date '+%Y-%M-%D %H-%M-%S'                                                  108  date '%Y-%M-%D %H-%M-%S'                                                 109  date '+%Y-%M-%D %H-%M-%S'                                                 110  date '+%Y-%m-%d %H-%M-%S'                                                  111  date '+%Y-%m-%d %H:%M:%S'                                                  112  date -d '1 days ago'                                                     113  date -s '2035-10-20 20:26:00'                                           114  date                                                                      115  history 10                                                                                
```

**使用上述命令**

```shell
[root@erha ~]# !107                                                            date '+%Y-%M-%D %H-%M-%S'                                                     2035-30-10/20/35 20-30-18    
```

**清空历史命令**

```shell
[root@erha ~]# history -c                                                     [root@erha ~]# history                                                         1  history 
```



### 时间日期类

#### 基本语法

**data [option]...[+format]**

#### 选项说明

| **选项**           | **功能**                                           |
| ------------------ | -------------------------------------------------- |
| **-d<时间字符串>** | **显示指定的“时间字符串”表示的时间，而非当前时间** |
| **-s<日期时间>**   | **设置系统日期时间**                               |

#### 参数说明

| **参数**            | **功能**                         |
| ------------------- | -------------------------------- |
| **<+日期时间格式>** | **指定显示时使用的日期时间格式** |

### data显示当前时间

#### 基本语法

1. **date                                                                显示当前时间**
2. **date "+%Y"                                                    显示当前年份**
3. **date "+%m"                                                   显示当前月份**
4. **date "+%d"                                                    显示当前是哪一天**
5. **date "+%Y-%m-%d %H:%M:%S"                显示年月日时分秒**

> **年时分秒 最好大写**

#### 案例实操

```shell
[root@erha ~]# date "+%Y-%m-%d %H:%M%S"                                        2022-10-20 19:5909
```

### date显示非当前时间

#### 基本语法

1. **date -d '1 days ago'**                                   **显示前一天时间**
2. **date -d '-1 days ago'**                                  **显示明天时间**
3. **date -d  '1 hours ago'                                  显示前一小时**

#### 案例实操

```shell
[root@erha ~]# date -d '1 days ago'                                           2022年 10月 19日 星期三 20:18:08 CST
```

### date设置系统时间

#### 基本语法

**date -s 字符串时间**                    **将系统时间设置为字符串时间**

**ntpdate   time.nist.gov**          **同步系统时间**（如果同步不成功查看[解决报错](https://blog.csdn.net/shaotaiban1097/article/details/83153204?ops_request_misc=&request_id=&biz_id=102&utm_term=20%20Oct%2020:52:22%20ntpdate%5B6209%5D:&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-83153204.142^v59^opensearch_v2,201^v3^control_2&spm=1018.2226.3001.4187))

#### 案例实操

```shell
[root@erha ~]# date -s '2035-10-20 20:26:00'                                   2035年 10月 20日 星期六 20:26:00 CST                                             [root@erha ~]# date                                                           2035年 10月 20日 星期六 20:26:29 CST 
```

### cal查看日历

#### 基本语法

**cal [选项]                           显示本月日历**

#### 选项说明

| **-1, --one**    | **只显示当前月份(默认)**     |
| ---------------- | ---------------------------- |
| **-3, --three**  | **显示上个月、当月和下个月** |
| **-s, --sunday** | **周日作为一周第一天**       |
| **-m, --monday** | **周一用为一周第一天**       |
| **-y, --year**   | **输出整年**                 |
| **-V,--version** | **显示版本信息并退出**       |
| **-h, --help**   | **显示此帮助并退出**         |

#### 案例实操

```shell
[root@erha ~]# cal
      十月 2022     
日 一 二 三 四 五 六
                   1
 2  3  4  5  6  7  8
 9 10 11 12 13 14 15
16 17 18 19 20 21 22
23 24 25 26 27 28 29
30 31
```

## 用户管理命令

### useradd添加新用户

#### 基本语法

**useradd  用户名                                         添加新用户**

**useradd  -g  组名  用户名                          添加新用户到某个组**
