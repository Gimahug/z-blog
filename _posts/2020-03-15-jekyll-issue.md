---
title: 用jekyll搭建个人博客遇到的问题及解决方法
description: 使用个人博客以来，从基础环境搭建到后期配置更改探索中遇到的问题以及找到的解决办法。
categories:
 - 博客
tags: jekyll
---

## 更换头像失败
注意jekyll（next主题）默认的头像像素为215*215，格式为GIF，名称是avatar。选好自己想要的图片，用ps将图片改成215\*215的gif，再把..\assets\images路径下的avatar.gif替换成自己的图片就好啦。

## 运行bundle exec jekyll server时报错：No repo name found
解决方法：在_config.yml中添加

```yml
repository: Simpleyyt/jekyll-theme-next
```

[github上的详细解决方案](https://github.com/jekyll/jekyll/issues/4705#issuecomment-200991736)