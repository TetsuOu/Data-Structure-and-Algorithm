# 零、参考



# 一、概述

字符串的匹配算法。关键是Next数组的求解。
Next[i]表示str[0..i-1]的**最长公共前缀后缀**（前缀不包括最后一个字符，后缀不包括第一个字符），在kmp算法中又表示**匹配失败时模式串的下一次匹配位置**。字符串str[0..len-1]的最长公共前缀后缀长度是Next[len]，公共前缀后缀长度还可以是Next[Next[len]]、Next[Next[Next[len]]]等。（不为0）

**若将str看做str[1..n]，则Next[i]表示str[1..i]的最长公共前缀后缀。**

```C++
int Next[MAXN];
int a[MAXN],b[MAXN];//文本串、模式串
int lenA,lenB;//文本串长度、模式串长度
void GetNext()
{
    int k=Next[0]=-1;
    int j=0;
    while(j<lenB)
    {
        if(k==-1||b[k]==b[j]) Next[++j]=++k;
        else k=Next[k];
	}
}
int kmp()
{
	int i=0,j=0，ans=-1;
	while(i<lenA)
	{
		if(j==-1||a[i]==b[j]) ++i,++j;
		else j=Next[j];
		if(j==M)//修改这里满足不同要求
		{
			ans=i-j;//文本串与模式串匹配的首位
			break;
		}
	}
	return ans;
}
```

