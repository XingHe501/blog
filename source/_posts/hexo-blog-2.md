---
title: 使用Hexo搭建博客 Ⅱ
tags: 
    - Hexo
    - 主题
description: ''
date: 2023-05-06 14:29:07
categories: 教程
---

上文：[使用Hexo搭建博客Ⅰ](/2023/05/06/hexo-blog-1)

<!-- more -->

# 1.安装主题

首先在[这里](https://hexo.io/themes)选择一个主题

上面的图片是预览页，下面的蓝色链接是 GitHub 项目页

选好主题后就要安装，在博客根目录下运行下面的命令将主题 Clone 到本地

```bash
cd themes
git clone <link>.git <theme> --depth=1
```

> `--depth=1` 是为了 Clone 更快，只 Clone 最新提交

GitHub 打不开记得使用科学上网

比如我的主题是 [ParticleX](https://github.com/argvchs/hexo-theme-particlex)，就是 `git clone https://github.com/argvchs/hexo-theme-particlex.git particlex --depth=1`

安装完成后，在博客根目录下的 `_config.yml` 中设置 theme 参数为你的主题名称，就可以切换主题，一般主题在 GitHub 项目页下都会有介绍和配置说明，可以按照说明自定义页面

# 2.创建特殊界面

## 2.1. 标签分类页

输入命令 `hexo new page categories` 创建分类页
打开 `source/categories/index.md`，在 `---` 括起来的地方添加 `type: categories`

输入命令 `hexo new page tags` 创建标签页
打开 `source/tags/index.md`，在 `---` 括起来的地方添加 `type: tags`

## 2.2. 关于页

输入命令 `hexo new page about` 创建关于页
打开 `source/about/index.md` 在下面添加内容即可

---

如果想让标题大写的话可以将 `title` 参数改为大写，例如 `title: About`
虽然 ParticleX 是根据 `type` 参数来检测的，**但是一些主题是根据 `title` 检测的，可能检测不到**

# 3. 自定义网站配置

打开博客根目录下的 `_config.yml`，下面是主要参数的介绍

```yaml
title: # 标题
subtitle: # 副标题
description: # 描述
keywords: # 关键字
author: # 作者
language: # 语言
timezone: # 时区
url: # 网址
root: # 根目录
permalink: # 文章链接格式
permalink_defaults: # 链接默认值
source_dir: # 源文件夹，内容的存储位置
public_dir: # 公用文件夹，静态文件的生成位置
tag_dir: # 标签目录
archive_dir: # 存档目录
category_dir: # 分类目录
skip_render: # 复制到原始路径，不进行渲染
new_post_name: # 新帖子的文件名格式
titlecase: # 将标题转换为小写1/大写2
external_link.enable: # 在新标签页中打开外部链接
post_asset_folder: # 启用资源文件夹功能
filename_case: # 将文件名转换小写小写1/大写2
relative_link: # 是否创建相对于根文件夹的链接
index_generator.per_page: # 每页显示的文章数
index_generator.order_by: # 发布顺序
date_format: # 日期格式
time_format: # 时间格式
per_page: # 每个页面上显示的文章数
pagination_dir: # 网址格式
theme: # 主题名称
theme_config: # 主题配置，覆盖主题默认值
deploy: # 部署设置
include: # 包括隐藏文件
exclude: # 排除文件/文件夹
ignore: # 忽略文件/文件夹
```

由于参数过多，只写了一部分，其他可以参考 [Hexo 文档](https://hexo.io/zh-cn/docs/configuration.html)

**P.S.：`titlecase` 参数改变后部分主题可能检测不到 Categories 和 Tags 页面**

下一篇：[Hexo 博客搭建教程 III](/2023/05/06/hexo-blog-3)