---
title: 排序算法-快速排序
date: 2018-07-25 23:00:34
categories:
- 算法
tags:
- php
#top: true
---



# 快速排序

百度快速排序算法的时候 网上有这种php写法

[baidu-快速排序](https://baike.baidu.com/item/%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/369842?fromtitle=%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F&fromid=2084344&fr=aladdin#3_13)

# 介绍

> 快速排序由C. A. R. Hoare在1960年提出。它的基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

# 动画演示

<img src="/img/quickS.gif">

```

function quick_sort($arr)
{
    // 判断是否需要继续
    if (count($arr) <= 1) {
        return $arr;
    }
 
    $middle = $arr[0]; // 中间值
 
    $left = array(); // 小于中间值
    $right = array();// 大于中间值
 
    // 循环比较
    for ($i=1; $i < count($arr); $i++) { 
        if ($middle < $arr[$i]) {
            // 大于中间值
            $right[] = $arr[$i];
        } else {
 
            // 小于中间值
            $left[] = $arr[$i];
        }
    }
 
    // 递归排序两边
    $left = quick_sort($left);
    $right = quick_sort($right);
 
    // 合并排序后的数据，别忘了合并中间值
    return array_merge($left, array($middle), $right);
}

```


有网友说了这种虽然符合快速排序的思想。但是开辟了新的变量空间

> $left = array(); // 小于中间值
> $right = array();// 大于中间值

# 按照动画写了一段code

然后结合其他大神的算法 自己写了一个。


```

function quick(&$arr,$start,$end){

    $x = (int)$start;//xy 单进程递归时用于区分 上一层快排拆分出来的位置
    $y = (int)$end;//用于最后两行的递归调用
    if($x >= $y){
        return;//递归到的最小单位（或者说是两相邻的元素）
    }
    $pivot = $arr[$end]; //以最后一个元素作为基准，划分数组
    //从开始找到第一个大于基准的元素  和  从尾部找到的小于基准的元素   并交换位置Swap
    while ($start < $end){
        //从前寻找 找到第一个大于基准的元素
        while ($start < $end){
            if($arr[$start] < $pivot){
                $start++;
            }else{
                break;
            }
        }
        //从后寻找 第一个小于基准的元素
        while ($start < $end ){
            if($arr[--$end] > $pivot){
                continue;
            }else{
                break;
            }
        }
        
        if($start == $end){
            //完成划分 后把基准和放到小于基准的那一部分的结尾
            Swap($arr, $start, $y);
        }else{
            Swap($arr, $start, $end);//交换 这两个元素的位置
        }
    }
    quick($arr,$x ,$start -1);
    quick($arr,$start+1,$y);//两部分 再递归处理
    return $arr;
}

```


# reference

另外附大神代码：

[https://blog.csdn.net/qq_36045946/article/details/80751831#6.%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F(QuickSort)](https://blog.csdn.net/qq_36045946/article/details/80751831#6.%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F(QuickSort))



