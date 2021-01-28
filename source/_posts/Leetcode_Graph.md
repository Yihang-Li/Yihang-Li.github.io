---
title: Graph
tags: [Leetcode, Graph]
categories: Leetcode
---
* [X] 👀️ 图在Python中的表示方法有哪些？
* [X] 🎉️ DFS
* [X] BFS
* [X] Priority Queue by heap
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

* [X] [752](https://leetcode-cn.com/problems/open-the-lock/)

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

## 优先队列 Prior Queue

一种抽象数据类型，实现`删除最小元素` （`删除最大元素`可通过相应转化改写得到）和 `插入元素`

### API  (堆实现 (Heap))

```python
#API
class MinPQ:
    def __init__(self): #初始化一个优先队列
        pass
    def insert(self, v): #向优先队列中插入一个元素
        pass
    def mini(self): # 返回最小元素
        pass
    def delMin(self): #删除并返回最小元素
        pass
    def isEmpty(self): #返回队列是否为空
        pass
    def size(self): # 返回优先队列中的元素个数
        pass
    def less(self, i, j): # 堆实现的比较方法
        pass
    def exch(self, i, j): # 堆实现的交换方法
        pass
    def swim(self, k): # 由下至上的堆有序化实现
        pass
    def sink(self, k): # 由上至下的堆有序化实现
        pass
    pass
```

> 最大堆（大顶堆) for MaxPQ; 最小堆（小顶堆）for MinPQ

> - 优先队列由一个基于堆的完全二叉树表示，存储于数组pq[1:N]中, pq[0]没有使用
> - 在Insert中，我们将N加一并把新元素添加在数组最后，然后用swim()恢复堆的秩序
> - 在delMin()中，我们从pq[1]得到需要返回的元素，然后将pq[N]移动到pq[1]，将N减1并用sink()恢复堆的秩序
> - 对于一个含有N个元素的基于堆的优先队列，插入元素操作只需不超过$(lgN+1)$次比较，删除最小元素的操作需要不超过$ 2lgN$次比较

### 具体实现

```python
class MinPQ:

    def __init__(self): #初始化一个优先队列
        self.pq = [0]
        self.N = 0

    def isEmpty(self): #返回队列是否为空
        return self.N == 0

    def size(self): # 返回优先队列中的元素个数
        return self.N

    def mini(self): # 返回最小元素
        if self.N == 0:
            return
        return self.pq[1]

    def insert(self, v): #向优先队列中插入一个元素
        self.pq.append(v)
        self.N += 1
        self.swim(self.N)

    def delMin(self): #删除并返回最小元素
        if self.N == 0:
            return
        res = self.pq[1]
        self.exch(1, self.N)
        self.pq.pop()
        self.N -= 1
        self.sink(1)
        return res

    def less(self, i, j): # 堆实现的比较方法
        return self.pq[i] < self.pq[j]

    def exch(self, i, j): # 堆实现的交换方法
        self.pq[i], self.pq[j] = self.pq[j], self.pq[i]

    def swim(self, k): # 由下至上的堆有序化实现
        while k > 1 and not self.less(k//2, k):
            self.exch(k//2, k)
            k = k//2

    def sink(self, k): # 由上至下的堆有序化实现
        while 2*k <= self.N:
            j = 2*k
            if j < self.N and not self.less(j, j+1):
                j += 1
            if self.less(k, j): break
            self.exch(k, j)
            k = j
```

### 测试用例

```python
PQ = MinPQ()
PQ.insert(7)
PQ.insert(6)
PQ.insert(5)
PQ.insert(4)
PQ.insert(3)
PQ.insert(2)
PQ.insert(1)
PQ.insert(0)
PQ.insert(-1)
PQ.insert(-2)
PQ.pq
# %% run 10 times
PQ.delMin()
PQ.pq
```

> 测试结果符合

### 应用刷题

* [X] [剑指Offer41](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

```python
## 改写MinPQ --> MaxPQ
class MaxPQ:

    def __init__(self): #初始化一个优先队列
        self.pq = [0]
        self.N = 0

    def isEmpty(self): #返回队列是否为空
        return self.N == 0

    def size(self): # 返回优先队列中的元素个数
        return self.N

    def maxi(self): # 返回最大元素
        if self.N == 0:
            return
        return self.pq[1]

    def insert(self, v): #向优先队列中插入一个元素
        self.pq.append(v)
        self.N += 1
        self.swim(self.N)

    def delMax(self): #删除并返回最大元素
        if self.N == 0:
            return
        res = self.pq[1]
        self.exch(1, self.N)
        self.pq.pop()
        self.N -= 1
        self.sink(1)
        return res

    def less(self, i, j): # 堆实现的比较方法
        return self.pq[i] < self.pq[j]

    def exch(self, i, j): # 堆实现的交换方法
        self.pq[i], self.pq[j] = self.pq[j], self.pq[i]

    def swim(self, k): # 由下至上的堆有序化实现
        while k > 1 and self.less(k//2, k):
            self.exch(k//2, k)
            k = k//2

    def sink(self, k): # 由上至下的堆有序化实现
        while 2*k <= self.N:
            j = 2*k
            if j < self.N and self.less(j, j+1):
                j += 1
            if not self.less(k, j): break
            self.exch(k, j)
            k = j
```

```python
#构造一个大顶堆，一个小顶堆，中位数在最后的maxi和mini中求得
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.maxpq = MaxPQ()
        self.minpq = MinPQ()


    def addNum(self, num: int) -> None:
        if self.maxpq.N == self.minpq.N:
            if self.minpq.N >= 1 and num > self.minpq.mini():
                num, self.minpq.pq[1] = self.minpq.pq[1], num
                self.minpq.sink(1)
            self.maxpq.insert(num)
        else:
            if num < self.maxpq.maxi():
                num, self.maxpq.pq[1] = self.maxpq.pq[1], num
                self.maxpq.sink(1)
            self.minpq.insert(num)


    def findMedian(self) -> float:
        if self.maxpq.N == self.minpq.N:
            return (self.maxpq.maxi() + self.minpq.mini())/2
        else:
            return self.maxpq.maxi()

```




## Dijkstra

- Like BFS
- `Prior Queue` (use `heap`)
- Greedy
- Only consider distance from source
- Non-negative weight on edges

### 动手实现


### 应用刷题


## A* Algorithm

- Also consider distance to end (approximate value, e.g. Euclidian Dist)
- Bias-version BFS

### 动手实现

### 应用刷题

## Reference

> 《算法4》
