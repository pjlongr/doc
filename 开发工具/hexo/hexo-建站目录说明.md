---
title: hexo 建站目录说明
comments: false
author: long
date: 2023-06-14 00:12:13
categories:
- hexo
tags:
- hexo 文档
description: hexo 建站目录下的文件说明
---

- 建站

    安装hexo 完成后，新建个目录，进到对应的目录，打开 gitbash

    ```
    hexo init
    ```

- 建站完成后有这些文件

    ```
    .
    ├── _config.yml
    ├── package.json
    ├── scaffolds
    ├── source
    |   ├── _drafts
    |   └── _posts
    └── themes
    ```

- _config.yml

    网站的配置信息，您可以在此配置大部分的参数。 [配置参数讲解](https://hexo.io/zh-cn/docs/configuration)

- package.json

    应用程序的信息，以及需要安装的模块信息。

- scaffolds

    模版文件夹。新建文章时，Hexo 会根据 scaffold 中的模板文件来建立新的文件。Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。也就是说，通过hexo命令每新建一个文章，都会包含指定模板文件中的内容。

    [官网模板详述]([模版 | Hexo](https://hexo.io/zh-cn/docs/templates))

- source

    资源文件夹是存放用户资源的地方，如markdown文章。**Markdown 和 HTML 文件会被解析并放到 `public` 文件夹**，而其他文件会被拷贝过去。

- themes

    主题文件夹。Hexo 会根据主题来解析source目录中的markdown文件生成静态页面。[官网主题详述](https://hexo.io/zh-cn/docs/themes)

