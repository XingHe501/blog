---
title: 使用hexo搭建博客Ⅰ
tags: 
    - Hexo
    - 主题
description: '照着Argvchs的教程自己搭了一个博客，补一篇自己的教程，顺带把自己搭建过程中遇到的坑记录一下'
date: 2023-05-06 11:07:41
categories: 教程
---

<!-- more -->

{% blockquote argvchs "--- " https://argvchs.github.io/2022/04/17/hexo-blog-1/ Hexo 博客搭建教程 I %}
Hexo 是一个快速、简洁且高效的博客框架，Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页
Markdown 可以看 GitHub 的[基本撰写和格式语法](https://docs.github.com/zh/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
{% endblockquote %}
# 1. 安装 Git 和 Node.js

-   [Git](https://git-scm.com/downloads)

    完成后用 `git --version` 命令检查，有提示即安装正确

-   [Node.js](https://nodejs.org)

    选择 LTS 或 Current 版本，安装一路确认即可
    完成后用 `node -v` 命令检查，有提示即安装正确

# 2. 启动Corepack

运行`corepack enable` 启用 `Corepack`

可能会出现如下报错。使用管理员身份重新执行一下`corepack enable`就可以了
```bash
Internal Error: EPERM: operation not permitted, open 'Y:\nodejs-v16.17.0-x64\pnpm
```

启用后自动安装 `Yarn` 和 `PNPM`，这样以后的命令都可以换成喜欢的包管理器.以下命令使用的是`npm`
这里是速度参考
![pnpm-vs-npm-vs-yarn](https://pnpm.io/img/benchmarks/alotta-files.svg)

# 3. 安装Hexo

首先新建一个文件夹作为自己的博客目录，进入到博客目录安装Hexo

[Hexo官方文档](https://hexo.io/zh-cn/docs/#%E5%AE%89%E8%A3%85-Hexo)

```bash
npm install -g hexo-cli #全局安装
```
也可以局部安装
```bash
npm install hexo
```


# 4. Hexo 的一些命令

-   生成静态文件：`hexo g`
-   清空静态文件：`hexo cl`
-   在本地运行：`hexo s`
-   部署到网站：`hexo d`
-   生成静态文件并部署到网站：`hexo d -g` 或 `hexo g -d`
-   创建新文章：`hexo new <file>`

**P.S.
    1. 创建新文章命令中的 `<file>` 是文件名，标题在文章中的 `title` 参数中修改
    2. 如果是局部安装的`Hexo`,那么命令要加上`npx`,如：在本地运行：`npx hexo s`**

下一篇：[使用Hexo搭建博客Ⅱ](/2023/05/06/hexo-blog-2)