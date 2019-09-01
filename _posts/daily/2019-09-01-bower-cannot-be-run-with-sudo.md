---
layout: post
title: linux下使用bower时提示bower ESUDO Cannot be run with sudo解决办法
categories: nodejs
tags: 日常 bower npm nodejs
---
* content
{:toc}

# linux下使用bower时提示bower
今天准备在使用bower安装一些东西的时候，废了老半天劲，因为需要node环境以及bower平台，安装不顺利，通过百度，最解决了这些问题：

在执行bower命令的时候，总是会报错，原来需要在命令后添加 --allow-root 选项：
```
bower install --allow-root
```
# 转载于：
> http://www.ipandao.com/articles/bower-cannot-be-run-with-sudo