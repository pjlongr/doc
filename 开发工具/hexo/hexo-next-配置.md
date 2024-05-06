---
title: hexo next 配置
comments: false
author: long
date: 2023-06-14 00:27:22
categories:
- hexo
tags:
- hexo 主题
description: hexo 主题 next 的文档
---

# 文档

[Hexo-NexT](https://hexo-next.readthedocs.io/zh_CN/latest/)



# 添加RSS

切换到blog 根目录安装hexo 插件

```
npm install --save hexo-generator-feed
```

打开根目录下的 _config.yml 

在底部添加如下设置

```
feed:
  enable: true
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  icon: icon.png
  autodiscovery: true
  template:
```

- `type`：制定类型
- `path`：生成路径
- `limit`：文章数量。使用`0`或`false`表示所有文章
- `content_limit`：文章内容限制
- `order_by`：排序

打开next主题文件夹里面的`_config.yml`,在里面配置为如下样子：(就是在`rss:`的后面加上`/atom.xml`,**注意**在冒号后面要加一个空格)

```
在菜单栏显示
# Usage: `Key: /link/ || icon`
# Key is the name of menu item. If the translation for this item is available, the translated text will be loaded, otherwise the Key name will be used. Key is case-senstive.
# Value before `||` delimiter is the target link, value after `||` delimiter is the name of Font Awesome icon.
# External url should start with http:// or https://
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  RSS: /atom.xml || fa fa-rss
  
在文章底部显示
follow_me:
  #Twitter: https://twitter.com/username || fab fa-twitter
  #Telegram: https://t.me/channel_name || fab fa-telegram
  #WeChat: /images/wechat_channel.png || fab fa-weixin
  RSS: /atom.xml || fa fa-rss
```

RSS 在本地中文乱码，部署到gitee上后就正常显示了。

# 加载进度条

打开next主题文件夹里面的`_config.yml`,

```
pace:
  enable: true
  # All available colors:
  # black | blue | green | orange | pink | purple | red | silver | white | yellow
  color: orange
  # All available themes:
  # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
  # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
  theme: minimal
```

# 动态背景

## Canvas ribbon

新增的动态背景，具体效果类似于[NeverYu](https://neveryu.github.io/neveryu/)

```
# Canvas ribbon
# For more information: https://github.com/hustcc/ribbon.js
canvas_ribbon:
  enable: true
  size: 300 # The width of the ribbon
  alpha: 0.6 # The transparency of the ribbon
  zIndex: -1 # The display level of the ribbon
```

## 配置

在`hexo/source/_data`目录下新建文件`footer.swig`

```
<script color="0,0,255" opacity="0.5" zIndex="-1" count="99" src="https://cdn.jsdelivr.net/npm/canvas-nest.js@1/dist/canvas-nest.js"></script>
```

修改`NexT _config.yml`，配置`footer`路径

```
# Define custom file paths.
# Create your custom files in site directory `source/_data` and uncomment needed files below.
custom_file_path:
  #head: source/_data/head.swig
  #header: source/_data/header.swig
  #sidebar: source/_data/sidebar.swig
  #postMeta: source/_data/post-meta.swig
  #postBodyEnd: source/_data/post-body-end.swig
  footer: source/_data/footer.swig
  #bodyEnd: source/_data/body-end.swig
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  #style: source/_data/styles.styl
```

# 美化

- [Hexo NexT主题7.8.0新版本美化 | 不会飞的章鱼 (octopuslian.github.io)](https://octopuslian.github.io/2022/04/27/hexo-next-7-8-0-theme-beautify/)
- 