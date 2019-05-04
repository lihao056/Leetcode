#### Leetcode题库33. 搜索旋转排序数组

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 `-1` 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 *O*(log *n*) 级别。

**示例 1:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

**示例 2:**

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

思路：

​	问题的主要问题是复杂度为 *O*(log *n*)，所以肯定使用了二分查找的方法。

​	所以先设立两个指针头，并做相应处理。

​	主要程序太复杂了，以后可以想想有没有更简单的方法。

第一版:

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        # 数组为空则返回-1
        if len(nums) == 0:
            return -1
        # res是用来存储目标值的位置信息
        res = 0
		# 当nums长度大于2时进行如下处理
        while len(nums) > 2:
            i = int(len(nums) / 2)
            # 由于是旋转数组，所以当nums[i]大于nums[0]，说明[0, i]段是按照顺序排列的
            if nums[i] > nums[0]:
                # 
                if target >= nums[0]:
                    # 当target大于nums[0]同时小于或等于nums[i]时，说明target在[0 , i]中
                    if target <= nums[i]:
                        nums = nums[:i + 1]
                    # 否则target在[i + 1, -1]中
                    else:
                        nums = nums[i + 1:]
                        res += i + 1
                # 当target小于nums[0]，说明target肯定不在[0, i]中
                else:
                    nums = nums[i + 1:]
                    res += i + 1
            # 说明[i + 1, -1]段是按照顺序排列的
            else:
                # 与第一段的处理方法相同
                if target >= nums[0]:
                    nums = nums[:i + 1]
                else:
                    if target <= nums[i]:
                        nums = nums[:i + 1]
                    else:
                        nums = nums[i + 1:]
                        res += i + 1
		# 当nums长度为2的时候直接判断
        if len(nums) <= 2:
            for i, item in enumerate(nums):
                if target == item:
                    return i + res
        return -1
```

