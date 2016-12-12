# Problem D: 勤奋的涟漪2
[原题链接](http://note.youdao.com/)
## Description
> 涟漪进入集训队后，他会去实验室训练或者去操场锻炼。 接下来n天，每天的情况是一下4种中的一种： 1.当天体育馆关门了和没有训练赛 2.当天体育馆关门了和有训练赛 3.当天体育馆开放和没有训练赛 4.当天体育馆开放和有训练赛 涟漪知道之后n天的情况。 涟漪每一天可以休息，或者打训练赛（当天有训练赛）或者运动（当天体育馆开放）。 涟漪要制定一个训练计划，决定每天干什么，但是涟漪不会连续两天都运动或者连续两天都打训练赛， 请帮涟漪找出她最少休息的天数（她不打训练赛和运动）。 休息的时候，她会做下面的数学题
> 
> ![image](http://gdutcode-gdutcode.stor.sinaapp.com/image/1209.png)

## Input
> 第一行一个整数t（t<=100）,代表测试数据， 第二行一个整数 n（1<=n<=100） 第三行有n个数a1,a2,a3,....an(0<=ai<=3)) ai=0 ,代表第一种情况 ai=1,代表第二种情况 ai=2 ,代表第三种情况 ai=3 ,代表第四种情况

## Output
> 输出 一个数 表示（涟漪休息的天数） 乘以（数学题的答案的积）。

## Sample Input
> 4
>
> 7
>
> 1 3 3 2 1 2 3
>
> 1
>
> 1
>
> 1
>
> 2
>
> 1
>
> 3

## Sample Output
> 0
> 
> 0
> 
> 0
> 
> 0

## 个人感想
当他们讲题的时候说这是动态规划我是一脸懵逼的。另外还是要自己思考，直接接受别人的思想不仅收获很小而且还很容易茫然。

## 我的思路
这题的限制条件就是连续两天不能是相同的状态，休息除外，因此因该有一个ystday来储存上一天的行为，workday来储存不是休息的天数

ystday值：
- 0：休息
- 1：训练
- 2：锻炼
- 3：锻炼或者训练

然后根据当天的情况来进行相应的判断

当天情况：
- 0：只能休息，workday不变
- 1：训练或者休息，只要ystday不是1，workday就加一，并且把ystday变为1；否则workday不变，ystday变为0
- 2：锻炼或者休息，和上面的类似
- 3：三者都可以，workday加一，如果ystday是1或者2，ystday就变成2或者1，否则ystday变为3
- 【其实ystday的0和3可以合并】

## 代码

```C
#include<stdio.h>

int main()
{
//    freopen("test.in", "r", stdin);
    int t;
    scanf("%d", &t);
    while (t--)
    {
        int n;
        scanf("%d", &n);
        int ystday, workday;
        ystday = workday = 0;
        for (int i = 0; i < n; ++i)
        {
            int today;
            scanf("%d", &today);
            switch (today)
            {
                case 0:
                    ystday = 0;
                    break;
                case 1:
                    if (ystday != 1)
                    {
                        ystday = 1;
                        workday += 1;
                    } else
                        ystday = 0;
                    break;
                case 2:
                    if (ystday != 2)
                    {
                        ystday = 2;
                        workday += 1;
                    } else
                        ystday = 0;
                    break;
                case 3:
                    workday += 1;
                    if (ystday == 1)
                        ystday = 2;
                    else if (ystday == 2)
                        ystday = 1;
                    else
                        ystday = 3;
                    break;
            }
        }
        printf("%d\n", (n - workday) * -24);
    }
    return 0;
}

```

## 最后
其实我这个做法依然是动态规划，本质上是一样的，只不过我用ystday来储存前一天的状态而讲解的是直接用数组储存所有情况

[一个参考范例](http://blog.csdn.net/jnxxhzz/article/details/53455350#)