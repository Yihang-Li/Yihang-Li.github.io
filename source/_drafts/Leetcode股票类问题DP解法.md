---
title: Leetcode股票类问题DP解法
tags: [Leetcode, DP]
categories: Leetcode
---
> Deterministic    finite-state    forward in time

- stage —— k: 时间（天）， 对应数组下标
- state —— x_k: ~~当前的股票价格~~当前的持有状态
- control —— u_k：{买入，卖出，不交易} := {buy， sell， rest}
- disturbance —— \omega_k: ~~无？~~ 当前的股票价格？price[k]
- system dynamics ——x_{k+1} = f(x_k, u_k, \omega_k) := 新的的持有状态

  > 从system dynamics 来看，state设置为股票价格似乎是不合理的，因为股票价格的变动在这个情境中是独立于你的u_k的。那么是什么呢？labladong上说是（天数，允许交易的最大次数，当前的持有状态）， 看来 设置为 当前的持有状态 比较合适，这一点也是与u_k相应的。那这样看来disturbance是股票价格了？
  >
- stage cost —— g_k(x_k, u_k, \omega_k): buy -> price[k]; sell -> -price[k], rest -> 0
- Terminal cost ——g_N = ?  是base case吗?

Figure these things out
