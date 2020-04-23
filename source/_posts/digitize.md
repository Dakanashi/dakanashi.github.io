---
title: numpy.digitize()笔记
date: 2020-03-19 23:27:23
tags:
- Numpy
- Python
categories:
- 机器学习
---
***对digitilize函数做一下记录***
<!--more-->
---
## numpy.digitize(a, bins, right = False)
- a：numpy数组
- bins：一维单调数组，必须是升序或者降序。
- right：间隔是否包含最右。
- 返回值：a在bins中的位置。
```python
>>> import numpy as np
>>> bins = np.array(range(1, 102, 3))
>>> bins
array([  1,   4,   7,  10,  13,  16,  19,  22,  25,  28,  31,  34,  37,
        40,  43,  46,  49,  52,  55,  58,  61,  64,  67,  70,  73,  76,
        79,  82,  85,  88,  91,  94,  97, 100])
>>> np.digitize(5,bins)
2
>>> np.digitize(96,bins)
32
```
- 参考自<https://blog.csdn.net/zby1001/article/details/86616056>