# Problem B：Zhazhahe究竟有多二

[原题链接](http://gdutcode.sinaapp.com/problem.php?cid=1051&pid=1)

## Description
Zhazhahe竟然能二到把耳机扔到洗衣机里去洗，真的是二到了一种程度，现在我们需要判断一下zhazhahe二的程度（就是计算zhazhahe的脑残值有几个2的因子），下面给你一个n，n!表示zhazhahe的脑残值。

## Input
输入一个正整数t(0<t<3000)表示样例组数，每组样例输入一个正整数n(0<n<1e18)，n!表示zhazhahe的脑残值

## Output
输出一个正整数表示zhazhahe二的程度

## Sample Input
3

2

4

15

## Sample Output
1

3

11


## 个人感想
真的十分想吐槽这道题...前后经历了TLE->RE->WA最后终于AC了，不过里面还是有一些坑需要我以后继续注意

## 我的思路
我已开始是直接求n!然后再求因子，然而这样太暴力了O(n^2)真的不是好玩的...一个TLE救过来了，后来想着用log2(n)来减少除的个数，用n-=2代替减1，然而O(n^2)还是O(n^2)。

之后看到了这样的一个公式：
> ==n!素因子分解中素数p的幂为 [n/p]+[n/(p^2)]+[n/(p^3)]+……==

于是用这个结论就避免了TLE

然而要注意的是他给的n的范围很大，用int是解决不了的，因为越界了所以会出现RE

而且要把所有的都改成ll，n，two，rst，有一个忘了改就会变成WA

最后把所有的都改过来了就可以AC了
## 代码

```C
#include <stdio.h>

// n!素因子分解中素数p的幂为 [n/p]+[n/(p^2)]+[n/(p^3)]+……
// RE...
int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        long long n;
        scanf("%lld", &n);
        long long rst = 0;
        long long two = 2;
        while (two <= n)
        {
            rst += n / two;
            two *= 2;
        }
        printf("%lld\n", rst);
    }
    return 0;
}

```

## 最后
这个故事告诉我们
- 要努力优化自己的算法，一些基本的结论要知道
- 要仔细的看题目，越界很容易出现RE，另外int是4个字节每个字节8位二进制数，范围是-2^31~2^31-1
- 改代码的时候把全部都改过来...

[附上一个各种类型的范围](http://blog.csdn.net/xuexiacm/article/details/8122267)

