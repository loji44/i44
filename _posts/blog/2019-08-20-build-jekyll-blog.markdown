---
layout: post
title: Jekyll博客搭建
date: 2019-08-20 15:28:34.000000000 +08:00
tags: 
 - 博客
 - Jekyll
---

博客基于 <a href="https://jekyllrb.com" target="_blank">Jekyll</a> 博客框架来搭建，并采用 <a href="https://github.com/loji44/ExSimple" target="_blank">ExSimple</a> 作为博客主题。分两步走：

- 安装Jekyll博客的开发环境
- 配置ExSimple作为博客主题

### 一、安装Jekyll博客开发环境
Jekyll依赖Ruby语言环境，所以要先安装Ruby。我自己电脑(MacOS)本身自带了Ruby，版本为：

```bash
$ ruby -v
ruby 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin18]
```

首先，执行下面的指令来安装 Jekyll：

```bash
$ gem install bundler jekyll
```

很遗憾，我运行该指令到最后，它报了一个错误给我：

```bash
··· ···
Fetching: jekyll-sass-converter-2.0.0.gem (100%)
ERROR:  Error installing jekyll:
	jekyll-sass-converter requires Ruby version >= 2.4.0.
```

嗯... 很明显，我电脑自带的Ruby版本没满足Jekyll要求的版本。还能怎么办？那就来升级一下Ruby的版本呗 `:)`

这里通过Ruby的版本管理器<a href="http://rvm.io" target="_blank">RVM</a>(`Ruby Version Manager`)来升级Ruby的版本。先来安装RVM：

```bash
$ curl -L get.rvm.io | bash -s stable
```

RVM安装完成之后，执行下面的指令来使RVM立即生效：

```bash
$ source ~/.rvm/scripts/rvm
```

来检查一下RVM是否已经成功安装以及生效：

```bash
$ rvm -v
rvm 1.29.9 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]
```

好了，RVM安装完毕。我们来升级Ruby的版本，从Ruby官网了解到当前最新版本是`2.6.3`：
```bash
$ rvm install 2.6.3
... ...
ruby-2.6.3 - #generating default wrappers.......
ruby-2.6.3 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.6.3 - #complete
```

看上面的提示，应该是成功安装了`2.6.3`版本的Ruby，我们来检验一下：

```bash
$ ruby -v
ruby 2.6.3p62 (2019-04-16 revision 67580) [x86_64-darwin18]
```

没错了，接下来继续安装我们的Jekyll。直接执行下面的指令进行安装：
                           
```bash
$ gem install bundler jekyll
```

这个安装过程可能会有点耗时，请耐心等待安装Jekyll完成 ...

### 二、配置ExSimple作为博客主题

至此，Jekyll开发环境搭建好了。现在来配置ExSimple作为博客主题。

- 下载ExSimple主题源码

```bash
$ git clone https://github.com/loji44/ExSimple.git your_blog
$ cd your_blog
```

下载之后，直接在`_posts`目录下面创建自己的博客文章：`vim 2019-08-20-my-blog.markdown` 编辑内容如下

```bash
---
layout: post
title: 关于我的博客网站
date: 2019-08-20 15:28:34.000000000 +08:00
tags: 
 - 博客
 - Jekyll
---

这是我的个人博客。

```

- 本地调试：启动Jekyll

```bash
$ bundle install && bundle exec jekyll serve
```

启动成功之后，访问 <a href="http://127.0.0.1:4000" target="_blank">http://127.0.0.1:4000</a> 就可以看到搭建好的博客页面。

Jekyll默认监听的host是`127.0.0.0`，只能从本机电脑访问；启动Jekyll的时候使用`--host`参数让Jekyll监听的host为`0.0.0.0`。

```bash
$ bundle exec jekyll serve --host 0.0.0.0
Configuration file: /root/your_blog/_config.yml
            Source: /root/your_blog
       Destination: /root/your_blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.408 seconds.
 Auto-regeneration: enabled for '/root/your_blog'
    Server address: http://0.0.0.0:4000
  Server running... press ctrl-c to stop.
```

还可以使用`--port`参数来修改Jekyll监听的端口：

```bash
$ bundle exec jekyll serve --host 0.0.0.0 --port 8080
Configuration file: /root/your_blog/_config.yml
            Source: /root/your_blog
       Destination: /root/your_blog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.419 seconds.
 Auto-regeneration: enabled for '/root/your_blog'
    Server address: http://0.0.0.0:8080
  Server running... press ctrl-c to stop.
```

使用`jekyll serve -h`来查看Jekyll启动服务时支持的其他启动参数。

### 三、将Jekyll博客以静态资源方式部署

博客文章写完，想将博客打包成静态资源部署到Web服务器上，例如使用`Nginx`或者`Github Pages`进行部署。

- 进入博客代码根目录执行下面的指令，将博客打包成静态资源：

```bash
$ bundle install && jekyll build
```

运行完毕后，会在根目录下生成`_site`文件夹，这个文件夹就是我们的博客文章静态资源。直接将这个文件夹下的全部内容拷贝到`Nginx`的静态资源部署目录下，即可完成部署。

或者你使用的是`Github Pages`，请移步Github官网教程：<a href="https://pages.github.com" target="_blank">Github Pages</a>。

<hr />