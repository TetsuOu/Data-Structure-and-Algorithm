# 0、参考



# 1、概述

字符串HASH，将字符串转化为可以用变量表示的数据，主要用于解决字符串匹配问题。

选取两个合适的互质常数b和h（b<h），假设字符串![S=s_1s_2\cdots s_m](https://private.codecogs.com/gif.latex?S%3Ds_1s_2%5Ccdots%20s_m)，那么我们定义哈希函数：![H(S)=(s_1b^{m-1}+s_2b^{m-2}+\cdots +s_mb^0)mod\, h](https://private.codecogs.com/gif.latex?H%28S%29%3D%28s_1b%5E%7Bm-1%7D&plus;s_2b%5E%7Bm-2%7D&plus;%5Ccdots%20&plus;s_mb%5E0%29mod%5C%2C%20h)。相当于把字符串看作是b进制数。



![](http://latex.codecogs.com/gif.latex?\\frac{1}{1+sin(x)})