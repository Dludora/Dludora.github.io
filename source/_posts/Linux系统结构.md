---
title: Linux系统常识
date: 2022-09-04 17:23:06
tags: "Linux"
---

# Linux系统结构

## Linux系统根目录结构

- **/bin** (/usr/bin，/usr/local/bin)

  是Binary的缩写，这个目录存放需要经常用到的指令

- /sbin 

  s就是super user的意思，这里存放的是系统管理员使用的指令

- **/home**

  存放普通用户的主目录，在Linux中每一个用户都有一个自己的目录，目录名一般是用户名

- **/root**

  该目录为系统管理员，也称作超级权限者的用户主目录

- /lib

  系统开机所需要的最基本的动态连接共享库，其作用类似于windows系统中的DLL文件，几乎所有的应用程序都需要这些共享库

- /lost+found

  一般是空目录，当系统非法关机时存放一些文件

- **/etc**

  所有的系统管理所需要的配置文件和子目录，比如安装mysql

- **/usr**

  类似于windows系统的program files目录

- **/boot**

  存放的是启动Linux的一些核心文件，包括一些连接文件和镜像文件

- /proc

  这个目录是一个虚拟的目录，它是系统内存的映射，访问该目录获取系统信息

- /srv

  service的缩写。存放一些服务器启动之后需要提取的数据

- /sys

  Linux内核的不同之处，该目录下安装了2.6内核中新出现的一个文件系统sysfs

- /tmp

  这个目录用来存放一些临时文件

- /dev

  类似于windows的设备管理器，把所有的硬件用文件的形式存储

- **/media**

  Linux系统会自动识别一些设备，例如U盘，光驱等，当识别之后，Linux会把识别的设备挂载到这个目录之下。

- /opt

  软件安装包的存放地点

- **/usr/local**

  软件exe存放地点

- **/var**

  存放着在不断扩充的东西，习惯将经常被修改的目录放在这个目录下，包括各种日志文件

- /selinux (security-enhanced linux)

  SELinux是一种安全子系统，可以控制程序只能访问特定文件。

## 远程操作Linux系统

使用VScode，下载ssh-remote插件

### 获取Linux系统公网IP

在终端中输入`ifconfig`，第一个inet后面的即为公网IP

在windows终端中输入

```
ssh root@IP
```

再输入密码即可远程登录到Linux系统上

## 找回root密码

1. 启动系统，在开机界面输入‘e’，进入编辑界面
2. 找到以`Linux16`开头的一串字符，在行的最后输入`init=/bin/sh`
3. 按快捷键`ctrl+X`进入单用户模式
4. 在光标闪烁的位置输入`mount -o remount,rw /`，按回车
5. 在新的一行输入`passwd`，回车
6. 重新设置密码即可
