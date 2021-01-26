---
title: Graph
tags: [Leetcode, Graph]
categories: Leetcode
---
* [X] 👀️ 图在Python中的表示方法有哪些？
* [X] 🎉️ DFS
* [ ] BFS
* [ ] Dijkstra

<!--more-->

## 灵魂发问

> 要实现图相关的算法涉及到一个问题：图这种数据结构在Python中怎么表示？---> Task1: 弄清python中图的相关表示

## 图在Python中的表示方法

### 邻接矩阵

> 这个应该蛮简单的

### 邻接链表

> Python没有指针，那么邻接链表怎么表示出来呢，看样子这部分是重点

解决方案：

> 参考这篇：[常见的图结构表示(python)](https://blog.csdn.net/u014281392/article/details/79120406)
>
> 还有这个：[Python数据结构与算法视频教程](https://python-data-structures-and-algorithms.readthedocs.io/zh/latest/18_%E5%9B%BE%E4%B8%8E%E5%9B%BE%E7%9A%84%E9%81%8D%E5%8E%86/graph/)

## 深度优先搜索（DFS）

### Intuitive —— 走迷宫

> Tremaux 搜索 （迷宫-图， 通道-边， 路口-顶点）
>
> - 选择一条未标记过的通道，在走过的路上铺一条绳子
> - 标记所有第一次路过的路口和通道
> - 当来到一个标记过的路口时回退到上个路口
> - 当回退到的路口已没有可走的通道时继续回退
>
>> 划重点：`绳子`, `标记`
>>

### 动手实现

#### Version1——递归

> 和Tremaux类似，访问其中一个顶点时：
>
> - 标记它为已访问
> - 递归(`绳子`)地访问它所有未标记过的邻结点
>
> Note: 若图是连通的，则每个顶点都会被访问到

```python
#递归
def DepthFirstSearch(G, s):
    marked = set()
    def dfs(G, s):
        if s not in marked:
            #print(s) #入栈时访问
            marked.add(s)
        for node in G[s]:
            if node not in marked:
                dfs(G, node)
        print(s) # 出栈时访问
    dfs(G, s)
```

#### Version2 —— 辅助栈

> 注意每一次只入栈一个未标记的邻结点，最后一个未标记的邻结点都找不到了才出栈访问

```python
#辅助栈
def DepthFirstSearch(G, s):
    marked = set()
    stack = [s]
    while stack:
        tmp = stack[-1]
        if tmp not in marked:
            # print(tmp) # 入栈时访问
            marked.add(tmp)
        flag = True
        for neighbor in G[tmp]:
            if neighbor not in marked:
                # print(neighbor)
                stack.append(neighbor)
                flag = False
                break
        if flag:
            res = stack.pop()
            print(res) # 出栈时访问
```

#### 测试

- 测试用例

```python
G = {
    'A': ['B', 'F'],
    'B': ['C', 'I', 'G'],
    'C': ['B', 'I', 'D'],
    'D': ['C', 'I', 'G', 'H', 'E'],
    'E': ['D', 'H', 'F'],
    'F': ['A', 'G', 'E'],
    'G': ['B', 'F', 'H', 'D'],
    'H': ['G', 'D', 'E'],
    'I': ['B', 'C', 'D'],
}
```

> 算法遍历边和访问顶点的顺序与图的表示是有关的，而不是仅与图的结构或算法有关

- 测试结果

- [X] 入栈时访问/出栈时访问
  - [X] 递归
  - [X] 显式栈

### 应用刷题

> 在应用的时候并非按部就班的套以上实现代码，重点在于算法思想的应用
>
> 这是区别于并查集的地方，原因在于并查集算是一种数据结构，而这个是实打实的算法

* [X] [200](https://leetcode-cn.com/problems/number-of-islands/)

```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        #DFS
        def dfs(i, j):
            grid[i][j] = 0
            if i > 0 and grid[i-1][j] == "1": dfs(i-1, j)
            if i < row - 1 and grid[i+1][j] == "1": dfs(i+1, j)
            if j > 0 and grid[i][j-1] == "1": dfs(i, j-1)
            if j < col - 1 and grid[i][j+1] == "1": dfs(i, j+1)

        row, col = len(grid), len(grid[0])
        res = 0
        for i in range(row):
            for j in range(col):
                if grid[i][j] == "1":
                    res += 1
                    dfs(i, j)
        return res
```

- [X] [111](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```python
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        #DFS
        if not root: return 0
        if not root.left and not root.right: return 1
        left = self.minDepth(root.left)
        right = self.minDepth(root.right)
        if not root.left or not root.right: return 1 + left + right
        return 1 + min(left, right)
```




## 广度优先搜索 （BFS）

> 启发问题：单点最短路径   DFS无所作为，BFS应运而生
>
> 一组人一起朝各个方向走迷宫，每个人都有自己的绳子。出现新岔路时，一个探索者分裂为更多的人来探索；两个探索者相遇时合体，继续使用先到达者的绳子

### 动手实现

#### 辅助队列 （FIFO）

```python
# %%
import collections
def BreathFirstSearch(G, s):
    marked = set()
    queue = collections.deque()
    queue.append(s)
    while queue:
        tmp = queue.popleft()
        if tmp not in marked:
            print(tmp) # 访问
            marked.add(tmp)
        for neighbor in G[tmp]:
            if neighbor not in marked:
                queue.append(neighbor)
```

#### 测试

- 测试用例同DFS
- 测试结果

- [X] 一种访问方式

### 应用刷题

* [X] [111](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        # BFS
        if not root: return 0
        res = 1
        queue = collections.deque()
        queue.append(root)
        # marked = set()
        # marked.add(root)
        while queue:
            for _ in range(len(queue)):
                tmp = queue.popleft()
                # if tmp not in marked:
                    # marked.add(tmp)
                if tmp.left: queue.append(tmp.left)
                if tmp.right: queue.append(tmp.right)
                if not (tmp.left or tmp.right):
                    return res
            res += 1
```

* [ ] [752](https://leetcode-cn.com/problems/open-the-lock/)

```python
def children(s):
    a, b, c, d = int(s[0]), int(s[1]), int(s[2]), int(s[3])
    child = []
    #first
    if a > 0: child.append(str(a-1)+s[1:])
    else: child.append('9'+s[1:])
    if a < 9: child.append(str(a+1)+s[1:])
    else: child.append('0'+s[1:])
    #second
    if b > 0: child.append(s[0] + str(b-1)+s[2:])
    else: child.append(s[0] + '9'+ s[2:])
    if b < 9: child.append(s[0] + str(b+1)+s[2:])
    else: child.append(s[0]+'0'+s[2:])
    #third
    if c > 0: child.append(s[0:2] + str(c-1)+s[-1])
    else: child.append(s[0:2]+'9'+s[-1])
    if c < 9: child.append(s[0:2]+str(c+1)+s[-1])
    else: child.append(s[0:2]+'0'+s[-1])
    #fourth
    if d > 0: child.append(s[:3]+str(d-1))
    else: child.append(s[:3]+'9')
    if d < 9: child.append(s[:3]+str(d+1))
    else: child.append(s[:3]+'0')
    return child

class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        res = 0
        queue = collections.deque()
        root = '0000'
        if target == root: return res
        if root in deadends: return -1
        queue.append(root)
        marked = set()
        marked.add(root)
        while queue:
            for _ in range(len(queue)):
                tmp = queue.popleft()
                child = children(tmp)
                for ch in child:
                    if ch not in deadends and ch not in marked:
                        queue.append(ch)
                        marked.add(ch)
            res += 1
            if target in queue: return res

        return -1
```

> 优化思路: 双向BFS (前提：知道终点在哪)

```python
def children(s):
    a, b, c, d = int(s[0]), int(s[1]), int(s[2]), int(s[3])
    child = []
    #first
    if a > 0: child.append(str(a-1)+s[1:])
    else: child.append('9'+s[1:])
    if a < 9: child.append(str(a+1)+s[1:])
    else: child.append('0'+s[1:])
    #second
    if b > 0: child.append(s[0] + str(b-1)+s[2:])
    else: child.append(s[0] + '9'+ s[2:])
    if b < 9: child.append(s[0] + str(b+1)+s[2:])
    else: child.append(s[0]+'0'+s[2:])
    #third
    if c > 0: child.append(s[0:2] + str(c-1)+s[-1])
    else: child.append(s[0:2]+'9'+s[-1])
    if c < 9: child.append(s[0:2]+str(c+1)+s[-1])
    else: child.append(s[0:2]+'0'+s[-1])
    #fourth
    if d > 0: child.append(s[:3]+str(d-1))
    else: child.append(s[:3]+'9')
    if d < 9: child.append(s[:3]+str(d+1))
    else: child.append(s[:3]+'0')
    return child

class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        res = 0
        root = '0000'
        if target == root: return res
        if root in deadends: return -1
        #双向BFS
        q1, q2 = set(), set()
        marked = set()
        q1.add(root)
        q2.add(target)
        while q1 and q2:
            if len(q1) > len(q2):
                q1, q2 = q2, q1
            tmp = set()
            for p in q1:
                if p in q2:
                    return res
                marked.add(p)
                child = children(p)
                for ch in child:
                    if ch not in deadends and ch not in marked:
                        tmp.add(ch)
            res += 1
            q1 = q2
            q2 = tmp

        return -1
```



> DFS-线-Solo-递归好写

> BFS-面-团战-找最短路径适用


暂时到这儿了，有空再更新👀️



## Dijkstra

- Like BFS
- Prior Queue (use heap)
- Greedy
- Only consider distance from source

### 动手实现

### 应用刷题

## A* Algorithm

- Also consider distance to end (approximate value, e.g. Euclidian Dist)
- Bias-version BFS

### 动手实现

### 应用刷题

## Reference

> 《算法4》
