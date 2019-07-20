#### Leetcode题库56. 合并区间(规律推导)

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

```
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```


示例 2:

```
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

思路：

​	一开始想着没有什么算法可以套，所以想着看看有没有规律可以找，

​	首先题目要求要合并重叠的区间，说明一个区间的尾部很可能小于另一个区间头部，基于这种情况，所以可以使用排序算法将数组进行排序，判断是否会有合并的情况。

第一版：

​	

```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        if len(intervals) == 0: return []
        nums = sorted(intervals)
        result = []
        # 将数组里的区间拆开，组成(*, 1)的元组再进行排序
        for item in nums:
            for i, item_1 in enumerate(item):
                result.append((item_1, i))
        result = sorted(result)
        # time用来指示是否可以组成一个区间，只有当time为0才能合并区间
        # 经过排序肯定是由(*, 0)开始的，同时会以(*, 1)结束，并且会成对出现
        time = 0
        final_result = []
        for item in result:
            if time == 0:
                start = item[0]
            if item[1] == 0:
                time += 1
            else:
                time -= 1
            if time == 0:
                end = item[0]
                final_result.append([start, end])
        return final_result
```

第二版：

​	这是Leetcode中耗时80ms的代码，它的想法也是通过排序进行合并。

​	巧妙的是对结果进行修改合并区间，最后得到目的结果。

```python
class Solution(object):
    def merge(self, intervals):
        """
        :type intervals: List[List[int]]
        :rtype: List[List[int]]
        """
        if len(intervals) <= 1:
            return intervals
        # 使用x[0]进行排序
        intervals.sort(key=lambda x:x[0])
        merged = []
        
    	# 通过merged[-1][-1]表示最后一个区间的尾部进行修改，完成合并操作。
        for interval in intervals:
            if not merged or merged[-1][-1] < interval[0]:
                merged.append(interval)
            else:
                merged[-1][-1] = max(interval[-1], merged[-1][-1])
                
        return merged
```







