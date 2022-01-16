---
layout: post
title: pip/pip3配置国内源，为安装依赖包加速
date: 2022-01-16 17:20:02.000000000 +08:00
tags: 
  - Python
---

`midkr ~/.pip && vim ~/.pip/pip.conf`编辑pip配置文件，加入以下内容：

```txt
[global]
timeout=60
index-url=https://pypi.tuna.tsinghua.edu.cn/simple
```
更多配置项可以参考官网配置说明：<a href="https://pip.pypa.io/en/stable/topics/configuration" target="_blank">https://pip.pypa.io/en/stable/topics/configuration</a>

国内可选pip源：

- 清华：https://pypi.tuna.tsinghua.edu.cn/simple
- 阿里云：http://mirrors.aliyun.com/pypi/simple
- 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple
- 华中理工大学：http://pypi.hustunique.com
- 山东理工大学：http://pypi.sdutlinux.org
- 豆瓣：http://pypi.douban.com/simple

</ hr>

参考：
- <a href="https://pip.pypa.io/en/stable/topics/configuration" target="_blank">https://pip.pypa.io/en/stable/topics/configuration</a>
- <a href="https://mirrors.tuna.tsinghua.edu.cn/help/pypi/" target="_blank">清华大学开源软件镜像站：pypi 镜像使用帮助</a>





