---
title: numpy库中的repeat和tile
date: 2020-03-19 21:21:53
tags:
- Numpy
- DL
- Python
categories:
- 机器学习
---
***numpy.repeat()与numpy.tile()的区别***
两个方法都是赋值原数组进行扩展，但细节略微不同，repeat还有一种初始化数组的方法。
<!--more-->
---
## np.repeat(a, repeat, axis=None)
## a.repeat(repeat, axis=None)
- 将a数组进行repeat倍扩充，axis=0时进行纵向扩充，axis=1时进行横向扩充，为设置时进项扁平化处理,不改变自身。
```python
>>> import numpy as np

>>>np.repeat(2, 9)
array([2, 2, 2, 2, 2, 2, 2, 2, 2])

>>> a = np.arange(8).reshape(2, 4)
>>> a
array([[0, 1, 2, 3],
       [4, 5, 6, 7]])

>>> a.repeat(2)
array([0, 0, 1, 1, 2, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7, 7])

>>> a.repeat(2, 0)
array([[0, 1, 2, 3],
       [0, 1, 2, 3],
       [4, 5, 6, 7],
       [4, 5, 6, 7]])

>>> a.repeat(2, 1)
array([[0, 0, 1, 1, 2, 2, 3, 3],
       [4, 4, 5, 5, 6, 6, 7, 7]])
```

## np.tile(a, tile)
- 对数字进行tile倍<u>轮流扩充</u>，不改变自身。
```python
>>> import numpy as np

>>> a = np.arange(8).reshape(2, 4)
>>> a
array([[0, 1, 2, 3],
       [4, 5, 6, 7]])

>>> np.tile(a, 2)
array([[0, 1, 2, 3, 0, 1, 2, 3],
       [4, 5, 6, 7, 4, 5, 6, 7]])
```