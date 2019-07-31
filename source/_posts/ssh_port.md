---
title: linux开通ssh通道
date: 2019-07-27 23:00:34
categories:
- linux
tags:
- shell
#top: true
---

> 临时 紧急接到一个运营需求，要求统计一个活动的参与情况。按理说该需求本来是应该由公司BI部门负责的。但是BI的数据库和线上的数据库数据同步有时差，说最快2个小时、最迟1个自然日。而且2个小时还不能完全保证数据的完整性。运营人员接受不了，说要实时的。因为该活动是秒杀活动，称自己需要根据实时情况调整活动资源、奖品的配置。


最后讨论无果、说只能麻烦让我在活动期间内每天中午12点整  手动整理线上数据，然后抄送邮件。


¥%……&* 我肯定不能每天大中午的给他整理这个去啊么，再说活动持续一个多月呢。



最后想个办法。既然我navcat软件能通过只读账号+跳板机链接到mysql。我程序也肯定能啊

google。。。。

大概搜到两种方法
1. Php有ssh相关的函数
2. 在shell中增加一条ssh通道


我的想法是在测试服务器上添加一个定时计划。然后写一个php脚本。每天12点执行这个计划
抱着试试的态度  选了第二种方法


```
#!/bin/bash

#杀死
kill -9 $(ps -aux|grep -E 3308|grep -v grep|awk '{print $2}')

## 本地打开3308端口通过ssh管道连接到阿里rds:3306数据库(前提是私钥公钥 配置好)
ssh -fN -L3308:rdsu43ck*****:3306 -p22 develop@182.92.*.*

PID=$(ps -aux|grep -E 3308|grep -v grep|awk '{print $2}')

cd ~/wanglibao/api
#发送邮件+统计数据脚本
php artisan SendMail

#关闭通道
kill -9 $PID
if [ $? -eq 0 ];then
    echo "kill PID: $PID success"
else
    echo "kill PID:$PID fail"
fi

```


定时计划

```
*/5 * * * * sh /root/sendmail.sh >>/tmp/sendmail.log 2>&1  
```

