---
title: v2ray的流量伪装
categories: 教程
description: "由于服务器一直被墙，所以在网上翻找了一下资料来解决被墙的服务器"
tags:
  - V2Ray
  - AWS
  - Cloudflare
  - Linux
date: 2023-10-26 15:45:04
---

<!-- more -->

> 本文转载自：[v2ray 高级伪装](https://v2xtls.org/v2ray%e9%ab%98%e7%ba%a7%e6%8a%80%e5%b7%a7%ef%bc%9a%e6%b5%81%e9%87%8f%e4%bc%aa%e8%a3%85/) 如文中内容有错误请到原文查看

## 前提条件

1. 一台境外 vps 服务器，本文使用的是：[亚马逊](https://aws.amazon.com/)

2. 一个域名（国内域名可使用 cdn 加速，但是需要备案），本文使用的是：[Namesilo](/2023/10/26/使用namesilo购买域名)

3. 为域名申请一个证书，参考[从阿里云获取免费 SSL 证书](/2023/10/26/从阿里云获取免费SSL证书)

4. 有基本 Linux 技巧，能使用 vim/nano 等编辑器。

## 流量伪装的配置步骤

1. 演示域名: testweb.yuhuijie.buzz
2. 服务器: AWS Linux(CentOS)
3. web 服务器软件: Nginx

### 效果

web+websocket+tls 组合，最终效果为：http/https 方式打开域名，显示正常的网页；V2Ray 客户端请求特定的路径，例如`https://testweb.yuhuijie.buzz/awesomepath`，能科学上网；浏览器直接请求`https://testweb.yuhuijie.buzz/awesomepath`，返回”400 bad request”。即外部看起来完全是一个人畜无害的正规网站，特定手段请求特定网址才是科学上网的通道。

## 操作步骤

服务端涉及到了 Nginx 和 v2ray，分别介绍其配置。

## 1. 配置 DNS

设置 DNS 将域名正确解析到 vps 的 ip, 如`testweb.yuhuijie.buzz`解析到`xx.xxx.xxx.xxx`

文章参阅：[使用 Cloudflare 中转流量](/2023/10/26/使用cloudflare中转流量)

可以先关闭`cf`的中转代理，使用 [DNS 解析](https://www.whatsmydns.net/)查看域名是否正确指向自己服务器的 ip

## 2. 配置 Nginx

如果你的域名并正确配置了 SSL 证书，可忽略这一步。

Nginx 是市面上占有率最高的网站服务器软件，Centos 系统安装 nginx 命令：`yum install -y nginx`。

Linux 系统上 Nginx 默认站点配置文件是`/etc/nginx/conf.d/`目录下的 `default.conf`，我们对伪装网站进行全站 https 配置，示例内容如下：

```ini
server {
    listen 80;
    server_name xxxxx;  # 改成你的域名
    rewrite ^(.*) https://$server_name$1 permanent;
}

server {
    listen       443 ssl http2;
    server_name xxxxx;
    charset utf-8;

    # ssl配置
    ssl_protocols TLSv1.2 TLSv1.3; # tls 1.3要求nginx 1.13.0及以上版本
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_certificate xxxxx; # 改成你的证书地址
    ssl_certificate_key xxxx; # 改成证书密钥文件地址

    access_log  /var/log/nginx/xxxx.access.log;
    error_log /var/log/nginx/xxx.error.log;

    root /usr/share/nginx/html;
    location / {
        index  index.html;
    }
}
```

配置好用 `nginx -t` 命令查看有无错误，没问题的话 `systemctl restart nginx` 启动 Nginx。打开浏览器在地址栏输入域名，应该能看到 https 访问的 Nginx 欢迎页。

新域名如何快速做一个像模像样的网站？最简单的办法是从网上下载网站模板，上传到 web 服务器的根目录（默认是`/usr/share/nginx/html`）。对于伪装站来说，静态站足够。如果你的境外流量比较大，建议用爬虫或者其他手段做一个看起来受欢迎、流量大的站点，例如美食博客，图片站等。

## 3.安装配置 V2Ray

详细过程可参考：[V2Ray 教程](/2023/05/05/v2ray)

到此为止，Nginx 和 V2ray 应该都能各自独立正常工作。如果有一个出现问题，应该先解决再继续下面的操作。

## 4.Nginx 配置 websocket

接下来我们让 Nginx 和 v2ray 结合，完成服务端的配置。

首先我们选择一个<strong style="color:red; font-size:120%">伪装路径</strong>，建议为二级或者较长的一级路径，例如`/abc/def` 或`/awesomepath`。

配置 Nginx 将伪装路径的访问都转发到 `v2ray`。编辑`/etc/nginx/conf.d/default.conf` 的第二个 server 段，增加以下转发配置：

```ini
location /awesomepath { # 与 V2Ray 配置中的 path 保持一致
      proxy_redirect off;
      proxy_pass http://127.0.0.1:12345; # 假设v2ray的监听地址是12345
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```

配置好后重启 Nginx：`systemctl restart nginx`。

配置 v2ray 接受 Nginx 传来的数据。编辑 `/etc/v2ray/config.json` 文件，在“inbounds”中新增“streamSetting”配置，设置传输协议为“websocket”。配置好后 `config.json` 文件看起来是：

```ini
{
  "log": {
    "loglevel": "warning",
    "access": "/var/log/v2ray/access.log",
    "error": "/var/log/v2ray/error.log"
   },
  "inbounds": [{
    "port": 12345,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "xxxxx", # 可以使用/usr/bin/v2ray/v2ctl uuid生成
          "level": 1,
          "alterId": 0
        }
      ]
    },
    "streamSettings": {     # 载体配置段，设置为websocket
        "network": "ws",
        "wsSettings": {
          "path": "/awesomepath"  # 与nginx中的路径保持一致
        }
      },
    "listen": "127.0.0.1" # 出于安全考虑，建议只接受本地链接
  }],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  },{
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

> ## _注意_：
>
> 1. _json 文件不支持注释，上述配置中”#”号及后续内容都要删掉；_
>
> 2. _因为使用 tls 加密，强烈建议 alterId 改成 0，以节省 cpu 加快 v2ray 的速度_

配置无误后，重启 v2ray 服务：`systemctl restart v2ray`。

如何测试 nginx 与 v2ray 结合没有问题？打开浏览器，输入域名及其他路径，应该显示正常网页或者页面不存在，说明 Nginx 正常工作；输入域名加 v2ray 路径，例如`https://testweb.yuhuijie.buzz/awesomepath`，应该出现”Bad Request”，说明 Nginx 将流量转发给了 v2ray，并且 v2ray 收到了请求。
