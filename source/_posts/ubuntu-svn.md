---
title: 解决Ubuntu使用SVN命令每次都要输入密码的问题
tags:
  - ubuntu
  - svn
categories: 教程
description: "起因是公司要用到WSL进行开发，在windows下安装了Ubuntu使用svn co或者svn up的时候每次都要输入密码"
date: 2023-05-06 18:32:13
---

<!-- more -->

**P.S. 这里可以直接从步骤 3 开始，也可以从步骤 1 开始**

# 1. 直接删除掉原来的配置

```bash
rm -rf ~/.subversion
```

# 2. 执行一次 svn 命令输入账号密码

```bash
svn up
```

执行完命令之后会在`~/.subversion/auth/svn.simple`目录下生成一个文件，我这边的名字叫
`201ec234477d9acdbef432a0ff4519af`

```bash
~/projects$ ls ~/.subversion/auth/svn.simple/
201ec234477d9acdbef432a0ff4519af
```

# 3. 修改生成的文件

```bash

cd ~/.subversion/auth/svn.simple/
vim 201ec234477d9acdbef432a0ff4519af

# 或者直接编辑
vim ~/.subversion/auth/svn.simple/201ec234477d9acdbef432a0ff4519af
```

# 4.修改内容如下, passtype 固定是 simple, K 和 V 后面的数字分别代表 key 和 value 的长度, 注意 SVN 服务器的地址

```text
K 15
svn:realmstring
V 44
<https://xxx.xxx.xxx.xxx> VisualSVN Server
K 8
passtype
V 6
simple
K 8
username
V 4  # 4 代表你 登陆账户 的长度
root # 这个地方填你的登录账户，
K 8
password
V 4  # 4 代表你 登录密码 的长度
root # 这个地方填你的登录密码，
END
```
