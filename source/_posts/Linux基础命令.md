---
title: Linux基础命令
date: 2022-09-04 16:48:43
tags: "Linux"
---

# Linux基础命令

## 关机&重启命令

`shutdown -h now`立即关机

`shutdown -h 1`分钟后关机

`shutdown -r now`现在重新启动计算机

`halt`关机

`reboot`现在重启计算机

`sync`把内存的数据同步到磁盘

**首先都要运行sync命令**

## 用户管理

### 用户登录/注销

普通用户下输入`logout`注销当前用户

普通用户下输入`su - 用户名`可切换到其他用户，如root

### 添加用户

`useradd username`在`/home/username`下生成目录

`useradd -d location username`可以自己指定生成目录

`useradd -g groupname username`

### 指定/修改密码

`passwd username`

### 删除用户

`userdel username`会保留用户的家目录

`user del -r username`会删除用户的家目录

一般情况下建议保留，可以保存下该用户的资料

### 查询用户信息

`id username`

### 组操作

`usermod -g groupname username`把一个用户放到一个组里

`groupadd groupname` 添加组

`groupdel groupname` 删除组

## 文件/目录管理

### 当前目录绝对路径

`pwd`

### 拷贝文件到指定目录下

`cp`

### 移动/重命名文件

`mv oldName newName`

`mv nowLocation targetLocation`

### 查看文件

`less`异步查看文件

### echo指令

`echo`指令输出内容到控制台

### head指令

显示文件开头部分内容

### tail指令

显示文件结尾部分内容

### cat命令

连接多个文件并打印到标准输出。

### 在文件中查找字符串

`grep`指令

### 文件打包命令

`tar`
