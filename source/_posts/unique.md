---
title: numpy.unique()详解
date: 2020-03-19 22:50:39
tags:
- Numpy
- Python
- Unique
categories:
- 独特值相关计算
- 机器学习
---
***numpy库里面的unique参数比较多，有些复杂，故整理一下方便以后查阅。***
<!--more-->
---
## np.unique(a, return_index=False, return_inverse=False, return_counts=False, axis=None)
- 对于只有a参数的一位数组，去除数组中的重复元素，进行排序后输出。
```python
>>> import numpy as np

>>> a = np.array([1, 2, 2, 5, 3, 4, 3])
>>> np.unique(a)
array([1, 2, 3, 4, 5])
```
- return_index=True<u>返回新列表元素在旧列表中的位置</u>，并以列表形式储存。
```python
>>> b,s = np.unique(a, return_index=True)
>>> print(b)
[1 2 3 4 5]
>>> print(s)
[0 1 4 5 3]
```
- return_inverse=True<u>返回旧列表元素在新列表中的位置</u>，并以列表形式储存。
```python
>>> c,s,p = np.unique(a, return_index=True, return_inverse=True)
>>> print(c)
[1 2 3 4 5]
>>> print(s)
[0 1 4 5 3]
>>> print(p)
[0 1 1 4 2 3 2]
```
- return_count=True对独特值进行计数。
```python
>>> d,q = np.unique(a, return_counts=True)
>>> print(d)
[1 2 3 4 5]
>>> print(q)
[1 2 2 1 1]
```
---
- 参考自<https://blog.csdn.net/yangyuwen_yang/article/details/79193770>