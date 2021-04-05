---
title: Python_for_Combinations
tags: [Python]
---
[401.二进制手表](https://leetcode-cn.com/problems/binary-watch/): 一道典型的回溯题，一般的思路是将num分解成h和m，分别表示hour中LED灯亮的数量和minute中LED灯亮的数量，同一个数量对应着不同的取值 i.e. h = 1 对应着 小时可以取到1， 2， 4， 8这四种取值，且都是合法的，而h=2对应着小时可以取到3， 5， 9，6，10，12 这6种取值，其中12超过了11是不合法的。

h的数量固定时，m= num - h 自然也就固定， 而我们要做的就是在固定h的数量时, 将所有合法的小时的对应取值H和分钟的对应取值MM组合起来成为 "H:MM"

沿着这个思路, 问题归结为两点：

> - 其一， 给定h，如何算取[1， 2， 4， 8]的数量为h的合法组合（的和），自然也有m对应的
> - 其二， 在第一点解决后，如何将给定h情况下，不同的小时对应的取值和分钟对应的取值组合起来

以上，难点在于 如何恰当地实现对组合数的求解

> 给定一个长度为n的列表，求长度为h的不同组合 （注意，需要的是具体的组合，不是组合数）



<!-- more -->


```python
def combinations(nums: list, h: int) -> list[list]:
    pass

```

其实回顾一下我们小学是怎么求组合的，这个问题也就不攻自破了：

> 假定有一列数：a, b, c, d, e, ...... 共n个
>
> 我们要求它的长度为k的组合，那么可以分两步：
>
> 1. 选择第一个数，从剩下的n-1个数里选择k-1个
> 2. 第一个数不选，从剩下的n-1个数里选择k个数

有没有受到一点启发？我们减小了问题的规模😯

再考虑一下边界情况，我们就可以轻松实现求解所有的长度为h的组合啦

> 1. 如果h > n, 不满足组合的定义，用assert报错
> 2. 如果列表的长度就是h，直接返回整个列表就好
> 3. 如果h==0: 返回一个空列表
> 4. 如果 h==1, 那直接枚举每一种可能

话不多说，上代码：

```python
def combinations(nums: list, h: int):
    n = len(nums)
    # 如果h > n, 不满足组合的定义，用assert报错
    assert h <= n, "h should be less than or eqaul to n!"
    # 如果列表的长度就是h，直接返回整个列表就好
    if n == h: return [nums]
    # 如果h==0: 返回一个空列表
    if h == 0: return [[]]

    res = []
    # 如果 h==1, 那直接枚举每一种可能
    if h == 1:
        for num in nums:
            res.append([num])
        return res
    # 主要情形：1 < h < n

    # case 1: 选第一个，从剩下的n-1个里选h-1个
    for h_1_list in combinations(nums[1:], h-1):
        res.append([nums[0]] + h_1_list)
    # case 2: 不选第一个，从剩下的n-1个里选h个
    for h_list in combinations(nums[1:], h):
        res.append(h_list)
    return res
```

[我写的题解](https://github.com/leetcode-pp/91alg-3/issues/45#issuecomment-797876551)
