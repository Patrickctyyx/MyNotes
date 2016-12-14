# Problem A: pigofzhou的巧克力棒
[原题链接](http://gdutcode.sinaapp.com/problem.php?cid=1051&pid=0)
## Description
> 众所周知，pigofzhou有许多妹子。有一天，pigofzhou得到了一根巧克力棒，他想把这根巧克力棒分给他的妹子们。具体地，这根巧克力棒长为 n，他想将这根巧克力棒折成 n 段长为 1 的巧克力棒，然后分给妹子们。
>
> 但是他妹子之一中的 15zhazhahe 有强迫症。若它每次将一根长为 k 的巧克力棒折成两段长为 a 和 b 的巧克力棒，此时若 a=b，则15zhazhahe会得到一点高兴值。
>
> pigofzhou想知道15zhazhahe最多能获得多少高兴值。

## Input
> 输入数据为T组(T <= 10000)，每组数据读入一个n（n<=1000000000）

## Output
> 一行一个整数代表能获得的最大高兴值

## Sample Input
> 1
> 
> 5

## Sample Output
> 3

## 个人感想
我一直以为这是水题，结果在知道揭发的情况下还是做了一下午才AC...我真的好菜呀orz，要继续加油啦！

## 我的思路
一开始我以为直接一次次的对半分就可以了，然而这样得到的并不是最大值。最大的情况是这样一种分法：分成若干个数，这每个数都是2^n，这样继续每次分就有最大值了。

话说这个方法还是上次听他们讲解的时候知道的...至于怎么样分，答案是二进制。比如15=8+4+2+1，二15变成二进制数为1111=1000+100+10+1zhenghao正好和上面的算式对应。

然后8对应的最大高兴值是4+2+1=8-1，4同理为2+1=4-1。

这样思路就很清楚了，先把输入的n变成二进制数，然后再根据每一位的数来求出最后的结果。

## 代码

```C
#include <stdio.h>
#include <string.h>
#include <math.h>

typedef int array[30];  // 直接在函数参数里面用数组名有莫名的报错...
int ten2two(int n, array &a)  // 转化为二进制数并将结果保存在数组中
{
    int i = 0;
    while (n != 0)  // 除二取余法
    {
        a[i] = n % 2;
        n = n / 2;
        i++;
    }
    return i - 1;
}
int main()
{

    int T;
    scanf("%d", &T);
    while (T--)
    {
        int n;
        array a;
        memset(a, 0, sizeof(a));  // 一键赋初值
        scanf("%d", &n);
        int happiness = 0;
        int num = ten2two(n, a);  // 二进制数的最高位
        for (; num >= 0; --num)
        {
            if (a[num] == 1)
                // pow是求次方，这里是2^num，结果应该是double型的，我这里直接赋给int
                happiness = happiness + pow(2, num) - 1;  
        }
        printf("%d\n", happiness);
    }

    return 0;
}

```

## 最后
二进制数神奇的用法真多，不愧是底层的机器语言，跪拜。

