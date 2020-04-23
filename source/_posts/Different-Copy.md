---
title: 三种不同的Copy方式详解
date: 2020-03-21 14:36:15
tags:
- Numpy
- Python
categories:
- 机器学习
---
***Python中的Copy有三种方式——完全不复制、浅复制（试图）、深复制，接下来进行举例讲解。***
<!--more-->
---
## 完全不复制（两个对象共同指向一个数据）
```python
>>> import numpy as np
>>> a = np.arange(6)
>>> a
array([0, 1, 2, 3, 4, 5])

#b的id和a的id相同
>>> b = a
>>> id(b)
139643466946240
>>> id(a)
139643466946240

#a和b都会保管数据。
>>> b.flags.owndata
True
>>> a.flags.owndata
True

#a就是b
>>> b is a
>>> True
```
---
## 浅复制（又称试图，发生在np.view和切片操作中）
```python
>>> b = a[:]   #或者b = a.view()
>>> b
array([0, 1, 2, 3, 4, 5])

#a和b的id不同
>>> id(b)
139643466911216
>>> id(a)
139643466946240

#a负责保管数据，b不负责报关数据
>>> b.flags.owndata
False
>>> a.flags.owndata
True

##a不是b
>>> b is a
>>> False

#改变b的形状不会影响a
>>> b.shape=(2, 3)# no.reshape()也是一种浅复制
>>> b
array([[0, 1, 2],
       [3, 4, 5]])
>>> a
array([0, 1, 2, 3, 4, 5])

#改变b的元素会影响a，即使改变b形状后改变b元素的值也会影响a
>>> a[3] = 67
array([ 0,  1,  2, 67,  4,  5])
>>> b
array([[ 0,  1,  2],
       [67,  4,  5]])
```
---
## 深复制
```python
>>> b = a.copy()
>>> b
array([ 0,  1,  2,  3,  4,  5])

##a和b的id不同
>>> id(a)
139643466946240
>>> id(b)
139643675206080

#a不是b
>>> a is b
False

#a和b各自保管一份不同的数据
>>> a.flags.owndata
True
>>> b.flags.owndata
True

#改变a的元素不会影响b
>>> a[2] = 90
>>> a
array([ 0,  1, 90,  3,  4,  5])
>>> b
array([0, 1, 2, 3, 4, 5])
```
