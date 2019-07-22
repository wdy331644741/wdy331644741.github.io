---
title: C configure配置
date: 2018-04-27 23:00:34
categories:
- C
tags:
- c
---

## 1 说明

> 在linux 中，经常需要用到交叉编译。简单地说，就是在一个平台上生成另一个平台上的可执行代码。同一个体系结构可以运行不同的操作系统；同样，同一个操作系统也可以在不同的体系结构上运行。
> 对于大部分代码，都有configure文件，让开发者进行配置，配置完毕之后自动生成makefile，然后进行编译。本文旨在说明configure中
常用的一些参数。


<span id = "configure参数说明"></span>
## 2 configure参数说明
<span id = "查看configure 配置选项"></span>
### 2.1 查看configure 配置选项

> 在configure目录下，运行 --help命令，可以查看到configure的配置参数一共有哪些。


```
[02:34:58][root@wdy:~/lnmp1.5/src/php-7.3.6]# ./configure --help
`configure' configures this package to adapt to many kinds of systems.

Usage: ./configure [OPTION]... [VAR=VALUE]...

To assign environment variables (e.g., CC, CFLAGS...), specify them as
VAR=VALUE.  See below for descriptions of some of the useful variables.

Defaults for the options are specified in brackets.

Configuration:
  -h, --help              display this help and exit
      --help=short        display options specific to this package
      --help=recursive    display the short help of all the included packages
  -V, --version           display version information and exit
  -q, --quiet, --silent   do not print `checking ...' messages
      --cache-file=FILE   cache test results in FILE [disabled]
  -C, --config-cache      alias for `--cache-file=config.cache'
  -n, --no-create         do not create output files
      --srcdir=DIR        find the sources in DIR [configure dir or `..']

Installation directories:
  --prefix=PREFIX         install architecture-independent files in PREFIX
                          [/usr/local]
  --exec-prefix=EPREFIX   install architecture-dependent files in EPREFIX
                          [PREFIX]

By default, `make install' will install all the files in
`/usr/local/bin', `/usr/local/lib' etc.  You can specify
an installation prefix other than `/usr/local' using `--prefix',
for instance `--prefix=$HOME'.

For better control, use the options below.

Fine tuning of the installation directories:
  --bindir=DIR            user executables [EPREFIX/bin]
  --sbindir=DIR           system admin executables [EPREFIX/sbin]
```


<span id = "2.2参数说明"></span>
### 2.2 参数说明


<span id = "2.2.1"></span>
#### 2.2.1 build 参数

>  --build=BUILD     configure for building on BUILD [guessed]

build: 执行代码编译的主机，正常的话就是你的主机系统。这个参数一般由config.guess来猜就可以。当然自己指定也可以。可以默认不写，默认为当前正在使用的ubuntu主机，如 i386-linux

```
 --build=i386-linux
 或者xilinx-arm的编译器主机
 --build=i686-pc-linux-gnu
 或者不写

```

<span id = "2.2.2"></span>
##### 2.2.2 host 参数

>  --host=HOST       cross-compile to build programs to run on HOST [BUILD]

指定软件运行的系统平台.如果没有指定,将会运行`config.guess’来检测.–host 指定的是交叉编译工具链的前缀。如本位中采用的是arm-xilinx-linux-gnueabi 工具链，则参数配置为:

```
--host=arm-xilinx-linux-gnueabi
```

<span id = "2.2.3"></span>
##### 2.2.3 target 参数

>   --target=TARGET   configure for building compilers for TARGET [HOST]

target: 这个选项只有在建立交叉编译环境的时候用到，正常编译和交叉编译都不会用到。他用build主机上的编译器，编译一个新的编译器（binutils, gcc,gdb等），这个新的编译器将来编译出来的其他程序将运行在target指定的系统上。
如果不编译新的编译器，这个参数可以不填，或者与 host的参数一致

```
--target=arm-xilinx-linux-gnueabi
或者不写 --target的参数

```

<span id = "2.2.4"></span>
##### 2.2.4 CC 编译器参数

> CC          C compiler command

指定GCC 交叉编译器命令，如果配置了，则使用CC配置的编译器，如果不配置则默认为host对应的GCC工具
如配置了 --host=arm-xilinx-linux-gnueabi，则默认CC的编译器为 arm-xilinx-linux-gnueabi-gcc
这个参数如无特殊指定，可以忽略不写。


<span id = "2.2.5"></span>
##### 2.2.5 prefix 安装参数

>  --program-prefix=PREFIX            prepend PREFIX to installed program names

该参数指定编译后，文件安装的目录。
不指定prefix，则可执行文件默认放在/usr/local/bin，库文件默认放在/usr/local/lib，配置文件默认放在/usr/local/etc。其他的资源文件放在/usr/local/share。你要卸载这个程序，要么在原来make目录下用make uninstall(前提是make文件指定过make uninstall)，要么去上述文件中一个一个手动删除。如需指定make install 的目录，如 /home/tmp/test

```
--prefix=/home/tmp/test

```


【转载】https://blog.csdn.net/xhoufei2010/article/details/82768995 
