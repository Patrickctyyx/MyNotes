# Problem C: 剁手女生节
[原题链接](http://gdutcode.sinaapp.com/problem.php?cid=1051&pid=2)
## Description
由于女生节准备到了，ming打算给班上女生送一份大礼。没错，就是数学练习册！

ming先前就已经收藏了 n 本练习册了，一直不舍得做，这次突然决定把它们都拿出来当作礼物送出去
！
但是，ming班上一共有 4 个女生，为了不要显得自己偏爱哪一个，他觉得每个女生都应该分到同等数量的练习册。

这样的话，原来的 n 本就可能不太够了。于是他去逛亚马当商城。

他发现，最近ACM（Association of Counting Method）又出版了好多新版数学练习册：高数、线代、离散、概率论…

而且商店有三种促销优惠套餐：

第一种：任选 1 本练习册，送欧几里德主题套尺。只需 a 个比特币；

第二种：任选 2 本练习册，送莱布尼兹同款2B铅笔。只需 b 个比特币；

第三种：任选 3 本练习册，送爱因思坦专用橡皮擦。只需 c 个比特币。

那么问题来了：吃土ming如何用最少的比特币购买若干本练习册，使得全部（包括原来的n本）可以平分给四个女生？

## Input
每组输入是一行四个整数：n，a，b，c（1 <= n，a，b，c <= 1e9）意思如题目描述。

## Output
对每组输入，输出一行一个整数，表示ming要花的最少的比特币数。

## Sample Input
3

1 1 3 4

6 2 1 1

4 4 4 4

## Sample Output
3

1

0


## 个人感想
即使原始数据还没超过int最后还是可能变成long long才能AC，比如这个。

还有就是之前不知道为什么莫名其妙的一堆秘制错误，删掉那些输出语句反而就好了...

## 我的思路
没什么多说的，情况考虑清楚就可以了，比如三本书反而比一本书便宜之类的

## 代码

```C
#include <stdio.h>

long long Min(long long x, long long y)
{
    if (x > y)
        return y;
    else
        return x;
}

int main()
{
    int t;
    scanf("%d", &t);
    while (t--)
    {
        int n;
        long long a, b, c;
        scanf("%d%lld%lld%lld", &n, &a, &b, &c);
        long long min = 0;
        int need = 4 - n % 4;
        switch (need)
        {
            case 1:
                min = Min(a, b + c);
                min = Min(min, 3 * c);
                break;
            case 2:
                min = Min(Min(b, 2 * a), 2 * c);
                break;
            case 3:
                min = Min(Min(3 * a, a + b), c);
                break;
            case 4:
                min = 0;
                break;
        }
        printf("%lld\n", min);
    }
    return 0;
}

```

## 最后
各种表达式一定不要手贱打错！

3a这个是错的！要么就是3*a！

最后在强调一次long long！