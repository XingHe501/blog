---
title: SSH远程免密登录
tags:
    - SSH
    - Linux
categories: 教程
date: 2023-05-08 11:50:13
---

使用SSH免密登录另一台服务器

<!-- more -->

### 有两台服务器，服务器A（192.168.1.1）和服务器B（192.168.1.2），在A上实现免密登录服务器B

## 一、先测试两台服务器是否可以ping通，确保网络通畅
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

## 二、在服务器A上生成公钥、私钥

可以使用 -t指定密钥类型，-b 指定大小，一般直接执行最普通的`ssh-keygen`就可以，弹出来的提示直接默认回车
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

尝试连接服务器B

```bash
ssh root@192.168.1.2
```