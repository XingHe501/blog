---
title: 在ParticleX上使用Waline
tags:
  - ParticleX
  - Vercel
  - Netlify
  - Waline
categories: 教程
date: 2023-05-08 11:50:13
---

看了一些部署 Waline 的文档，写的感觉都不太全面

<!-- more -->

# 0. 前言

**P.S. Vercel 和 Netlify 选择其一即可，Netlify 不需要代理就可访问，所以个人更向于 Netlify。当然最好的办法还是[通过自定义域名来激活 Waline 的服务](https://lisenhui.cn/blog/use-custom-domain-active-vercel-waline.html)**

> [Waline 官网](https://waline.js.org/) > [Waline 仓库地址](https://github.com/walinejs/waline)

可以跟着官方的教程走，也可以很快的搭建好

# 1. 部署 LeanCloud

1. [登录](https://console.leancloud.app/login)或者[注册](https://console.leancloud.app/register) `LeanCloud 国际版` 并进入 [控制台](https://console.leancloud.app/apps)

1. 点击左上角`创建应用`，输入一个自己喜欢的名字并选择免费的`开发版`

1. 点击刚刚创建的应用，选择左下角的 `设置` > `应用凭证`。你可以看到你的 `APP ID`, `APP Key` 和 `Master Key`。请记录它们，以便后续使用。

# 2. 部署 Waline

## 2.1 Vercel 部署

[![Vercel](https://vercel.com/button#pic_center)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fwalinejs%2Fwaline%2Ftree%2Fmain%2Fexample)

1. 点击上方按钮，跳转至 Vercel 进行 Server 端部署。

   > 如果你未登录的话，Vercel 会让你注册或登录，请使用 GitHub 账户进行快捷登录。

1. 在右侧`Repository Name`输入一个你喜欢的 Vercel 项目名称并点击 `Create` 继续:

1. 此时 Vercel 会基于 Waline 模板帮助你新建并初始化仓库，仓库名为你之前输入的项目名。

   一两分钟后，满屏的烟花会庆祝你部署成功。此时点击 `Go to Dashboard` 可以跳转到应用的控制台。

1. 点击顶部的 `Settings` - `Environment Variables` 进入环境变量配置页，并配置三个环境变量 `LEAN_ID`, `LEAN_KEY` 和 `LEAN_MASTER_KEY` 。它们的值分别对应上一步在 LeanCloud 中获得的 `APP ID`, `APP KEY`, `Master Key`。

   ![设置环境变量](/images/vercel-2.png)

1. 环境变量配置完成之后点击顶部的 `Deployments` 点击顶部最新的一次部署右侧的 `Redeploy` 按钮进行重新部署。该步骤是为了让刚才设置的环境变量生效。

   ![redeploy](/images/vercel-3.png)

1. 此时会跳转到 `Overview` 界面开始部署，等待片刻后 `STATUS` 会变成 `Ready`。此时请点击 `Visit` ，即可跳转到部署好的网站地址，此地址即为你的服务端地址。

   ![redeploy success](/images/vercel-4.png)

## 2.2 在 Netlify 上部署 Waline

    [官方教程](https://waline.js.org/guide/deploy/netlify.html#%E5%A6%82%E4%BD%95%E9%83%A8%E7%BD%B2)

# 3. 注册成为 Waline 管理员账号

1. 部署完成之后，在你服务端地址后面加上 `/ui/register` 进行注册。首个注册的人被设定为管理员
2. 管理员登陆后，即可看到评论管理界面。在这里可以修改、标记或删除评论。
3. 用户也可通过评论框注册账号，登陆后会跳转到自己的档案页。

# 4. 打开主题文件夹中的`_config.yml`文件，将下面内容粘贴进去

```yaml
waline:
  # New! Whether enable this plugin
  enable: true
  # Waline server address url, you should set this to your own link
  serverURL: #此处填写你Vercel或者Netlify部署的网址
  # Custom locales
  # locale:
  #   placeholder: Welcome to comment # Comment box placeholder
  # If false, comment count will only be displayed in post page, not in home page
  commentCount: true
  # Pageviews count, Note: You should not enable both `waline.pageview` and `leancloud_visitors`.
  pageview: false
  # Custom emoji
  emoji:
    - https://unpkg.com/@waline/emojis@1.0.1/weibo
    - https://unpkg.com/@waline/emojis@1.0.1/alus
    - https://unpkg.com/@waline/emojis@1.0.1/bilibili
    - https://unpkg.com/@waline/emojis@1.0.1/qq
    - https://unpkg.com/@waline/emojis@1.0.1/tieba
    - https://unpkg.com/@waline/emojis@1.0.1/tw-emoji
  # Comment infomation, valid meta are nick, mail and link
  meta:
    - nick
    - mail
    - link
  # Set required meta field, e.g.: [nick] | [nick, mail]
  requiredMeta:
    - nick
  # Language, available values: en-US, zh-CN, zh-TW, pt-BR, ru-RU, jp-JP
  lang: zh-CN
  # Word limit, no limit when setting to 0
  wordLimit: 0
  # Whether enable login, can choose from 'enable', 'disable' and 'force'
  login: enable
  # comment per page
  pageSize: 10
```

由于 ParticleX 内置 Waline，所以只需要更改 serverUrl 即可

其他主题不支持 Waline，需要把 HTML 引入到客户端的可以查看[官方文档](https://waline.js.org/guide/get-started/#html-%E5%BC%95%E5%85%A5-%E5%AE%A2%E6%88%B7%E7%AB%AF)

# 5. 评论通知

    [评论通知](https://waline.js.org/guide/features/notification.html)

## 5.1 评论样式设置

    [评论样式一览](https://www.sarakale.top/blog/posts/537344b2.html#post-comment)
