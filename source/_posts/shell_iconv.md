---
title: Navcat导出数据为csv乱码问题（iconv）
date: 2019-05-27 23:00:34
categories:
- linux
tags:
- shell
#top: true
---


这是因为navcat导出csv文件的编码格式是gbk，而我们用的excel是urf8。

```
$ iconv -s -c -f UTF8 -t GBK "vs活动.csv" > ./iconv.utf8.gbk.tmp
$ mv iconv.utf8.gbk.tmp "11111.csv"
```
