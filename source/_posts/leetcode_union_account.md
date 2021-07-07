---
title: (leetcode)整理合并email账户
date: 2021-07-07 23:00:34
categories:
- 算法
tags:
- leetcode
#top: true
---



### 题目：按人合并email账户

```
Input: 
accounts = [
    ["John", "johnsmith@mail.com", "john00@mail.com"], 
    ["John", "johnnybravo@mail.com"], 
    ["John", "johnsmith@mail.com", "john_newyork@mail.com"], 
    ["Mary", "mary@mail.com"]
]

Output: [
    ["John", 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com'],  
    ["John", "johnnybravo@mail.com"], 
    ["Mary", "mary@mail.com"]
]

Explanation: 
  第一个和第三个 John 是同一个人，因为他们有共同的电子邮件 "johnsmith@mail.com"。 
  第二个 John 和 Mary 是不同的人，因为他们的电子邮件地址没有被其他帐户使用。
  我们可以以任何顺序返回这些列表，例如答案[['Mary'，'mary@mail.com']，['John'，'johnnybravo@mail.com']，
  ['John'，'john00@mail.com'，'john_newyork@mail.com'，'johnsmith@mail.com']]仍然会被接受。

```

### code：

```
class Solution {

    /**
     * 合并账户问题
     * @param String[][] $accounts
     * @return String[][]
     */
    function accountsMerge($accounts) {
        $init = iterator_to_array($this->xrange(1111));
        $rank = iterator_to_array($this->xrange(1111, 0)); //维护一个秩 较小的秩往较大大秩上面合
        $mail2id = [];
        $mail2name = [];

        $id = 0;
        foreach ($accounts as $item) {

            for ($i = 1; $i < count($item); $i++) {
                $mail2name[$item[$i]] = $item[0];
                if (!array_key_exists($item[$i], $mail2id)) {
                    $mail2id[$item[$i]] = $id++;
                }

                $this->union($mail2id[$item[1]], $mail2id[$item[$i]], $init);

            }
        }


        $tmp = [];
        foreach ($mail2name as $k=>$v){
            // $belong = $init[$mail2id[$k]];
            $belong = $this->_find($mail2id[$k], $init);
            $tmp[array_search($belong, $mail2id)][] = $k;
        }


        $res = [];
        foreach($tmp as $k=>$v){
            sort($v);
            array_unshift($v, $mail2name[$k]);
            array_push($res, $v);
        }

        return $res;
    }

    function xrange($num, $init = null)
    {
        for ($i = 0; $i < $num; $i++) {
            yield $init ?? $i;
        }
    }


    function find($x, &$arr)
    {
        if ($x != $arr[$x]) {
            $arr[$x] = $this->find($arr[$x], $arr);
        }

        return $arr[$x];
    }


    function _find($x, &$arr)
    {
        $son = $x;
        while ($x != $arr[$x]) {
            $x = $arr[$x];
        }

        //最短路径
        while ($son != $x) {
            $tmp       = $arr[$son];
            $arr[$son] = $x;
            $son       = $tmp;
        }

        return $x;
    }

    //按秩合并
    function union_rank($a, $b, &$arr, &$rank)
    {
        $_a = $this->_find($a, $arr);
        $_b = $this->_find($b, $arr);
        if ($_a !== $_b) {
            $rank[$_a] <= $rank[$_b] && $arr[$_a] = $_b;
            $rank[$_a] > $rank[$_b] && $arr[$_b] = $_a;
            $rank[$_a] == $rank[$_b] && $rank[$b]++;
        }
        return $arr;
    }

    function union($a, $b, &$arr)
    {
        $_a = $this->_find($a, $arr);
        $_b = $this->_find($b, $arr);
        if ($_a !== $_b) {
            $arr[$_b] = $_a;//默认把第二个合到第一个子叶子下
        }
        return $arr;
    }
}

```

```
accounts = [
    ["John", "johnsmith@mail.com", "john00@mail.com"], 
    ["John", "johnnybravo@mail.com"], 
    ["John", "johnsmith@mail.com", "john_newyork@mail.com"], 
    ["Mary", "mary@mail.com"]
]
```

##### 利用并查集结构

> 每个email对应一个id

0. johnsmith@mail.com
1. john00@mail.com
2. johnnybravo@mail.com
3. john_newyork@mail.com
4. mary@mail.com

```
graph LR
0-->0
1-->1
2-->2
3-->3
4-->4
```

> union(0,1)

```
graph LR
0-->0
1-->0

```

> union(0,3)

```
graph LR
0-->0
1-->0
2-->2
3-->0
4-->4
```


最终图结构 如上图

解：查找图的连接快

