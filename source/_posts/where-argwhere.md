---
title: where和argwhere详解
date: 2020-03-21 17:36:31
tags:
- Numpy
- Where
- Python
categories:
- 机器学习
---
***学习Numpy发现np.where()使用有一些难点，顺便一块将np.argsort()进行整理***
<!--more-->
---
## np.where(condition, x, y)
- 功能：当condition为真时执行x，否则执行y。
```python
>>> import numpy as np
>>> a = np.array([1.1, 2.2, 3.3, 4.4, 5.5])
>>> b = np.array([6.1, 7.2, 8.3, 9.4, 10.5])
>>> a
array([1.1, 2.2, 3.3, 4.4, 5.5])
>>> b
array([ 6.1,  7.2,  8.3,  9.4, 10.5])
>>> cond = np.array([True, False, True, False, True])
>>> np.where(cond, a, b)
array([1.1, 7.2, 3.3, 9.4, 5.5])
```
---
## np.where(cond)和a[expression]的辨析
- 功能：
- a[expression]返回符合expression的元素。
```python
>>> a = np.arange(5)
>>> a
array([0, 1, 2, 3, 4])
>>> a[a>2] #直接返回元素
array([3, 4])
```
- np.where(cond)返回符合cond条件的元素的下标，<u>如果cond中数组为一维，返回值由一个array和一个空值组成，要想得到真真结果需单独获取下标[0]的元素</u>。
```python
>>> a = np.array([1, 5, 6, 7, 8])
>>> a
array([1, 5, 6, 7, 8])

>>> np.where(a>2) #返回的是一个元组
(array([1, 2, 3, 4]),)
>>> np.where(a>2)[0] #想要取得里面的array需要单独获取下标[0]的元素
array([1, 2, 3, 4])
```
---
## np.argwhere(a)
- 功能：返回非0的数组元组的索引，其中a是要索引数组的条件。
```python
>>> a = np.arange(6).reshape(2, 3)
>>> a
array([[0, 1, 2],
       [3, 4, 5]])
>>> np.argwhere(a>1)
array([[0, 2],
       [1, 0],
       [1, 1],
       [1, 2]])


>>> np.where(a>1)#当a中涉及的是一个二位数组时，返回的元组会将下标分成两个维度的数组作为返回值返回，无空值。
(array([0, 1, 1, 1]), array([2, 0, 1, 2]))
```
