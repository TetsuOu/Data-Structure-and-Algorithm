# 0、参考

《挑战程序设计竞赛（第二版）》4.7.2 字符串匹配

# 1、概述

字符串HASH，将字符串转化为可以用变量表示的数据，主要用于解决字符串匹配问题。

选取两个合适的互质常数b和h（b<h），假设字符串$S=s_1s_2\dots s_n$，那么我们定义哈希函数：$H(S)=(s_1b^{n-1}+s_2b^{n-2}+\dots+s_nb^0)mod\ h$ 相当于把字符串看作是b进制数。

## 1.1、滚动哈希

字符串$S=s_1s_2\dots s_n$从位置k+1开始长度为m的字符串子串$S[k+1\dots k+m]$的哈希值，可以利用从位置k开始的字符串子串$S[k\dots k+m-1]$的哈希值，直接进行如下计算。
$$
H(S[k+1\dots k+m])=(H(S[k\dots k+m-1])\times b-s_k\times b^m+s_{k+m})
$$
于是，只要不断这样计算开始位置右移一位后的字符串子串的哈希值，就可以在$O(n)$时间内得到所有位置对应的哈希值，从而可以在$O(m+n)$时间内完成字符串匹配（m、n分别为待匹配的两个字符串的长度）。

在实现时，**可以用64位无符号整数计算哈希值，并取h等于$2^{64}$，通过自然溢出省去求模运算。**

（这种利用滚动哈希的字符串匹配算法叫做Rabin-Karp算法。不过，原本的Rabin-Karp算法在哈希值相等时，还要用传统的字符串比较算法来判断字符串是否相等。而在程序设计竞赛中，往往会特意准备一些出现大量相等情况的测试数据，如果一一检查的话，复杂度会退化成$O(mn)$。而不同字符串的哈希值发生冲突的概率非常低，通常可以忽视。因此，在程序设计竞赛中，通常只比较哈希值，而不做朴素的检查。）

```c++
#define ull unsigned long long
class Solution {
public:
    const static ull B = 1e9+7;
    //haystack为文本串，needle为模式串，返回模式串在文本串中首次出现的位置
    int strStr(string haystack, string needle) {
       int lenH=haystack.size(),lenN=needle.size();
       if(lenN==0) return 0;
       ull t=1,hashH=0,hashN=0;
       for(int i=1;i<=lenN;++i) t*=B;//B的lenN次方
       //计算haystack长度为lenN的前缀的哈希值 和 needle的哈希值 
       for(int i=0;i<lenN;++i) hashH=hashH*B+haystack[i],hashN=hashN*B+needle[i];
       //对haystack不断右移一位，更新哈希值并判断 
       for(int i=0;i+lenN<=lenH;++i){
           if(hashH==hashN) return i;
           if(i+lenN<lenH) hashH=hashH*B+haystack[i+lenN]-haystack[i]*t;
       } 
       return -1;
    }
};
```

当然，不光是右移一位，对于左移一位、左端或右端加长一位或是缩短一位的情况，也能够进行类似处理。

