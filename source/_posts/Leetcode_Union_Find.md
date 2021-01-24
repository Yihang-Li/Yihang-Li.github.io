---
title: Leetcode with Union Find
date: 2021-01-24T13:15:32+08:00
tags: [Leetcode, Union Find]
categories: Leetcode
---
## 并查集算法

* [X] 动手使用python实现`并查集`4个版本🚀️
* [ ] 应用刷题！🎉️：刷完[并查集tag](https://leetcode-cn.com/tag/union-find/)再勾（4/39）

<!-- more -->

### 动手实现

#### API

```python
class Union_Find():
	def __init__(self, n):
		"""Initialize a list of length n: n nodes as index and root as value,
		count: the number of disjoint set"""
		pass

  def is_connected(self, p: int, q: int)->bool:
    """decide if p and q are in the same disjoint set """
    pass
	def union(self, p: int, q: int):
		"""connect node p and node q """
		pass
	def find(self, p:int)->int:
		"""find the root of p"""
		pass

```

#### First Version: quick-find

> uf 初始化为index：结点， value： 结点自身
>
> if uf[p] == uf[q]: p, q 在同一个连通分量

```python
# quick-find
class Union_Find():
	def __init__(self, n: int):
		"""Initialize a list: Nodes as index and root as value,
		count: the number of disjoint set, """
		self.len = n
		self.uf = [i for i in range(self.len)]
		self.count = n

	def find(self, p:int)->int:
		"""find the root of p"""
		return self.uf[p]

	def is_connected(self, p: int, q: int)->bool:
		"""decide if p and q are in the same disjoint set """
		return self.find(p) == self.find(q)


	def union(self, p: int, q: int):
		"""connect node p and node q """
		if self.is_connected(p, q): return
		p_root, q_root = self.find(p), self.find(q)
		for i in range(self.len):
			if self.find(i) == p_root:
				self.uf[i] = q_root
		self.count -= 1
```

> 算法分析：find() 操作速度很快，但是对于每一对需要连接的输入，union() 都需要扫描整个 uf 数组
>
> ```
> quick-find 算法一般不能处理大型问题
> ```
>
> ```
>> quick-find ～
> ```
>
> $O(n^2)$

#### Second Version: quick-union

> uf 用父链接的形式表示了一片森林（每个父链接对应一棵树，既一个连通分量）
>
> 数组uf 中元素都是同一个连通分量中另一个结点的名称，当为它自身时表示其为该连通分量中的根结点
>
> 由结点p, q 的链接分别找到它们的根结点， 然后将一个根结点链接到另一个即可连通两个连通分量

```python
# quick-union
class Union_Find():
	def __init__(self, n: int):
		"""Initialize a list: Nodes as index and root as value,
		count: the number of disjoint set, """
		self.len = n
		self.uf = [i for i in range(self.len)]
		self.count = n

	def find(self, p:int)->int:
		"""find the root of p"""
		while self.uf[p] != p: p = self.uf[p]
		return p

	def is_connected(self, p: int, q: int)->bool:
		"""decide if p and q are in the same disjoint set """
		return self.find(p) == self.find(q)


	def union(self, p: int, q: int):
		"""connect node p and node q """
		p_root, q_root = self.find(p), self.find(q)
		if p_root == q_root: return
		self.uf[p_root] = q_root
		self.count -= 1
```

> 算法分析： 最好情况～$O(n)$, 最差情况~$O(n^2)$
>
> ```
> quick-find 的改进，解决了其最主要的问题：使得union的操作变成了线性
> ```
>
> ```
> 不能保证所有情况下它都比quick-find快
> ```

#### Third Version: weighted-quick-union

> 记录每一棵树🌲的大小（也就是我们的weight），在union合并操作的时候总是将小树链接到大树上
>
> 空间换时间

```python
# weighted-quick-union
class Union_Find():
	def __init__(self, n: int):
		"""Initialize a list: Nodes as index and root as value,
		count: the number of disjoint set, """
		self.len = n
		self.uf = [i for i in range(self.len)]
		self.count = n
		self.size = [1 for i in range(self.len)]

	def find(self, p:int)->int:
		"""find the root of p"""
		while self.uf[p] != p: p = self.uf[p]
		return p

	def is_connected(self, p: int, q: int)->bool:
		"""decide if p and q are in the same disjoint set """
		return self.find(p) == self.find(q)


	def union(self, p: int, q: int):
		"""connect node p and node q """
		p_root, q_root = self.find(p), self.find(q)
		if p_root == q_root: return
		if self.size[p_root] < self.size[q_root]:
			self.uf[p_root] = q_root
			self.size[q_root] += self.size[p_root]
		else:
			self.uf[q_root] = p_root
			self.size[p_root] += self.size[q_root]
		self.count -= 1
```

> 算法分析： weighted-quick-union ~ $O(log(n))$
>
> ```
>> 这三种算法中唯一能用于解决大型实际问题的算法🎉
> ```

#### Optimal Version: weighted-quick-union with path compression

> 希望每个结点都直接链接到它的根结点上，但又不想像quick-find那样在union中通过for循环修改所有结点链接——>路径压缩
>
> 在find结点的同时将它们链接到根结点：为find添加一个循环将在路径上遇到的所有节点都直接链接到根结点
>
> 得到几乎完全扁平化的树🌲

```python
# weighted-quick-union
class Union_Find():
	def __init__(self, n: int):
		"""Initialize a list: Nodes as index and root as value,
		count: the number of disjoint set, """
		self.len = n
		self.uf = [i for i in range(self.len)]
		self.count = n
		self.size = [1 for i in range(self.len)]

	def find(self, p:int)->int:
		"""find the root of p"""
		res = p
		while self.uf[res] != res: res = self.uf[res]
		while self.uf[p] != p: p, self.uf[p] = self.uf[p], res
		return res

	def is_connected(self, p: int, q: int)->bool:
		"""decide if p and q are in the same disjoint set """
		return self.find(p) == self.find(q)


	def union(self, p: int, q: int):
		"""connect node p and node q """
		p_root, q_root = self.find(p), self.find(q)
		if p_root == q_root: return
		if self.size[p_root] < self.size[q_root]:
			self.uf[p_root] = q_root
			self.size[q_root] += self.size[p_root]
		else:
			self.uf[q_root] = p_root
			self.size[p_root] += self.size[q_root]
		self.count -= 1
```

> 算法分析：weighted-quick-union with path compression 是这四者中最优的算法
>
> ```
> 但并非所有操作都能在常数时间内完成
> ```

### 应用刷题

- [X] [1319](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)

```python
class Solution:
    def makeConnected(self, n: int, connections: List[List[int]]) -> int:
        if len(connections) < n - 1: return -1
        uf = Union_Find(n)
        edges = 0
        for i, j in connections:
            if uf.is_connected(i, j):
                edges += 1
                continue
            uf.union(i, j)
        if edges >= uf.count - 1:
            return uf.count - 1
        return -1
```

* [X] [547](https://leetcode-cn.com/problems/number-of-provinces/)

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        uf = Union_Find(n)
        for i in range(n-1):
            for j in range(i+1, n):
                if isConnected[i][j]:
                    uf.union(i, j)
        return uf.count
```

* [X] [778](https://leetcode-cn.com/problems/swim-in-rising-water/)

```python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        n = len(grid)
        uf = Union_Find(n*n)
        def height_t(t):
            for i in range(n):
                for j in range(n):
                    if grid[i][j] == t:
                        return i, j
                    j += 1
                i += 1
        for t in range(1, n*n):
            i, j = height_t(t)
            if i-1 >= 0 and grid[i-1][j] <= t:
                uf.union(grid[i][j], grid[i-1][j])
            if i+1 < n and grid[i+1][j] <= t:
                uf.union(grid[i][j], grid[i+1][j])
            if j-1 >= 0 and grid[i][j-1] <= t:
                uf.union(grid[i][j], grid[i][j-1])
            if j+1 < n and grid[i][j+1] <= t:
                uf.union(grid[i][j], grid[i][j+1])
            if uf.is_connected(grid[0][0], grid[n-1][n-1]):
                return t
        return t
```

* [X] [990](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)

```python
class Solution:
    def equationsPossible(self, equations: List[str]) -> bool:

        alpha_set = set()
        alpha_dict = dict()

        for equation in equations:
            alpha_set.add(equation[0])
            alpha_set.add(equation[3])

        n = 0
        for i in alpha_set:
            alpha_dict[i] = n
            n += 1

        uf = Union_Find(n)

        equations.sort(key=lambda x: x[1], reverse=True)

        for equation in equations:
            if equation[1] == '=':
                uf.union(alpha_dict[equation[0]], alpha_dict[equation[3]])
            elif uf.is_connected(alpha_dict[equation[0]], alpha_dict[equation[3]]):
                return False
        return True

```

* [ ] 未完待续



> 并查集相关的题目不止是并查集这么简单，并查集只是可以用来辅助的一种数据结构！

### Reference

> [Union-Find 并查集算法详解](https://mp.weixin.qq.com/s?__biz=MzAxODQxMDM0Mw==&mid=2247484751&idx=1&sn=a873c1f51d601bac17f5078c408cc3f6&chksm=9bd7fb47aca07251dd9146e745b4cc5cfdbc527abe93767691732dfba166dfc02fbb7237ddbf&scene=21#wechat_redirect)

- 主要解决图论中动态连通性问题
- Key poits: `union` and `connected`
- 并查集也被称为不相交集数据结构。顾名思义，并查集主要操作是合并与查询，它是把初始不相交的集合经过多次合并操作后合并为一个大集合，然后可以通过查询判断两个元素是否已经在同一个集合中了。
- 并查集的应用场景一般就是动态连通性的判断，例如判断网络中的两台电脑是否连通，在程序中判断两个变量名是否指向同一内存地址等。

> ***[Python 实现并查集](https://www.cnblogs.com/yscl/p/10185293.html)***

> ***[并查集与合并-查找算法的 Python 实现](http://siliconraleigh.com/2018/01/18/Union-Find/)***

> 算法4—— 1.5 案例研究：union-find 算法
