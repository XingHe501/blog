---
title: v2ray的流量伪装
categories: 教程
description: "由于服务器一直被墙，所以在网上翻找了一下资料来解决被墙的服务器"
tags:
  - v2ray
  - aws
  - cloudflare
date: 2023-10-26 15:45:04
---

<!-- more -->

> 本文转载自：[v2ray 高级伪装](https://v2xtls.org/v2ray%e9%ab%98%e7%ba%a7%e6%8a%80%e5%b7%a7%ef%bc%9a%e6%b5%81%e9%87%8f%e4%bc%aa%e8%a3%85/)、[使用 cf 拯救被墙的服务器](https://v2xtls.org/v2ray%e4%bd%bf%e7%94%a8cloudflare%e4%b8%ad%e8%bd%ac%e6%b5%81%e9%87%8f%ef%bc%8c%e6%8b%af%e6%95%91%e8%a2%ab%e5%a2%99ip/)、[从阿里云获取免费 SSL 证书](https://v2xtls.org/%e4%bb%8e%e9%98%bf%e9%87%8c%e4%ba%91%e8%8e%b7%e5%8f%96%e5%85%8d%e8%b4%b9ssl%e8%af%81%e4%b9%a6/), 如文中内容有错误请到原文查看

## 前提条件

1. 一台境外 vps 服务器，本文使用的是：[亚马逊](https://aws.amazon.com/)

2. 一个域名（国内域名可使用 cdn 加速，但是需要备案），本文使用的是：[Namesilo](/2023/10/26/使用namesilo购买域名)

3. 为域名申请一个证书，参考[从阿里云获取免费 SSL 证书]()

4. 有基本 linux 技巧，能使用 vim/nano 等编辑器。

## 流量伪装的配置步骤

1. 演示域名: testweb.yuhuijie.buzz
2. 服务器: AWS Linux(CentOS)
3. web 服务器软件: Nginx

### 效果

web+websocket+tls 组合，最终效果为：http/https 方式打开域名，显示正常的网页；V2Ray 客户端请求特定的路径，例如`https://testweb.yuhuijie.buzz/awesomepath`，能科学上网；浏览器直接请求`https://testweb.yuhuijie.buzz/awesomepath`，返回”400 bad request”。即外部看起来完全是一个人畜无害的正规网站，特定手段请求特定网址才是科学上网的通道。

## 操作步骤

## 1. 配置 DNS

设置 DNS 将域名正确解析到 vps 的 ip, 如`testweb.yuhuijie.buzz`解析到`xx.xxx.xxx.xxx`

文章参阅：[使用 Cloudflare 中转流量](/2023/10/26/使用cloudflare中转流量)

可以先关闭`cf`的中转代理，使用 [DNS 解析](https://www.whatsmydns.net/)查看域名是否正确指向自己服务器的 ip
