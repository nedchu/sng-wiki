---
title: STL
date: 2018-11-03 20:33:01
tags: [STL]
categories: [教程]
author:
---
STL 能够简化编程流程，比较实用，此次主要讲解一些基础的STL然后讨论一些比赛中的常见技巧。
<!-- more -->
## 常用函数

`swap(x, y)`交换值

`min(x, y) max(x, y)`获得最大值以和最小值

`min_element(a, a + n) max_element(a, a + n)`获得数组中最大值和最小值的位置

初始化可以使用`fill(a, a + n, x)`，使用`memset`的时候是按照字节赋值的，`fill`能够避免这个限制，若在`vector`中使用，需要保证空间已经被分配

`find(a, a + n, x)`能够遍历寻找元素并返回指针

`count(a, a + n, x)`能够遍历统计元素个数

`next_permutation`枚举全排列

## 排序

程序设计中经常需要定义优先顺序进行排序，一般的做法是定义小于的关系，进而定义所有的`<,>,==,!=,>=,<=`关系。

对于两个东西`a,b`，沿用C系列语言的逻辑符号，就有：

- `!(a<b)`等价于`a>=b`
- `b<a`等价于`a>b`
- `!(a<b) && !(b<a)`等价于`a>=b && b>=a`等价于`a==b`
- `!(a==b)`等价于`a!=b`

其他关系类似推导，只要定义了小于关系，就能够确定顺序关系。所以只要给出一个判断两个东西`a,b`是否满足`a<b`的函数，就能够进行排序。

C++中能够使用标准库直接进行排序，包含头文件`#include <algorithm>`以及`using namespace std`之后。调用`sort(a, a + n)`能升序排序`a[0..n-1]`，降序排序可以`sort(a, a + n, greater<int>())`

`sort`函数的第三个参数为排序算子，定义了小于的关系，默认算子为`less<int>()`返回了`a<b`的结果，而`greater<int>()`返回了`a>b`的结果，尖括号内指定了变量`a,b`的类型。如果自己写了函数直接传入函数名即可，自己来写可以这么写：
```cpp
#include <bits/stdc++.h>

using namespace std;

bool less_(int a, int b) { return a < b; }
bool greater_(int a, int b) {return a > b; }

int main() {
    sort(a, a + n, less_);
    sort(a, a + n, greater_);
    return 0;
}
```
如果使用结构体数组对象数组之类的也是类似的，由于结构体没有默认顺序，需要传入比较算子，写一个结构体之间的比较函数即可。以下代码按照结构体先排序`x`，相同时按`y`排序，这样的结构可以使用标准库中的`pair,tuple`来替代，此处只是用作例子：
```cpp
struct P {
  int x, y;
};
bool cmp(P a, P b) {
  if (a.x == b.x) {
    return a.y < b.y;
  } else {
    return a.x < b.x;
  }
}
```

指定排序的另一种实现的方式是重载小于号，这里不进行展开。

C++11中有匿名函数，在比较算子无需多次使用的时候可以使用，方便进行理解和维护，具体方式如下：
```cpp
sort(a, a + n, [](int a, int b) {
  return a < b;
});
```

## 排序相关的其他函数

如果只需要获得数组排序后某个位置的值：`nth_element(a, a + i, a + n)`

如果只需要部分数组是排序后数组中的值：`partial_sort(a, a + i, a + n)`

排序后，如果需要去重可以调用`unique(a, a + n)`，其返回值为去重后最后一个元素后一个位置（不重复元素个数）

排序后，针对`vector`去重：`v.erase(unique(v.begin(), v.end()), v.end())`

注意以上函数只保证部分位置是有序且满足条件的，剩余位置的情况是不可预期的。

有序情况下调用二分查找元素是否存在：`binary_search(a, a + n, x)`

有序情况下调用二分查找元素范围：`equal_range(a, a + n, x)`

有序情况下获得大于等于$x$以及大于$x$的第一个位置的指针：`lower_bound(a, a + n, x) upper_bound(a, a + n, x)`

## 队列

`#include<queue>`中有`queue, priority_queue`

`queue`就是普通的队列

优先队列就是大根堆，支持在对数时间内取出最大值，插入元素。

使用 `#include <queue>` 后就能够使用优先队列了：

```cpp
priority_queue<int> pq; // 声明一个保存int的优先队列
pq.push(1);
pq.push(2);
pq.push(2); // add 1, 2, 2
int t = pq.top(); // 获得最优先（大）元素 t = 2
pq.pop(); // 2 is poped
pq.pop(); // 2 is poped
int s = pq.size(); // 获得优先队列大小 s = 1
pq.pop(); // 1 is poped
```

优先队列可以用于内置类型和定义了顺序的结构体，优先队列的模板中实际包含三个参数，分别是元素类型，容器类型和比较算子。

通过更改比较算子能够改变对优先级的定义，优先队列操作都是一致的，小根堆声明方式如下：

```cpp
priority_queue<int, vector<int>, greater<int>> q; // 小根堆
```

给第三个参数传入类似排序中的比较算子就能定义自己的优先级了。

## set, map

C++中`set, map`是平衡二叉搜索树，能够实现对数时间能插入，删除，查询元素。

`set`中所有元素为升序，且元素唯一，操作如下：

```cpp
set<int> s; // 声明
s.insert(3);
s.insert(2);
s.insert(2);
s.insert(1); // 插入元素，由于去重，留有1 2 3
s.earse(1); // 删除元素
s.size(); // 2
if (s.find(x) == s.end()) // 判断是否存在x
for (set<int>::iterator it = s.begin(); it != s.end(); it++) // seq: 1 2
    printf("%d\n", *it);
for (auto x : s); // in C++11
```

注意不要在迭代过程中插入或者删除元素，这样可能导致迭代器失效。

`map`中储存的是键值对，更明确来说是一个`pair`，第一个元素为键值，第二元素为值。

`map`当中实际就是保存一个映射关系，可以近似看成哈希表。

`map`中所有的元素为升序，键值是唯一的，操作如下：

```cpp
map<int, int> ma; // 声明
ma[1] = 2;
ma[1] = 3;
ma[2] = 1; // 插入元素，由于去重，留有{ 1:3, 2:1 }
if (ma.find(x) == ma.end()) // 判断是否键值x
for (map<int, int>::iterator it = s.begin(); it != s.end(); it++)
    printf("%d %d\n", it->first, it->second); //输出键值和对应的值
// 输出 1 3    2 1
for (auto x : ma) x.first, x.second;
```
