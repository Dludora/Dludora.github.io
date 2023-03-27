---
title: nvm, nrm 使用
date: 2022-09-01 19:11:40
tags: "vue"
---

# NVM、NRM使用教程

## 一、NVM介绍

NVM全称`node.js version management` ，专门针对node版本进行管理的工具，通过它可以安装和切换不同版本的node.js

### 1、Nvm常用命令操作

1. `nvm list`：查看现在所有安装的node版本
2. `nvm list available`：查看node.js官方的所有版本
3. `nvm install (版本号)`：下载对应的node版本号
4. `nvm use (版本号)`：切换node版本

## 二、NRM介绍

nrm 是一个 npm 源管理器，允许你快速地在 npm源间切换。

### 1、安装

```
npm install -g nrm
```

### 2、NRM常用命令操作

1. `npm ls`：查看可选源
2. `nrm use (源)`：切换源
3. `nrm add (源)`：添加源
4. `nrm test npm`：测试速度
