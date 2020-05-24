---
title: git配置公钥后仍需手动输入用户名密码
description: 
categories:
 - 博客
tags: git issue
---

[->参考博客1](https://blog.csdn.net/dreamstone_xiaoqw/article/details/78355873)，[->参考博客2](https://blog.csdn.net/u012150360/article/details/93710781)
+ 首先服务器端：修改C:\Program Files\Git\etc\ssh\ssh_config文件

```
# Host *

#   RSAAuthentication yes
#   PubkeyAuthentication yes
#   GSSAPIAuthentication yes
```
前两行需要添加，第三行需要修改

+ 然后配置客户端：

```
$ ssh-agent bash
$ ssh-add ~/.ssh/id_rsa
```
搞定！