---
title: mac 关闭sip 保护系统
date: 2018-02-11 21:30:54
categories:
- linux
tags:
- mac
#top: true
---


# 最近在mac上操作文件发现提示

> chmod: Unable to change file mode on /usr/bin/cc: Operation not permitted

原因是El Capitan（10.11） 加入了Rootless机制，很多系统目录不再能够随心所欲的读写了，即使设置 root 权限也不行。

以下路径无法写和执行
> /System
/bin
/sbin
/usr (except /usr/local)

加入这个机制主要是为了防止恶意程序的入侵，更多我们可以查看官网

[链接](https://developer.apple.com/videos/play/wwdc2015/706/)

# 如何关闭

重启按住 Command+R，进入恢复模式，打开Terminal

```
csrutil disable
```

# 如何开启

重启按住 Command+R，进入恢复模式，打开Terminal。

```
csrutil enable
```

