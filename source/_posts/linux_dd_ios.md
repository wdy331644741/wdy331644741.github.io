---
title: Linux制作开机启动盘
date: 2019-12-07 23:00:34
categories:
- linux
tags:
- shell
#top: true
---


1. 格式化u盘
2. 推出u盘

```
diskutil list
diskutil unmountDisk /dev/disk2
```


3. dd刻录


```
dd if=./ubuntu-***.iso of=/dev/disk2 bs=1m;sync
```

<img src="/img/ubuntu-14.04.6-desktop-amd64.iso.png">
