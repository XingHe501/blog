---
title: docker容器使用
description: ''
date: 2023-05-26 15:17:29
tags: docker
categories: 教程
---

```cmd
runoob@runoob:~$ docker run -i -t ubuntu:15.10 /bin/bash
root@0123ce188bd8:/#
```

各个参数解析
- -t: 在新容器中指定一个伪终端或者终端
- -i: 允许你对容器内的标准输入进行交互