---
title: numpy 基本使用
date: 2020-03-14 18:04:06
tags:
- Numpy
- Python
categories:
- 机器学习
---
***对Numpy库中的函数进行整理***
学习DL途中对于numpy的一个粗略整理，部分函数只给出常用参数，之后可能会跳出其中几个复杂的做详细记录。
<!--more-->
---
## 查看数组属性
- **函数及其功能**
a.ndim：维度。
a.shape：改变自身各维度的尺度。 
a.size：元素的个数。
a.dtype：元素的类型。
a.itemsize：每个元素的大小。
***

## 数组的初始化
- **函数及其功能**
np.array(a)/np.ndarray(shape, dtype)：初始化数组a。
np.arange(start, end, distance, dtype=None)：由[start, end)创建一个步进为distance的行数组。
np.ones((shape), dtype=np.float64)：创建shape大小，内涵dtype类型的1数组。
np.zeros((shape), dtype=np.float64)：创建shape大小，内涵dtype类型的0数组。
np.full((shape), val, dtype=None)：创建shape大小，内涵dtype类型val的填充数组。
np.empty((shape), dtype=np.float64)：生成一个shape大小，dtype类型含无意义数的数组。
np.eye(n, dtype=np.float64)：创建一个n*n，dtype类型的单位数组。
np.ones_like(a, dtype=None)：创建一个和a相同大小，填充dtype类型的1数组。
np.zeros_like(a, dtype=None)：创建一个和a相同大小，填充dtype类型的0数组。
np.full_like(a, fill_value, dtype=None)：创建一个和a相同大小，填充dtype类型fill_value的数组。
np.empty_like(a, fill_value, dtype=None)：创建一个和a相同大小，填充dtype类型无意义值的数组。
np.linspace(start, end, distance)：由[start, end)区间等间距地生成数组.
np.set_printoptions(precision=None, threshold=None)：设置所有数组的精度和打印位数。
***

## 数组的合并
- **函数及其功能**
np.concatenate((a, b), axis=0):将b沿着垂直方向（axis=0）或者水平方向（axis=1）与a进行合并。
np.vstack((a, b))：沿着竖直方向将数组堆叠起来。
np.hstack((a, b))：沿着水平方向将数组堆叠起来。
np.c_(a, b)：将列向量进行连接。
np.r_(a, b)：将行向量进行链接。
***

## 数组的维度操作
- **函数及其功能**
a.reshape(shape) : 不改变当前数组，依shape生成。
a.resize(shape) : 改变当前数组，依shape生成。
a.swapaxes(ax1, ax2) : 将两个维度调换。
a.flatten() : 对数组进行降维，返回折叠后的一维数组。
a.ravel()：对数组进行降维，不会产生源数据的副本。
a.squeeze()：对数组进行降维，只能对维数为1的维度降维。
np.newaxis：增加一个维度。
a.repeat(repeat, axis=None)：将a数组进行repeat倍扩充，axis=0时进行纵向扩充，axis=1时进行横向扩充，为设置时进项扁平化处理。
np.repeat(x, y)：创建一个数组，内含y个x。
np.tile(a, tile)：对数字进行轮流扩充，将原数据最大单元看做一个单元，将形状变为tile的形状。
np.asscalar(a)：将只有一个元素的数组a，转化成标量值。
***

## 数组的类型变换
- **函数及其功能**
a.asarray(b)：将一个非numpy数组的元素转变为numpy数组。
a.astype(new_type) : 数据类型的转换。
a.tolist()：数组向列表的转换。
***

## 数组的运算操作
- **函数及其功能**
a.transpose()/a.T()：求a矩阵的转置矩阵。
np.abs(a)/np.fabs(a)/np.absolute(a): 取各元素的绝对值。
np.sqrt(a): 计算各元素的平方根。
np.square(a): 计算各元素的平方。
np.log(a): 计算以e为底，a的对数。
np.log10(a)：计算以10为底，a的对数。
np.log2(a)：计算以2为底，a的对数。
np.ceil(a)：返回输入值的上限，向上取整。
np.floor(a): 返回不大于输入参数的最大整数，向下取整。
np.around(a, decimals=0): 返回各元素四舍五入的值，decimals指定保留至小数点后xx位，如果为负，整数将四舍五入到小数点左侧的位置，与round()函数类似。
np.exp(a): 计算各元素的指数值。
np.sign(a): 计算各元素的符号值 1（+），0，-1（-）。
a.copy()：将a进行深复制。
a.view()：将a进行浅复制。
np.copysign(a, b): 将b中各元素的符号赋值给数组a的对应元素。
np.mod(a, b): 元素级的模运算。
np.dot(a, b)：对矩阵进行点乘运算。
np.sin(a)：计算a的正弦。
np.cos(a)：计算a的余弦。
np.tan(a)：计算a的正切。
np.cov(x)：求协方差。
np.corrcoef(x, y)：求x，y相关系数。
np.logical_and(x, y)：返回x和y逻辑与之后的结果。
np.logical_or(x, y)：返回x和y逻辑或之后的结果。
np.logical_not(x, y)：返回x和y逻辑非之后的结果。
np.logical_xor(x, y)：返回x和y逻辑异或之后的结果。
np.vectorize(myfunc, otype)：定义了一个矢量函数myfunc，输入是嵌套化的对象序列或者是numpy数组，输出是单个或元组的numpy数组，otype指定输出值的类型。
np.gradient(a)：计算数组a中元素的梯度。
numpy.linalg.solve(a, b)：返回矩阵方程ax=b的解。
numpy.linalg.norm(a, ord=2)：求矩阵a的范数，ord=1求一范数，ord=2求二范数，ord=np.inf求无穷范数，当a为两个数组相减的表达式时是计算欧氏距离。
***

## 随机数函数
- **函数及其功能**
np.random.seed(s)：随机数种子，使用下面的随机方法前最好设置一个随机数种子。
np.random.rand(d0, d1, …,dn)：各元素是[0, 1）的浮点数，服从均匀分布。
np.random.randn(d0, d1, …,dn)：标准正态分布。
np.random.randint(low，high=None, size)：创建size个随机整数或整数数组，范围是[low, high），如果没有写参数high的值，则返回[0,low)的值。
np.random.shuffle(a)：根据数组a的第一轴进行随机排列，改变数组a。
np.random.permutation(a)：根据数组a的第一轴进行随机排列，但是不改变原数组，将生成新数组。
np.random.choice(a, size, replace=False, p)：从一维数组a中以概率p抽取元素， 形成size形状新数组，replace表示是否可以重用元素，默认为False。
np.random.uniform(low, high, size)：产生均匀分布的数组，起始值为low，high为结束值，size为形状。
np.random.normal(loc, scale, size)：产生正态分布的数组，loc为均值，scale为标准差，size为形状。
np.random.poisson(lam, size)：产生泊松分布的数组，lam随机事件发生概率，size为形状。

***

## 统计函数
- **函数及其功能**
np.diff(a, n=1, axis=1)：沿着指定轴计算第N维的离散差值（执行的是后一个元素减去前一个元素）。
np.sum(a, axis = None)：依给定轴axis计算数组a相关元素之和，axis为整数或者元组。
numpy.cumsum(a, axis=None, dtype=None)：按照所给定的轴参数返回元素的梯形累计和(后一个加上前面元素的累加和)，axis=0，按照行累加。axis=1，按照列累加。axis不给定具体值，就把numpy数组当成一个一维数组。
numpy.ufunc.accumulate(a, axis=0, dtype=None)：照所给定的轴参数返回根据ufunc进行计算的元素的梯形累计，ufunc可以是add，multiply等。
np.mean(a)：返回数组元素中的均值。
np.average(a, axis =None, weights=None)：依给定轴axis计算数组a相关元素的加权平均值。
np.median(a, axis=None)：计算数组的中位数。
np.std(a, axis=None)：计算数组的标准差，axis=0时计算每一列的标砖茶，axis=1计算每一行的标准差。
a.var()：计算数组的方差。
np.max(a, axis=0)：寻找数组中的最大值。
np.min(a, axis=0)：寻找数组中的最小值。
np.maximum(a, b)/np.fmax()：a,b逐位比较（或者计算）元素级的最大值。
np.minimum(a, b)/np.fmin()：a,b逐位比较（或者计算）元素级的最小值。
np.argmax(a, axis=None)：沿axis返回最大值的索引。
np.argmin(a, axis=None)：沿axis返回最小值的索引。
np.argsort(a, axis=None)：返回的是数组值从小到大的索引值。
np.argpartition(a, kth)：是将第k大的数字，放在第k个位置。类似于快排找第k大的数，堆其它数不做排序。
np.amin(a，axis=None)：返回一维数组a中的最小值，二维数组需通过axis指定行或列，获取行（1）或列（0）的最小值，如不指定，则是所有元素的最小值 。
np.amax(a, axis=None)：返回一维数组a中的最大值，二维数组需通过axis指定行（1）或列（0），获取行或列的最大值，如不指定，则是所有元素的最大值。
numpy.apply_along_axis(func, axis, arr)：func指定转换方法，axis指定按行（1）运算还是按列（0）运算，arr指定数组。
np.nonzero(a)：返回数组a中非零元素的索引值数组。
np.percentile(a, percent)：返回数组中第percent%位置的浮点数。
(expression).all()：两个数组进行比较时如果所有元素根据expression都为True则返回True。
(expression).any()：两个数组进行比较时如果任意元素根据expression都为True则返回True。
np.bincount(a)：统计a中[min, max]之间各个数字的出现次数。
np.where(condition, x, y)：当condition为True时输出x，否则输出y。
np.where(condition)：当筛选满足condition条件的元素。
np.argwhere(expression)：返回非0的数组位置的索引，其中expression是要索引数组的条件，非0数组指的时expression中左值对应的数组。
a.ptp()：计算数组元素最大值与最小值的差。
np.pi：数学中的π。
numpy.euler_gamma：数学中的伽马。
np.e：欧拉的常数，自然对数的基础，纳皮尔的常数
np.newaxis：None的别称，用于增加一个维度。
np.inf：IEEE 754 浮点表示（正）无穷大。
np.infinity：IEEE 754 浮点表示（正）无穷大。
np.infty：IEEE 754浮点表示（正）无穷大。
np.pinf：IEEE 754 浮点表示（正）无穷大。
np.ninf：IEEE 754 浮点表示负无穷大。
np.nan：IEEE 754 浮点表示非数字（NaN）。
np.pzero：IEEE 754 浮点表示正零。
np.nzero：IEEE 754 浮点表示负零。
np.isnan(item)：查看item是否为nan值。
np.isinf(item) : 显示item元素是否为正或负无穷大。
np.isposinf(item) : 显示item元素是否为正无穷大。
np.isneginf(item) : 显示item元素是否为负无穷大。
np.isnan(item) : 显示item元素是否不是数字。
np.isfinite(item) : 显示item元素是否为有限的（不是非数字，正无穷大和负无穷大中的一个）
np.nan_to_num(x)：使用0代替数组x中的nan元素，使用有限的数字代替inf元素。
np.unique(a, return_index=False, return_inverse=False, return_counts=False, axis=None)：对于只有a参数的一位数组，去除数组中的重复元素，进行排序后输出；return_index=True返回新列表元素在旧列表中的位置，并以列表形式储存；return_inverse=True返回旧列表元素在新列表中的位置，并以列表形式储存；return_count=True对独特值进行计数。
np.digitize(x, bins, right = False)：获得属于数组的每个值的bin的索引数组。
np.searchsorted(a, x)：输出x在a中所处数字间的间隔代号。
np.sort(a, axis=1)：对a进行排序，默认为按行从小到大排序，当axis=0时按列从小到大排序，会影响列表本身。
np.sorted(a, axis=1)：对a进行排序，默认为按行从小到大排序，当axis=0时按列从小到大排序， 不会影响列表本身。
np.clip(a, a_min, a_max)：将a的范围限制到[a_min, a_max]之间。
np.intersect1d(a1, a2, assume_unique=False)：返回a1中和a2共有的值，当assume_unique为True时，嘉定输入的a1和a2是唯一的，可以加快计算速度。
np.setdiff1d(a1, a2, assume_unique=False)：返回a1中和a2差异的值，当assume_unique为True时，嘉定输入的a1和a2是唯一的，可以加快计算速度。
np.convolve(a, b, mode=‘full’)：对数组a，b进行卷积运算。
np.interp(x, xp, fp)：给定1维数据x中插入点(xp, fp)。
np.allclose(a, b)：比较两个array是不是每一元素都相等，默认在1e-05的误差范围内。
numpy.trunc(x, /, out=None, *, where=True, casting='same_kind', order='K', dtype=None, subok=True[, signature, extobj])：以元素方式返回输入的截断值。标量x的截断值是最接近的整数i，它比x更接近零。简而言之，丢弃带符号数x的小数部分。
***

