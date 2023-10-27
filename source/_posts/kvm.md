---
title: Linux 安装KVM
date: 2023-05-04 19:05:56
tags:
  - KVM
  - Linux
categories: 教程
description: "在Linux系统安装KVM"
---

# 安装 kvm 步骤

## 虚拟机安装：

### 查看硬件是否支持虚拟化

```shell
# grep -Ei 'vmx|svm' /proc/cpuinfo/more
```

### 安装虚拟机

```shell
# yum intall -y virt-* libvirt qemu-img
```

### 是否支持 kvm 模块

```shell
# lsmod | grep kvm
```

### 系统启动时自动启动虚拟机服务

```shell
# systemctl enable libvirtd
```

### 启动虚拟机服务

```shell
# systemctl start libvirtd
```

### 验证是否可用

```shell
# virsh list -all
```

<br/>

## 安装使用 Cockpit：

### 安装 Cockpit

```shell
# yum install cockpi
```

### 开机自启动 Cockpit

```shell
# systemctl enable --now cockpit.socket
```

### 安装软件包使 cockpit 支持 WEB 管理

```shell
# yum install cockpi-machines
```

使用端口 9090 通过浏览器访问主机<br/><br/>

## 网桥配置：

最好在物理机上操作，远程容易无法再次连接

### 新建网桥

```shell
# nmcli connection add type bridge con-name br0 iframe br0 autoconnect yes
```

## 修改网桥配置文件

#### 按照自己实际情况更改 ip、网关、dns、关闭 IPV6 等

```shell
# vim /etc/sysconfig/network-scripts/ifcfg-br0
```

## 执行桥接：

#### 执行后，在网络配置文件夹下出现一个名为 ifcfg-bridge-slave-eno1 的新文件。

```shell
# nmcli connection add type bridge-slave ifname eno1 master br0
```

## 关闭物理网卡

#### <font color="red">注意网络名称</font>

```shell
# nmcli connection down eno1
```

## 开启桥接：

```shell
# nmcli connection up br0
```

## 查看连接和网络信息：

```shell
# nmcli connection
# ifconfig
```

## 关闭 NAT 虚拟接口：

```shell
# virsh net-destory default
# virsh net-undefine default
# systemctl restart libvirtd
# virsh net-list
```

## 重新加载网络配置

```shell
# nmcli c reload
```

# 在过程中遇到的问题记录

## 在 VCN 中无法显示

#### 提示内容：

`.`

#### 解决办法，打开 bios 中的 Intel 显示技术选项:

```
BIOS -> ADVANCE -> CPU -> Intel Visualization Technology
```
