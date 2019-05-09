#### Leetcode题库41. 缺失的第一个正数

给定一个未排序的整数数组，找出其中没有出现的最小的正整数。

**示例 1:**

```
输入: [1,2,0]
输出: 3
```

**示例 2:**

```
输入: [3,4,-1,1]
输出: 2
```

**示例 3:**

```
输入: [7,8,9,11,12]
输出: 1
```

思路：

​	这道题目思路是怎么判断最小的正数

​	可能我也用了取巧的方法，使用not in的方法查找是否存在与数组中。

​	

第一版：

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        # 这一步完全没有必要，也可以直接查找，这一步只是把正数提取出来
        lis = []
        for item in nums:
            if item > 0:
                lis.append(item)
        # 从1开始遍历，不存在就跳出
        i = 1
        while True:
            if i not in lis:
                return i
            i += 1
        
```



