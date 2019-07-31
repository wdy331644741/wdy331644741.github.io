---
title: shell多进程控制
date: 2019-07-27 23:00:34
categories:
- linux
tags:
- shell
#top: true
---

> 试想平时我们需要使用shell多进程搞一些事情。如果是小事情还好，如果是大事情的话，我们for循环启动多个进程在后台运行。假设for循环很大 启动了几千个进程。我们的机器能支撑的住吗？

## FIFO管道

它是一种文件类型，在文件系统中可以看到。FIFO中可以很好地解决在无关进程间数据交换的要求，并且由于它们是存在于文件系统中的，这也提供了一种比匿名管道更持久稳定的通信办法。

> $mkfifo [option] name...


以下例来讲。我们把父shell进程PID绑定一个fifo管道，然后设定最大并发进程数3 。大概原理就是写入到fifo文件三个空行。然后后面我们不关心for循环中要启动多少个子进程，子进程运行完后会减去fifo文件中一条空行。达到一个**窗口限流**的效果。

```
#!/bin/bash

trap 'exec 6>&-; exec 6<&-; exit 0;' INT

#for ((i=1;i<=10;i++));
#do
#
#echo '\n'
#done

##网站域名
site='www.echototo666.com'

#首页
url_by_page=( $(curl -s ${site} |grep post-title-link |awk  '{ print substr($2,6)}'|  sed  's/"//g') )


#如果有其他页
##接受页面变量
if [ $# -gt 0 ];then
have_page=$1

for((i=2;i<=$have_page;i++));
do
url=$site"/page/"$i"/"
get_page=($(curl -s ${url} |grep post-title-link |awk  '{ print substr($2,6)}'|  sed  's/"//g') )
url_by_page+=${get_page[@]}
done


fi



#至此获取到所有到页面url
echo ${url_by_page[@]}


function flash(){
    ##传递的url参数
    _url=$1
    ref="Referer:http://$site$_url"
    i=0
    tmp_arr=(${uri//\// })
    bar=${tmp_arr[3]}':'
    while [ $i -le 100 ]
    do

        printf "[%-100s][%d%%]\r" "$bar" "$i"

    let i++
    ## 带上cookie时网站uv不变，只刷pv
    curl -s \
    -H $ref \
    -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36" \
    --cookie "busuanziId=C2B688C12EE64A93BE945853A895C3FA" \
    'http://busuanzi.ibruce.info/busuanzi?jsonpCallback=BusuanziCallback_813017575528' > /dev/null

        bar+='#'
    done

}

##其中$$为该进程的pid
tmp_fifofile="/tmp/$$.fifo"
##创建命名管道
mkfifo $tmp_fifofile
##把文件描述符6和FIFO进行绑定
exec 6<>$tmp_fifofile
##绑定后，该文件就可以删除了
rm -f $tmp_fifofile

##并发量为3，用这个数字来控制并发数
thread=13
for ((i=0;i<$thread;i++));
do
    ##写一个空行到管道里，因为管道文件的读取以行为单位
    echo >&6
done


for uri in ${url_by_page[@]}
do
##读取管道中的一行,每次读取后，管道都会少一行
read -u6
{
#tmp_arr=(${uri//\// })

#flash $uri > ~/${tmp_arr[3]}.log
flash $uri || {echo "task is failed"}
##每次执行完flash函数后，再增加一个空行，这样下面的进程才可以继续执行
echo >&6
} &
done
#tail -f file1 file2 | awk '/^==> / {a=substr($0, 5, length-8); next}  {print a":"$0}'

wait

##关闭文件描述符6的写
exec 6>&-
echo "\n alldone"

```




### ps:最后遗留有一个问题，就是上面那个脚本是去刷页面访问量的flsah方法中又一个进度条。然后在命令行中运行父进程的时候，不同子进程中的进度条会有一个输出覆盖的bug




