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

## 在博文里插入图片
将图片保存在/assets/images/post-image里，如pics.jpg。然后在md文件里用相对路径引用：

![img](https://Gimahug.github.io/z-blog/assets/images/post-image/pics.jpg)

此时在本地服务器打开页面可以看到图片，push到github后需要把site.url改成pages实际生成的网站地址。

## git push时出现error:failed to push some refs to
[参考博客->](https://www.jianshu.com/p/c6f2e1ca2999)
这是因为在github远程仓库修改了代码而本地没有修改产生的冲突。需要先把远程库同步到本地再push

```
git pull --rebase origion gh-pages/master
git push origin gh-pages/master
```

## git配置公钥后仍需手动输入用户名密码
[参考博客1](https://blog.csdn.net/dreamstone_xiaoqw/article/details/78355873)，[参考博客2](https://blog.csdn.net/u012150360/article/details/93710781)
+ 首先服务器端：修改C:\Program Files\Git\etc\ssh\ssh_config文件

```
Host *

    RSAAuthentication yes
    PubkeyAuthentication yes
    GSSAPIAuthentication yes
```
前两行需要添加，第三行需要修改

+ 然后配置客户端：

```
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa
```
搞定！