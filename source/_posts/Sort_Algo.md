---
title: Sort Algorithm
tags: [Algorithm]
categories: Notes
mathjax: true
---
## 冒泡排序 Bubble Sort

```python
class Solution:
    def MySort(self , arr ):
#         write code here
#         return sorted(arr)
        #冒泡排序 O(n**2)
        n = len(arr)
        left = 0
        while left < n-1:
            right = left + 1
            while right < n:
                if arr[left] > arr[right]:
                    arr[left], arr[right] = arr[right], arr[left]
                right += 1
            left += 1
        return arr
```

## 桶排序 Bucket Sort

基本思路是：

-  将待排序元素划分到不同的痛。先扫描一遍序列求出最大值 maxV 和最小值 minV ，设桶的个数为 k ，则把区间 [minV, maxV] 均匀划分成 k 个区间，每个区间就是一个桶。将序列中的元素分配到各自的桶。
  对每个桶内的元素进行排序。可以选择任意一种排序算法, 将各个桶中的元素合并成一个大的有序序列。
- 假设数据是均匀分布的，则每个桶的元素平均个数为 n/k 。假设选择用快速排序对每个桶内的元素进行排序，那么每次排序的时间复杂度为 O(n/klog(n/k)) 。总的时间复杂度为 O(n)+O(k)O(n/klog(n/k)) = O(n+nlog(n/k)) = O(n+nlogn-nlogk 。当 k 接近于 n 时，桶排序的时间复杂度就可以金斯认为是 O(n) 的。即桶越多，时间效率就越高，而桶越多，空间就越大。

<!-- more -->

原文链接：https://blog.csdn.net/qq_19446965/article/details/81517552

## 计数排序 Counting Sort

- 是一种O(n)的排序算法，其思路是开一个长度为 maxValue-minValue+1 的数组，然后分配。扫描一遍原始数组，以当前值- minValue 作为下标，将该下标的计数器增1。收集。扫描一遍计数器数组，按顺序把值收集起来。
- 举个例子， nums=[2, 1, 3, 1, 5] , 首先扫描一遍获取最小值和最大值， maxValue=5 , minValue=1 ，于是开一个长度为5的计数器数组 counter ，

1. 分配。统计每个元素出现的频率，得到 counter=[2, 1, 1, 0, 1] ，例如 counter[0] 表示值 0+minValue=1 出现了2次。
2. 收集。 counter[0]=2 表示 1 出现了两次，那就向原始数组写入两个1， counter[1]=1 表示 2 出现了1次，那就向原始数组写入一个2，依次类推，最终原始数组变为 [1,1,2,3,5] ，排序好了。

计数排序本质上是一种特殊的桶排序，当桶的个数最大的时候，就是计数排序。

原文链接：https://blog.csdn.net/qq_19446965/article/details/81517552

## 基数排序

> 待补充

## 选择排序

```python
class Solution:
    def MySort(self , arr ):
#         write code here
        def find_smallest(arr):
            mini = float('inf')
            res = -1
            for index, a in enumerate(arr):
                if mini > a:
                    mini = a
                    res = index
            return res
        res = []
        n = len(arr)
        for _ in range(n):
            smallest = find_smallest(arr)
            res.append(arr.pop(smallest))
        return res
```

## 快速排序 Quick Sort

> 本质：分而治之 （Divide and Conquer, D&C）

- 找基准值 （pivot element）
- 分区 （partitioning）

**虚假的快排**

```python
class Solution:
    def MySort(self , arr ):
#       write code here
        # 快速排序
        if len(arr) < 2:
            return arr
        else:
            pivot = arr[0]
            less = [i for i in arr[1:] if i <= pivot]
            greater = [i for i in arr[1:] if i > pivot]
            return self.MySort(less) + [pivot] + self.MySort(greater)
```

**真实的快排**

**一. 挖坑填数分区法**

```python
def partition1(arr, left, right):
    """挖坑填数"""
    pivot = arr[left]
    while left < right:
        while left < right and arr[right] >= pivot: right -= 1
        if left < right: arr[left] = arr[right]
        while left < right and arr[left] <= pivot: left += 1
        if left < right: arr[right] = arr[left]
    arr[left] = pivot
    return left
```

**二. 双指针前后互换法** 注：需加强练习

```python
def partition2(arr, left, right):
    """双指针交换"""
    pivot = arr[left]
    index = left
    while left < right:
        while left <= right and arr[left] <= pivot: left += 1
        while left <= right and arr[right] >= pivot: right -= 1
        if left > right: break
        arr[left], arr[right] = arr[right], arr[left]
    arr[index], arr[right] = arr[right], arr[index]
    return right

def partition2_mid(arr, left, right):
    """双指针交换 + 三数取中优化"""
    mid = (left + right) >> 1
    if arr[left] > arr[right]: arr[left], arr[right] = arr[right], arr[left]
    if arr[mid] > arr[right]: arr[mid], arr[right] = arr[right], arr[mid]
    if arr[mid] > arr[left]: arr[mid], arr[left] = arr[left], arr[mid]
    pivot = arr[left]
    index = left
    while left < right:
        while left <= right and arr[left] <= pivot: left += 1
        while left <= right and arr[right] >= pivot: right -= 1
        if left > right: break
        arr[left], arr[right] = arr[right], arr[left]
    arr[index], arr[right] = arr[right], arr[index]
    return right
```

**三. 三路快排法**

```python
#优选
def partition3(arr, left, right):
    """三路快排 + 三数取中"""
    mid = (left + right) >> 1
    if arr[left] > arr[right]: arr[left], arr[right] = arr[right], arr[left]
    if arr[mid] > arr[right]: arr[mid], arr[right] = arr[right], arr[mid]
    if arr[mid] > arr[left]: arr[mid], arr[left] = arr[left], arr[mid]
    pivot = arr[left]
    i = left + 1
    while i <= right:
        if arr[i] > pivot:
            arr[i], arr[right] = arr[right], arr[i]
            right -= 1
        if arr[i] < pivot:
            arr[i], arr[left] = arr[left], arr[i]
            i += 1
            left += 1
        if arr[i] == pivot:
            i += 1
    return left, right
```

主体：

```python
def quick_sort(arr, left, right):
    if left >= right: return
    index = partition2(arr, left, right)
    quick_sort(arr, left, index-1)
    quick_sort(arr, index+1, right)

def quick_sort3(arr, left, right): # for partition3
    if left >= right: return
    low, high = partition3(arr, left, right)
    quick_sort(arr, left, low-1)
    quick_sort(arr, high+1, right)
```

## 归并排序 Merge Sort

> 待补充！2020/03/09

> 快速查找的常量比归并查找的小

```python
class Solution:
    def MySort(self , arr ):
#       write code here
        n = len(arr)
        if n < 2:
            return arr
        m = (0 + n - 1) >> 1
        # 分
        left_part = self.MySort(arr[:m+1])
        right_part = self.MySort(arr[m+1:])
        # 治
        merge_list = []
        while left_part and right_part:
            if left_part[0] <=  right_part[0]:
                merge_list.append(left_part.pop(0))
            else:
                merge_list.append(right_part.pop(0))
        rest_ = left_part if left_part else right_part
        merge_list.extend(rest_)
        return merge_list
```

> 上述写法，切片和pop耗时太多，改为下面写法，不切片，不pop

```python
class Solution:
    def MySort(self , arr ):
#       write code here
        # 修改， 不使用切片和pop
        # 先定义 治
        def merge(arr, start, mid, end):
            # i for left_part; j for right_part
            i, j = start, mid + 1
            tmp = []
            while i <= mid and j <= end:
                if arr[i] <= arr[j]:
                    tmp.append(arr[i])
                    i += 1
                else:
                    tmp.append(arr[j])
                    j += 1
            while i <= mid:
                tmp.append(arr[i])
                i += 1
            while j <= end:
                tmp.append(arr[j])
                j += 1
            for i in range(len(tmp)):
                arr[start + i] = tmp[i]
        # 再定义 分
        def merge_sort(arr, start, end):
#             print(start, end)
            if start >= end: return
            mid = (start + end) >> 1
            merge_sort(arr, start, mid)
            merge_sort(arr, mid+1, end)
            merge(arr, start, mid, end)
        merge_sort(arr, 0, len(arr) - 1)
        return arr
```
