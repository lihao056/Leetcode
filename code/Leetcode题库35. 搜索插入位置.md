#### Leetcode题库35. 搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

**示例 1:**

```
输入: [1,3,5,6], 5
输出: 2
```

**示例 2:**

```
输入: [1,3,5,6], 2
输出: 1
```

**示例 3:**

```
输入: [1,3,5,6], 7
输出: 4
```

**示例 4:**

```
输入: [1,3,5,6], 0
输出: 0
```

思路：

​	本题没有规定复杂度，所以只需按照顺序遍历就可以了。

​	我们使用二分查找的方法，可能会比较绕，但是熟练一下总是好的

第一版:

```python
class Solution(object):
    def searchInsert(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums) == 0:
            return 0
        i, j = 0, len(nums) - 1
        # 同样都是确定mid的值
        while i <= j:
            mid = i + int((j - i)/2)
            if nums[mid] > target:
                j = mid - 1
            elif nums[mid] < target:
                i = mid + 1
            else:
                return mid
        # 多的一步是有可能mid的值是一个小数，则需要将位置增加1
        if nums[mid] > target:
            return mid
        else:
            return mid + 1
```

