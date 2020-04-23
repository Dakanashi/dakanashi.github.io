---
title: numpy中数组切片时使用None
date: 2020-03-21 16:51:48
tags:
- Numpy
- Slice
- Python
categories:
- 机器学习
---
***学习Numpy的途中偶然看到数组切片时使用None的技巧，故记录一下。***
<!--more-->
---
- np.newaxis：None的别称，用于增加一个维度，<u>以下所有代码中None都有可以替换为np.newaxis</u>。
- 行的位置设置为None是给整体增加一个维度。
- 列的位置设置为None是给<u>每个非整体最大单位元素</u>增加一个维度。
```python
>>> a = np.array([1, 2, 3])
>>> a
array([1, 2, 3])
>>> a[None, :]
array([[1, 2, 3]])
>>> a[:, None]
array([[1],
       [2],
       [3]])


>> a = np.array([[1, 1],[2, 2],[3, 3]])
>>> a
array([[1, 1],
       [2, 2],
       [3, 3]])
>>> a[:, None]
array([[[1, 1]],

       [[2, 2]],

       [[3, 3]]])
>>> a[None, :]
array([[[1, 1],
        [2, 2],
        [3, 3]]])
```