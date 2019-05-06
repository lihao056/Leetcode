#### Leetcode题库34. 在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 `nums`，和一个目标值 `target`。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

如果数组中不存在目标值，返回 `[-1, -1]`。

**示例 1:**

```
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
```

**示例 2:**

```
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]
```

思路：

​	问题的主要问题是复杂度为 *O*(log *n*)，所以肯定使用了二分查找的方法，而且和题号32很相似。

​	我看题目中很多使用  x in list的方法判断，我觉得这样子也可以，但是题目的原意就违背了。

​	所以我没有这样子做，同样是使用二分查找的方法进行查询，33题的解法还是不熟，这题运用的比较熟练

第一版:

```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        i, j = 0, len(nums) - 1
        # 只有i > j时才终止
        while i <= j:
            # 选取中间节点
            mid = i + int((j - i)/2)
            if nums[mid] < target:
                i = mid + 1
            elif nums[mid] > target:
                j = mid - 1
            # 由于题目中没有重复值，所以可以直接将nums[mid]的值舍去
            # 若是相等的话，则需要做以下的操作
            else:
                min_t = max_t = mid
                # 找到最前和最后位置的值
                while min_t - 1 >= 0 and nums[min_t - 1] == target:
                        min_t -= 1
                while max_t + 1 < len(nums) and nums[max_t + 1] == target:
                        max_t += 1
                return [min_t, max_t]
        return [-1, -1]
```

