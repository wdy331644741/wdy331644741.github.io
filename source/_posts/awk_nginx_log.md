---
title: 筛选出nginx日志中 8月份所有请求404的日志
date: 2018-04-30 03:00:34
categories:
- linux
tags:
- awk
- shell
#top: true
---

```
$awk '$4~/Aug/&&$9=="404"&&$7~/service.php/{print $0}' izhuanbei_tongji.wanglibao.com.access.log > tongji.txt
```
