---
layout: install_pnpm_on_windows
title: windows
tags:
  - pnpm
  - nodejs
categories: 
  - 教程
  - windows
date: 2023-07-04 23:43:49
---

在自己电脑上安装pnpm时遇到了一些问题，无法安装

<!-- more -->

# 使用npm安装pnpm完成之后，运行pnpm提示无法把C盘路径下的pnpm视作命令

在stackflow上找到了解决问题，原文：[How to install pnpm on Windows?](https://stackoverflow.com/questions/75365692/how-to-install-pnpm-on-windows)


## Step.1 Open command and run this command.

```shell
set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

When you run this command, you can see that your system has set all policies for the current user as remotely. It will take few seconds to complete this process.

## Step 2: Now you have to run the second command on your system. This command is:

``` shell
Get-ExecutionPolicy
```

When you have run this command your system has a show “RemoteSigned”. If you have received this message, then your problem will be solved. Now you have to go to the next step to view the list of policies which policy has been updated by the last commands.

## Step 3: To view their policy, you need to run this command in your command prompt:

```shell
Get-ExecutionPolicy -list
```