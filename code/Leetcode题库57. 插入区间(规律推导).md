#### Leetcode题库57. 插入区间(规律推导)

给出一个无重叠的 ，按照区间起始端点排序的区间列表。

在列表中插入一个新的区间，你需要确保列表中的区间仍然有序且不重叠（如果有必要的话，可以合并区间）。

示例 1:

```
输入: intervals = [[1,3],[6,9]], newInterval = [2,5]
输出: [[1,5],[6,9]]
```


示例 2:

```
输入: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出: [[1,2],[3,10],[12,16]]
解释: 这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

思路：

​	这个和Leetcode56题目一样的道理，也是使用排序合并数组，这里需要注意的是需要新插入数组的情况，插入数组后再排序即可。

第一版：

```python
class Solution(object):
    def insert(self, intervals, newInterval):
        """
        :type intervals: List[List[int]]
        :type newInterval: List[int]
        :rtype: List[List[int]]
        """
        # 注意排除特殊情况
        if intervals == [] and newInterval == []:   return [[]]
        result = []
        intervals.append(newInterval)
        # 以x[0]进行排序
        intervals.sort(key = lambda x:x[0])
		# 使用与56的处理方法一致
        for item in intervals:
            if not result or result[-1][-1] < item[0]:
                result.append(item)
            else:
                result[-1][-1] = max(result[-1][-1], item[1])
        return result
```
