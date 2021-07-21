---
title: 拆解复杂问题-实现计算器(Python版本)
date: 2021-04-30T16:31:10+08:00
tags: [Algorithm]
categories: Notes
mathjax: true
---
参考：[labuladong](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484903&idx=1&sn=184beaad36a71c9a8dd93c41a8ba74ac&chksm=9bd7fbefaca072f9beccff92a715d92ee90f46c297277eec10c322bc5ccd053460da6afb76c2&scene=21#wechat_redirect): 通过层层拆解问题，实现一个带括号的四则运算计算器

🚀**Final Goal**:

- 输入一个合法运算表达式字符串，包含 `+`. `-`, `*`, `/`, `(`, `)`, `数字`, `空格`；输出运算结果
- 要符合通常的运算法则，即括号优先，先乘除后加减
- 除号是整数除法，无论正负都向 0 取整（5/2=2，-5/2=-2）

<!-- more -->

```python
## 乘除法优先于加减法体现在，乘除法可以和栈顶的数结合，而加减法只能把自己放入栈， 这就是为什么要使用栈的原因
s = '1 - 42 + 3 + 3 * 7'
def calculator(s):
    stack = []
    sign = '+'
    num = 0
    for i in range(len(s)):
        c = s[i]
        if c.isdigit():
            num = 10*num + int(c)
        if c == ' ': continue # 处理空格
        if not c.isdigit() or i == len(s) - 1:
            if sign == '+':
                stack.append(num)
            elif sign == '-':
                stack.append(-num)
            #乘除法拿出栈顶元素参与运算再入栈
            elif sign == '*':
                top = stack.pop()
                stack.append(top*num)
            elif sign == '/':
                top = stack.pop()
                stack.append(int(top/num)) #注意保留靠近0的整数部分
            num = 0
            sign = c

    return sum(stack)
calculator(s)
```

```python
# 处理括号
## 括号具有递归性质, 遇到`(`开始递归，遇到`）`结束递归
### 由此知，还需要一个栈来辅助完成递归, （一种直观的写法就是写成递归函数的形式, 如labuladong里面的
def calculate(s: str) -> int:

    def helper(s) -> int:
        stack = []
        sign = '+'
        num = 0

        while len(s) > 0:
            c = s.pop(0)
            if c.isdigit():
                num = 10 * num + int(c)
            # 遇到左括号开始递归计算 num
            if c == '(':
                num = helper(s)

            if (not c.isdigit() and c != ' ') or len(s) == 0:
                if sign == '+': stack.append(num)
                elif sign == '-': stack.append(-num)
                elif sign == '*':
                    top = stack.pop()
                    stack.append(top*num)
                elif sign == '/':
                    top = stack.pop()
                    stack.append(int(top/num))
                num = 0
                sign = c
            # 遇到右括号返回递归结果
            if c == ')': break
        return sum(stack)

    return helper(list(s))
s = '1 - ((2 * (4 + 3) - 44) / 4)'
calculate(s)
```

目标：尝试加一个栈写出迭代形式：

```python

```
