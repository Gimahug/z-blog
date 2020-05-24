---
title: git push时出现error:failed to push some refs to
description: 
categories:
 - 博客
tags: git issue
---

[->参考博客](https://www.jianshu.com/p/c6f2e1ca2999)
这是因为在github远程仓库修改了代码而本地没有修改产生的冲突。需要先把远程库同步到本地再push

```
git pull --rebase origin gh-pages
git push origin gh-pages
```