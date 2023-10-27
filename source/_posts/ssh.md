---
title: 使用ssh免密登录服务器
tags: 
	- ssh
	- Linux
categories: 教程
description: ""
date: 2023-08-08 12:11:47
---

使用 SSH 免密登录另一台服务器

<!-- more -->

### 有两台服务器，服务器 A（192.168.1.1）和服务器 B（192.168.1.2），在 A 上实现免密登录服务器 B

## 一、先测试两台服务器是否可以 ping 通，确保网络通畅

```bash
ping 192.168.1.2
#PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
#64 bytes from 192.168.1.2: icmp_seq=1 ttl=128 time=1.44 ms
#64 bytes from 192.168.1.2: icmp_seq=2 ttl=128 time=0.348 ms
#64 bytes from 192.168.1.2: icmp_seq=3 ttl=128 time=0.305 ms
#64 bytes from 192.168.1.2: icmp_seq=4 ttl=128 time=16.2 ms
#64 bytes from 192.168.1.2: icmp_seq=5 ttl=128 time=4.57 ms
#^C
#--- 192.168.1.2 ping statistics ---
#9 packets transmitted, 9 received, 0% packet loss, time 8004ms
#rtt min/avg/max/mdev = 0.263/2.674/16.224/4.971 ms

```

## 二、在服务器 A 上生成公钥、私钥

可以使用 -t 指定密钥类型，-b 指定大小，一般直接执行最普通的`ssh-keygen`就可以，弹出来的提示直接默认回车

```bash
# ssh-keygen -t ed25519
# ssh-keygen -t rsa -b 4096
ssh-keygen
```

## 三、拷贝公钥到目标服务器

-i 指定刚刚生成的公钥地址

```bash
ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.1.2
```

## 四、连接

尝试连接服务器 B

```bash
ssh root@192.168.1.2
```
