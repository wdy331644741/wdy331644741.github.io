---
title: laravel pip管道验证HTTP 极思惶恐
date: 2018-05-17 14:20:11
categories:
- php
tags:
- laravel
#top: true
---

## 前言
> 最近研究laravel底层 看到这个pip管道。看后一脸茫然。茶不思饭不想，半夜还在回忆它这个代码。
皇天不负苦心人，finally......

_展示demo之前。先描述一个日常生活中常见的栗子——借钱。
Ex：cto 向leader借钱，而leader说没有，我问问下边的人
A说 我也没有，我问问B吧。
B说 没有，我问C要点...
最后C从蚂蚁花呗套现100元，给了B
B拿这个100元给A
A给 leader
leader给cto_

## 栗子

```
<?php
interface Middleware
{
    public static function handle($request='1块',Closure $next);
}

//请求开始 逐一穿过
class ranhaiqing implements Middleware
{
    public static function handle($request,Closure $next)
    {
        echo __class__.":好烦啊，又借{$request}\n";
	    $next($request);
        echo __class__.":就这么多钱了。\n";
    }
}

class zhaodongran implements Middleware
{
    public static function handle($request,Closure $next)
    {
        echo __class__.":哎呀我也{$request}钱，别着急我帮你想办法\n";
        $next($request);
        echo __class__.":这个钱来的不容易啊\n";
    }
}

class pangxiaofei implements Middleware
{
    public static function handle($request,Closure $next)
    {
        echo __class__.":我没有，我向旁边人先借{$request}\n";
        $next($request);
    }
}

class wangdongyang implements Middleware
{
    public static function handle($request,Closure $next)
    {
        echo __class__.":那行吧，我从小额贷撸{$request}\n";
        $next($request);
	    echo __class__.":%@！……*-#！\n";
    }
}


////////////////等同于_getSlice方法/////////////////
function getSlice(){
    return function ($stack, $pipe) {
        return function ($request) use ($stack, $pipe) {
            $slice = parentGetSlice();
            return call_user_func($slice($stack, $pipe), $request);
        };
    };
}
function parentGetSlice()
{   
    return function ($stack, $pipe){
        return function ($request) use ($stack, $pipe) {
            return $pipe::handle($request,$stack);
        };
    };
}
///////////////////////////////////////////////////
function _getSlice($stack, $pipe)
{
    return function ($request) use ($stack, $pipe) {
        return $pipe::handle($request,$stack);
        //return call_user_func_array([$pipe, 'handle'], [$request,$stack]);
    };
}
function then($request)
{
    $pipes = [
        "ranhaiqing",
        "zhaodongran",
        "pangxiaofei",
        "wangdongyang",
    ];

    $firstSlice = function($request) {
        echo "终于有{$request}了\n";
    };
    //数组数据反转
    $pipes = array_reverse($pipes);
    
    $go = array_reduce($pipes, getSlice(),$firstSlice);//var_dump($go);exit;
    return $go($request);
    // return call_user_func(
    //     array_reduce($pipes, getSlice(), $firstSlice), $request
    // );
}
//借钱开始：
then('1000块');
```

> 运行结果

```
ranhaiqing:好烦啊，又借1000块
zhaodongran:哎呀我也1000块钱，别着急我帮你想办法
pangxiaofei:我没有，我向旁边人先借1000块
wangdongyang:那行吧，我从小额贷撸1000块
终于有1000块了
wangdongyang:%@！……*-#！
zhaodongran:这个钱来的不容易啊
ranhaiqing:就这么多钱了。
```

![image](https://user-images.githubusercontent.com/9856659/60749317-9b58a980-9fca-11e9-89ff-82c3c0a3df84.png)


## 另附laravel管道处理请求

![image](https://user-images.githubusercontent.com/9856659/60749263-f0e08680-9fc9-11e9-8f19-43a95a2ed260.png)

