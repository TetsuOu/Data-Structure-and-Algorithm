# 0、参考

https://blog.csdn.net/zzkksunboy/article/details/72600679



# 1、概述

$O(n)$时间复杂度解决最长回文子串问题

1. 预处理：在原字符串首尾以及每两个字符间加上特殊字符，如`#`，构造的新字符串奇数位为添加的符号，偶数位是原来的字符（若以1开始计数）
2. 令p[i]表示i能向两边推（包括i）的最大距离，即回文半径，以i为中心的最长回文长度即2*p[i]-1。但这是预处理后的字符，原来的字符串的最长回文就是p[i]-1
3. 若p[1~i-1]已求出，现在来求p[i]。令当前能达到的最右边界为R，对应的中点为pos，j为i的对称点（j=2*pos-i）
4. 若i<R，p[i]=min(p[2*pos-i],R-i)；当i>=R，暴力

**i<R：**

![这里写图片描述](https://img-blog.csdn.net/20170521181144124?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvenpra3N1bmJveQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

由于L~R是回文，所以p[i]=p[j]（i的最长回文和j的最长回文相同）。

![这里写图片描述](https://img-blog.csdn.net/20170521181152562?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvenpra3N1bmJveQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这种情况是另一种：j的最长回文跳出L了。那么i的最长回文就不一定是j的最长回文了，但蓝色的肯定还是满足的。

综上所述，p[i]=min(p[2*pos-i],R-i)。

**i>=R：**

由于后面是未知的，于是只能暴力处理了。



```c++
int Manacher()
{
    for(int i=1;i<=len;++i) nstr[2*i-1]='%',nstr[2*i]=str[i];
    nstr[len=2*len+1]='%';
    int pos=0,R=0;
    for(int i=1;i<=len;++i)
    {
        if(i<R) r[i]=min(r[2*pos-i],R-i);
        else r[i]=1;
        while(i-r[i]>=1&&i+r[i]<=len&&nstr[i-r[i]]==nstr[i+r[i]]) r[i]++;
        if(i+r[i]>R) pos=i,R=i+r[i];
    }
    int res=0;
    for(int i=1;i<=len;++i) res=max(res,r[i]-1);//原字符串最长回文子串
    return res;
}
```



# 2、例题

1、[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

题解：