---
title: 忽略git文件权限
date: 2018-04-28 13:02:34
categories:
- linux
tags:
- git
#top: true
---


```
root@wdy hq_passport (dev) # git status
# 位于分支 dev
# 尚未暂存以备提交的变更：
#   （使用 "git add <file>..." 更新要提交的内容）
#   （使用 "git checkout -- <file>..." 丢弃工作区的改动）
#
#	修改：      app/service/rpcserverimpl/operativerpcimpl.php
#
修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
root@wdy hq_passport (dev) # git diff
diff --git a/app/service/rpcserverimpl/operativerpcimpl.php b/app/service/rpcserverimpl/operativerpcimpl.php
old mode 100644
new mode 100755
root@wdy hq_passport (dev) # git config core.filemode false
root@wdy hq_passport (dev) # git status
# 位于分支 dev
无文件要提交，干净的工作区
root@wdy hq_passport (dev) # 
```

```
$ git config core.filemode false  // 当前版本库
$ git config --global core.fileMode false // 所有版本库
```
![image](https://user-images.githubusercontent.com/9856659/51670305-54b95900-2001-11e9-9aa8-85ebb0c0ec0f.png)


