---
title: 图解aclocal、autoconf、automake、autoheader、configure
date: 2018-04-27 23:00:34
categories:
- C
tags:
- shell
- c
#top: true
---


## 1.autoscan (autoconf)
autoscan (autoconf): 扫描源代码以搜寻普通的可移植性问题，比如检查编译器，库，头文件等，生成文件configure.scan,它是configure.ac的一个雏形。

## 2.aclocal (automake)
aclocal (automake):根据已经安装的宏，用户定义宏和acinclude.m4文件中的宏将configure.ac文件所需要的宏集中定义到文件 aclocal.m4中。aclocal是一个perl 脚本程序，它的定义是：“**aclocal - create aclocal.m4 by scanning configure.ac**”
 
![image](https://user-images.githubusercontent.com/9856659/53383820-ce06eb80-39b3-11e9-96e8-66d8a471d371.png)


## 3. autoheader(autoconf)
autoheader(autoconf): 根据configure.ac中的某些宏，比如cpp宏定义，运行m4，声称config.h.in
 
![image](https://user-images.githubusercontent.com/9856659/53383951-3d7cdb00-39b4-11e9-84d4-47ae1007e637.png)


## 4. automake

automake: automake将Makefile.am中定义的结构建立Makefile.in，然后configure脚本将生成的Makefile.in文件转换为Makefile。如果在configure.ac中定义了一些特殊的宏，比如AC_PROG_LIBTOOL，它会调用libtoolize，否则它会自己产生config.guess和config.sub
 
![image](https://user-images.githubusercontent.com/9856659/53383937-30f88280-39b4-11e9-93bb-40214df80646.png)

## 5. autoconf
autoconf:将configure.ac中的宏展开，生成configure脚本。这个过程可能要用到aclocal.m4中定义的宏。
 
![image](https://user-images.githubusercontent.com/9856659/53383972-55545f00-39b4-11e9-9776-3e9cbc784b49.png)

