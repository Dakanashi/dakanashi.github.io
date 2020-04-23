---
title: Numpy文件的存取
date: 2020-03-22 00:23:05
tags:
- Numpy
- Python
categories:
- 机器学习
---
***对Numpy中文件相关的函数进行总结***
<!--more-->
---
## 1.数组以二进制格式保存。
- np.save和np.load是读写磁盘数组数据的两个主要函数。默认情况下，数组以未压缩的原始二进制格式保存在扩展名为npy的文件中，<u>文件的后缀名字一定会被置为.npy</u>。
```python
np.save("filename.npy",a) #将数组a中的数据存入文件
b = np.load("filename.npy") #将文件中的数据读取出来并存入b
```
---
## 2.存取文本文件
- 使用 np.savetxt 和 np.loadtxt 只能读写 1 维和 2 维的数组
np.savetxt：将数组写入以某种分隔符隔开的文本文件中
np.loadtxt：指定某种分隔符，将文本文件读入到数组中
```python
np.savetxt("filename.txt",a)
b =  numpy.loadtxt("filename.txt", delimiter=',')
```
---
## 3.保存为二进制文件
- 使用数组的 tofile 函数可以方便地将数组中数据以二进制的格式写进文件
```python
a.tofile("filename.bin") #将数组a的数据存入二进制文件
b = np.fromfile("filename.bin",dtype = **) #从二进制文件中以dtype的形式提取数据并存储于b
```
- 该方法与np.save有几点区别：
1. tofile函数只能将数组保存为二进制文件，文件后缀名没有固定要求。这种保存方法对数据读取有要求，np.fromfile 需要手动指定读出来的数据的的dtype，<u>如果指定的格式与保存时的不一致，则读出来的就是错误的数据</u>。
2. tofile函数不能保存当前数据的行列信息，不管数组的排列顺序是C语言格式的还是Fortran语言格式，统一使用C语言格式输出。<u>因此使用 np.fromfile 读出来的数据是一维数组，需要利用reshape指定行列信息</u>。
```python
>>> a = np.arange(0,12)
>>> a.shape = 3,4
>>> a
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>> a.tofile("a.bin")
>>> b = np.fromfile("a.bin", dtype=np.float) # 按照float类型读入数据
>>> b # 读入的数据是错误的
array([  2.12199579e-314,   6.36598737e-314,   1.06099790e-313,
         1.48539705e-313,   1.90979621e-313,   2.33419537e-313])
>>> a.dtype # 查看a的dtype
dtype('int32')
>>> b = np.fromfile("a.bin", dtype=np.int32) # 按照int32类型读入数据
>>> b # 数据是一维的
array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11])
>>> b.shape = 3, 4 # 按照a的shape修改b的shape
>>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
```
---
## 4.从url获取文件
- np.genfromtxt(url, delimiter, dtype=)：从url获取文件，以delimiter为分隔符，设定数据类型为dtype。
```python
url = 'https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data'
species = np.genfromtxt(url, delimiter=',', dtype='str', usecols=4)
```
---
- 参考自<https://blog.csdn.net/u010089444/article/details/52738479>