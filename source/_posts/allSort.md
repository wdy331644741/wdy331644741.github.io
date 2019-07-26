---
title: 排序算法-总结
date: 2018-07-25 23:00:34
categories:
- 算法
tags:
- php
top: true
---



    | 算法        | 平均时间复杂度    |  最好情况       |  最坏情况      |  空间复杂度    |  排序方式  |  稳定性  |
    | --------    | -----             | ----            | ----           |   ----         |     ----   |  ----    | 
    | 冒泡排序*   | O($n^2$)          |   O(n)          | O($n^2$)       | O(1)           | in-place   | 稳定     |
    | 选择排序*   | O($n^2$)          |   O($n^2$)      | O($n^2$)       | O(1)           | in-place   | 不稳定   |
    | 插入排序*   | O($n^2$)          |   O(n)          | O($n^2$)       | O(1)           | in-place   | 稳定     |
    | 希尔排序    | O(n log n)        |   O(n log^2 n)  | O(n log^2 n)   | O(1)           | in-place   | 不稳定   |
    | 并轨排序    | O(n log n)        |   O(n log n)    | O(n log n)     | O(n)           | out-place  | 稳定     |
    | 快速排序*   | O($n^2$)          |   O(n)          | O($n^2$)       | O(1)           | in-place   | 稳定     |
    | 堆排序      | O(n log n)        |   O(n log n)    | O(n log n)     | O(1)           | in-place   | 不稳定   |
    | 计数排序    | O(n+k)            |   O(n+k)        | O(n+k)         | O(k)           | out-place  | 稳定     |
    | 桶排序      | O(n+k)            |   O(n+k)        | O($n^2$)       | O(n+k)         | out-place  | 稳定     |
    | 基数排序    | O(n*k)            |   O(n*k)        | O(n*k)         | O(n+k)         | out-place  | 稳定     |


# 说明

稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面；
不稳定：如果a原本在b的前面，而a=b，排序之后a可能会出现在b的后面；
内排序：所有排序操作都在内存中完成；in-place
外排序：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行；
时间复杂度： 一个算法执行所耗费的时间。
空间复杂度：运行完一个程序所需内存的大小。


n: 数据规模
k: “桶”的个数





# 链接：

- {% post_link quickSort 排序算法-快速排序 %}

- {% post_link selectSort 排序算法-选择排序 %}

- {% post_link insertSort 排序算法-插入排序 %}

- {% post_link bubbleSort 排序算法-冒泡排序 %}
