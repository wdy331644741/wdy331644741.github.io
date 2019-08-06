---
title: PHPcode执行过程
date: 2018-04-28 23:00:34
categories:
- php
tags:
#top: true
---


1. Scanning(Lexing) ,将PHP代码转换为语言片段(Tokens)
2. Parsing, 将Tokens转换成简单而有意义的表达式
3. Compilation, 将表达式编译成Opocdes
4. Execution, 顺次执行Opcodes，每次一条，从而实现PHP脚本的功能。


> token_get_all(),这个函数就可以将一段PHP代码 Scanning成Tokens；

### 例子

```
➜  wdy cat for.php
<?php

$tokens = token_get_all('<?php echo 123;?>');
print_r($tokens);

?>
➜  wdy php for.php
Array
(
    [0] => Array
        (
            [0] => 374
            [1] => <?php
            [2] => 1
        )

    [1] => Array
        (
            [0] => 317
            [1] => echo
            [2] => 1
        )

    [2] => Array
```


### vld介绍


> vld是PECL（PHP 扩展和应用仓库）的一个PHP扩展，现在最新版本是 0.14.0（2016-12-18），它的作用是：显示转储PHP脚本（opcode）的内部表示（来自PECL的vld简介）。简单来说，可以查看PHP程序的opcode。

### 安装vld扩展
1. 下载官方插件压缩包
[http://pecl.php.net/package/vld](http://pecl.php.net/package/vld)
2. 安装

``` 
# wget http://pecl.php.net/get/vld-0.14.0.tgz

# tar zxvf vld-0.14.0.tgz
root@wdy vld-0.14.0 # phpize 
```

> Configuring for:
PHP Api Version:         20131106
Zend Module Api No:      20131226
Zend Extension Api No:   220131226

``` 
# ./configure --with-php-config=/usr/local/php/bin/php-config
# make && make install
```

> Build complete.
Don't forget to run 'make test'.

> Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/

```
# vi /usr/local/php/etc/php.ini 
```
> extension = "vld.so"

[php-config](http://www.php.net/manual/zh/install.pecl.php-config.php)

至此,VLD就安装完了。写个简单的测试文件

--- 

```
root@wdy ~ # cat yield.php
<?php

function xrange($start,$end,$step = 1){
	for($i = $start; $i<=$end;$i += $step){
		yield $i;
	}
}

//foreach(xrange(1,1000000) as $num){
//	echo $num,"\n";
//}


$range = xrange(1,1000000);
var_dump($range);
var_dump($range instanceof Iterator);
?>
```


```
root@wdy ~ # php -dvld.active=1 yield.php
Finding entry points
Branch analysis from position: 0
Jump found. (Code = 62) Position 1 = -2
filename:       /root/yield.php
function name:  (null) /*opcode所属函数。全局，此处为null。每个函数会有对应的完整的opcode信息。*/
number of ops:  12 /*此段opcode有多少个运算操作*／
compiled vars:  !0 = $range
／*op list 核心部分 需要对照zend文档查看*／
／*line 操作所在的行号*／
／*#* 操作*／
／*op 操作符 对应c的操作*／
line     #* E I O op                           fetch          ext  return  operands
-------------------------------------------------------------------------------------
   3     0  E >   NOP
  14     1        SEND_VAL                                                 1
         2        SEND_VAL                                                 1000000
         3        DO_FCALL                                      2  $0      'xrange'
         4        ASSIGN                                                   !0, $0
  15     5        SEND_VAR                                                 !0
         6        DO_FCALL                                      1          'var_dump'
  16     7        FETCH_CLASS                                 128  :3      'Iterator'
         8        INSTANCEOF                                       ~4      !0, $3
         9        SEND_VAL                                                 ~4
        10        DO_FCALL                                      1          'var_dump'
  18    11      > RETURN                                                   1

branch: #  0; line:     3-   18; sop:     0; eop:    11; out1:  -2
path #1: 0,
Function xrange:
Finding entry points
Branch analysis from position: 0
Jump found. (Code = 45) Position 1 = 10, Position 2 = 8
Branch analysis from position: 10
Jump found. (Code = 161) Position 1 = -2
Branch analysis from position: 8
Jump found. (Code = 42) Position 1 = 6
Branch analysis from position: 6
Jump found. (Code = 42) Position 1 = 4
Branch analysis from position: 4
filename:       /root/yield.php
function name:  xrange
number of ops:  11
compiled vars:  !0 = $start, !1 = $end, !2 = $step, !3 = $i
line     #* E I O op                           fetch          ext  return  operands
-------------------------------------------------------------------------------------
   3     0  E >   RECV                                             !0
         1        RECV                                             !1
         2        RECV_INIT                                        !2      1
   4     3        ASSIGN                                                   !3, !0
         4    >   IS_SMALLER_OR_EQUAL                              ~1      !3, !1
         5      > JMPZNZ                                        8          ~1, ->10
         6    >   ASSIGN_ADD                                    0          !3, !2
         7      > JMP                                                      ->4
   5     8    >   YIELD                                                    !3
   6     9      > JMP                                                      ->6
   7    10    > > GENERATOR_RETURN

branch: #  0; line:     3-    4; sop:     0; eop:     3; out1:   4
branch: #  4; line:     4-    4; sop:     4; eop:     5; out1:  10; out2:   8
branch: #  6; line:     4-    4; sop:     6; eop:     7; out1:   4
branch: #  8; line:     5-    6; sop:     8; eop:     9; out1:   6
branch: # 10; line:     7-    7; sop:    10; eop:    10; out1:  -2
path #1: 0, 4, 10,
path #2: 0, 4, 8, 6, 4, 10,
End of function xrange

object(Generator)#1 (0) {
}
bool(true)
```




##### 参考资料
- [VLD扩展使用指南](http://www.phppan.com/2011/05/vld-extension/)

- [Zend Engine 2 操作码列表](http://php.net/manual/zh/internals2.opcodes.list.php/)
