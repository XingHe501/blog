---
title: Windows安装Python3.11安装
tags: python
categories: 教程
description: ''
date: 2023-05-30 15:45:44
---

# 一、下载

- [Python3.11.3](https://www.python.org/ftp/python/3.11.3/python-3.11.3-amd64.exe)
- 选择需要的版本：[官方网站](https://www.python.org/downloads/windows/)

# 二、安装

省流：
1. 打开安装包
2. 勾选 `Add python.exe to Path`, 点击`Customize installation`.
3. `Next`.
4. 勾选`Install Python 3.11 for all users`.
5. 选择安装路径，点击`Install`
6. 打开`cmd`, 输入`python -V`查看python版本，如果输出版本代表安装成功

## 2.1 选项详解

点击 `Customize Installaton` 进行下一步

- `Install Now`：直接安装到当前的用户目录
- `Customize Installation`: 自定义安装
- `Use admin privileges when install py.exe`: 安装python时使用admin权限。**必选**
- `Add python.exe to PATH`: 把python.exe添加到环境变量。**必选**

![](/images/python-tutorial/uTools_1685436608857.png)


## 2.2 可选功能f
直接点击`Next`进行下一步即可

- `Documentation`: Python文档。 默认安装。
- `pip`: Python的包管理工具，可以用来下载安装Python的其他包。默认安装。
- `tcl/tk and IDLE`: `tkinter` 和 `IDLE`安装环境。默认安装。
- `Python test suite`: 标准库测试套件。默认安装。
- `py launcher`; `for all users(requires admin privileges)`： 安装全局的python解释器，以便更简单的启动python； 默认安装。

![](/images/python-tutorial/uTools_1685437140151.png)


## 2.3 进阶选择

`Customize install location`选择Python安装地址，点击`Install` 进行安装

![](/images/python-tutorial/uTools_1685440509953.png)


- `Install Python 3.11 for all users`: 安装在全局位置，勾选此选项,安装路径会自动更改到`Program Files`路径下。如图

![](/images/python-tutorial/uTools_1685440749172.png)

- `Associate files with Python`: 把文件跟python相关联，需要py解释器

- `Create shortcuts for installed applications`: 创建快捷方式

- `Add Python to environment variables`: 添加到环境变量

- `Precompile standard libary`: 预编译标准库。可选

- `Download debugging symbols`: 安装调试模块

- `Download debug binaries`: 下载debug二进制，需要VS 2017或者更高版本

# 三、查看python版本

打开`cmd`, 输入`python -V`命令
```cmd
> python -V
Python 3.11.2
```