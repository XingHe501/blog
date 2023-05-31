---
title: Nginx安装与配置
date: 2023-05-05 11:01:47
tags: nginx
categories: Linux
description: ""
---

## 一、安装编译工具及库文件

```shell
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

## 二、安装pcre

PCRE的作用主要是让nginx支持rewrite功能

1. 下载PCER安装包，下载地址：http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz

```shell
[root@bogon src]# cd /usr/local/src/
[root@bogon src]# wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
```

2. 解压和安装

```shell
    tar zxvf pcer-8.35.tar.gz
    cd pcer-8.35
    ./configure
    make && make install
# 查看版本
    pcre-config --version
```

## 三、安装nginx

1. 下载nginx，Centos8.4安装版本：https://nginx.org/download/nginx-1.20.2.tar.gz

```shell
[root@bogon src]# cd /usr/local/src/
[root@bogon src]# wget https://nginx.org/download/nginx-1.20.2.tar.gz
```

2. 解压

```shell
tar zxvf nginx-1.20.2.tar.gz
cd nginx-1.20.2
```

4. 编译安装

```shell
 /configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/local/src/pcre-8.35
 make
 make install
```

5. 设置环境变量

```shell
 vim /etc/profile
 # 在末尾加入
 PATH=$PATH:/usr/local/webserver/nginx/sbin
 export $PATH
 # source 读取并执行filename的命令
 source /etc/profile
```

6. 查看nginx版本

```shell
nginx -v
```

# 四、Nginx的设置

``` conf
    # Nginx目录浏览功能
    autoindex on;
    # 默认为on，显示出文件的确切大小，单位是bytes
    # 显示出文件的大概大小，单位是kB或者MB或者GB
    autoindex_exact_size off;
    #默认为off，显示的文件时间为GMT时间。
    #改为on后，显示的文件时间为文件的服务器时间  
    autoindex_localtime on;
    # 让浏览器不保存临时文件
    add_header Cache-Control no-store;
    # 避免中文乱码
    charset utf-8,gbk; 
    # 如果希望请求文件是下载而不是显示内容，可以通过添加下面参数实现：
    add_header Content-Disposition attachment;
```

# 五、Nginx 设置开机启动

1.创建nginx.service 文件

```shell
vim /etc/systemd/system/nginx.service
```

2. 配置文件内容如下

```conf
[Unit]

Description=nginx service

After=network.target

[Service]

Type=forking

ExecStart=/usr/local/webserver/nginx/sbin/nginx

ExecReload=/usr/local/webserver/nginx/sbin/nginx -s reload

ExecStop=/usr/local/webserver/nginx/sbin/nginx -s stop

PrivateTmp=true

[Install]

WantedBy=multi-user.target
```

3. 设置开机启动

```shell
systemctl enable nginx.service
```