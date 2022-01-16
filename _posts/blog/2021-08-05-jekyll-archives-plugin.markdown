---
layout: post
title: 使用jekyll-archives插件自动生成文章归档页面
date: 2021-08-05 15:00:00.000000000 +08:00
tags: 
 - 博客
 - Jekyll
---

在做 <a href="https://github.com/loji44/ExSimple" target="_blank">ExSimple</a> 博客主题的时候，被一个问题困扰了一天：如何自动根据标签`tag`自动生成页面，因为写文章的时候会随时新增新的标签。我如何能在新增`tag`的时候，自动生成`tag`下面的所有文章列表页面？

首先我已经写了`tag_post_list.html`这个layout，结合<a href="https://github.com/jekyll/jekyll-archives" target="https://github.com/jekyll/jekyll-archives">jekyll-archives</a>插件自动根据tag生成文章归档页面。`_config.yml`配置如下：

```txt
plugins:
  - jekyll-archives
jekyll-archives:
  enabled: ['tags']
  layout: tag_post_list
  permalinks:
    tag: '/tags/:name.html'
```

在执行`bundle install && jekyll build`生成博客静态资源文件时，就会根据tag归档生成归档页面，生成归档html页面所存放的路径为`_site/tags/`：

```bash
loji44@Ubuntu:~/i44/_site/tags$ tree
.
├── arthas.html
├── auth.html
├── docker.html
├── dubbo.html
├── iterm2.html
├── java.html
├── jekyll.html
├── linux.html
├── macos.html
├── mybatis.html
├── mysql.html
├── python.html
├── redis.html
├── spi.html
├── spring.html
├── sso.html
├── windows.html
├── 博客.html
├── 图片.html
├── 工具集.html
└── 树莓派.html
```

<hr />
