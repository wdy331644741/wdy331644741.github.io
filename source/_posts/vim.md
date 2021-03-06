---
title: vim常用操作
date: 2018-04-27 23:00:34
categories:
- linux
tags:
- linux
- shell
#top: true
---


## 基本操作

 - 退出：:q   或者ZZ
 - 插入： i，a，L，A，O，o

<span id = "复制"></span>
## 复制

```
yy --复制一行,3yy复制三行
yw --复制一个词 2yw复制2个词。p 粘贴出来
```

<span id = "删除"></span>
## 删除
```
dd --删除一行，即（d1）
D  --删除到行尾，即“d$”
```

<span id = "撤销、重做"></span>
## 撤销、重做
```
u（undo），CTRL-R（redo）
```

<span id = "移动"></span>
## 移动

> hl 左右移动
jk 下上移动

<span id = "按字（词）移动光标"></span>
 1. 按字（词）移动光标
> w和W命令：将光标右移至下一个字的字首，它们的区别是：w命令，把光标移到下一个字（狭义）的字首，W命令，将把光标移到下一个字（广义）的字首。

> b和B命令：如果光标处于所在字内（即非字首），则该命令将把光标移至本字字首；如果光标处于所在字字首，则该命令将把光标移到上一个字的字首。
<span id = "按句移动光标"></span>
 2. 按句移动光标
> ( 命令：将光标移至上一个句子的开头；
> ) 命令：将光标移至下一个句子的开头。
<span id = "按段移动光标"></span>
 3. 按段移动光标
> { 命令：将光标向前移至上一个段的开头；
> } 命令：将光标向后移至下一个段的开头。
<span id = "移动到行首或行尾"></span>
 4. 移动到行首或行尾
> $ 移动到行尾。1$表示：移动到当前行的行尾；2$表示：移动到下一行的行尾；
^ 移动到行首 数字0  也是移动到行首

> “G” 命令把光标移动到文末；
“gg”命令把光标移动到文首；
<span id = "括号匹配"></span>
 5. 括号匹配
> % 它能匹配一对括号(即“( )”，“[ ]”，“{ }”)。
如果光标在“(”上，它移动到对应的“)”上，反之，如果它在“)”上，它移动到“(”上。
当光标不在一个有用的字符上，“%”会先向前找到一个，然后会移动到它的匹配处。
<span id = "使用记号（mark）"></span>
 6. 使用记号（mark）
> 当用“G”命令跳到另一个地方，Vim会记住你从什么地方跳过去的，这个位置成为一个记号（mark）。可以成为记号的还有查找命令：“/”和“?”

> CTRL-O命令跳到一个“较老”的地方（提示：O表示older)；
CTRL-I命令跳到一个“较新”的地方（提示：I在键盘上仅靠着O）；

<span id = "多个文件分割窗口"></span>
## 多个文件分割窗口
```
:split a.php	水平分割(sp)
:vsplit xxx	垂直分割
```
> 窗口之间跳转
CTRL-w w 用于在窗口间跳动
CTRL-w h 跳转到左边的窗口
CTRL-w j 跳转到下面的窗口
CTRL-w k 跳转到上面的窗口
CTRL-w l 跳转到右边的窗口

<span id = "对比文件不同"></span>
### 对比文件不同
```
vimdiff 文件1 文件2
```
<span id = "搜索与替换"></span>
## 搜索与替换
>   : s // 和:g//，:!g//
:s是替换操作，:g是查找匹配模式的行，:!g是查找不匹配模式的行。

```
/word  向下搜索word
?word 向上搜索word
配合使用 n/N  
```

```
:s/old/new/	替换一行  第一个
:s/old/new/g	替换当前行所有

:n1,n2s/word1/word2/g	n1与n2 都为数字，表示行数,
可在第 n1 行与第 n2 行之间寻找 word1 字符串，并替换为 word2 
```

```
:1,$s/word1/word2/g	表示从第一行到最后一行，将 word1 字符串替换为 word2 
:1,$s/word1/word2/gc	表示从第一行到最后一行，也是将 word1 字符串替换为 word2，不同之处是在替换前显示提示字元，由用户确认是否最终替换 
```
> “%” ：表示整个文件，同“1,$”；
> 
> “. ,$” ：从当前行到文件尾；

