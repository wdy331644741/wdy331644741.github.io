---
title: phpredis 和 predis
date: 2018-04-27 23:00:34
categories:
- php
tags:
- redis
#top: true
---


# phpredis 和 predis

1. phpredis 是使用c写的php扩展，predis 是使用纯php写的。
在性能上的区别当然是扩展更好一些，但其实这两个实现还有更大的区别，就是连接的保持。
phpredis在扩展中使用c可以保持php-fpm到redis的长连接，所以一个php-fpm进程上的多个请求是复用同一个连接的。phpredis的pconnect就是长连接方式。

2. predis是使用php的socket来连接redis，所以需要每次请求连接redis。
可以看出laravel的官方是推荐使用predis的，因为纯php实现的原因，只需要composer即可安装，非常符合laravel便捷的思想。

**phpredis 和 predis 的性能差距没有跨数量级，当然要考虑具体业务，如果业务非常依赖redis，并且单机qps需要支持的比较大，建议使用phpredis。如果你只是使用laravel使用redis实现规模小的业务，建议不用改变predis。**
