---
title: 排序算法-选择排序
date: 2018-07-25 23:00:34
categories:
- 算法
tags:
- php
top: true
---



# 选择排序

> 选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理是：第一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，然后再从剩余的未排序元素中寻找到最小（大）元素，然后放到已排序的序列的末尾。以此类推，直到全部待排序的数据元素的个数为零。选择排序是不稳定的排序方法。


# 动画演示

<img src="/img/selectS.gif">

# 代码
```
function SelectionSort($arr)
{
    $length = count($arr);
    for ($i = 0; $i < $length - 1; $i++) {// $i为已经排序序列的末尾下标
        $min = $i;// 暂存未排列序列的最小值下标
        for ($j = $i + 1; $j < $length; $j++) {//遍历未排列序列
            if ($arr[$j] < $arr[$min]) {
                $min = $j;//找出排列序列最小值，下标赋给$min
            }
        }
        if ($min != $i) {//如果找到最小值，放到已排列序列末尾
            $t = $arr[$min];
            $arr[$min] = $arr[$i];
            $arr[$i] = $t;
        }
    }
    return $arr;
}

```



