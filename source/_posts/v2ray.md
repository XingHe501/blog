---
title: V2Ray教程
date: 2023-05-05 11:00:15
tags:
  - V2Ray
  - Linux
categories: "教程"
description: "在目标服务器安装v2ray通过代理转发"
---

# [fhs-install-v2ray](https://github.com/v2fly/fhs-install-v2ray)

## 一键安装

```bash
bash <(curl -s -L https://git.io/v2ray.sh)
```

## 安装和更新 V2Ray

```bash
// 安装可执行文件和 .dat 数据文件
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

### 安装最新發行的 geoip.dat 和 geosite.dat

```bash
// 只更新 .dat 数据文件
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

### **自启动**

```bash
systemctl enable v2ray
```

### **修改配置**

```bash
vim /usr/local/etc/v2ray/config.json
```

### **配置范文**

### [v2ray 生成 config](https://intmainreturn0.com/v2ray-config-gen/)

```json
{
  "inbounds": [
    {
      "port": 5555,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "b86e508f-46c4-4b0d-b753-c80c25efa212",
            "level": 1,
            "alterId": 64
          }
        ]
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
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

### 启动

```bash
systemctl start v2ray
```

### 停止

```bash
systemctl stop v2ray
```

### 重启

```bash
systemctl restart v2ray
```

### 移除 V2Ray

```bash
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
--remove
```

## **客户端下载**

Web： https://v2raya.org/
Win：https://github.com/2dust/v2rayN/releases
Linux：https://v2raya.org/docs/prologue/installation/debian/
Android：https://github.com/2dust/v2rayNG/releases
