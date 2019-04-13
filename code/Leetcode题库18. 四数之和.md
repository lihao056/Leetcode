#### Leetcode题库18. 四数之和

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

思路：

​	这个题目是三数之和的增强版本，由于一开始对于三数和的想法是判断所有可能的情况，所以对于四数和的想法跟当时三数之和一样判断所有的情况。就有了第一版程序。

​	1.第一层循环，判断是否有存在4个相同数字的情况

​	2.第二层循环，判断是否存在有两个数字的情况，两种[1, 3]及一种[2, 2]的情况。

​	3.第三层循环，判断是否存在有三个数字的情况，[1,2,3,1]，[1,2,3,2]，[1,2,3,3]

​	4.判断是否有4个数字的情况，[1,2,3,4]

以下为程序

```python
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        nums_time = {}
        final = list()
        # 同样先进行字典的存储，将数字和次数对应起来，并进行排序，方便判断
        for num in nums:
            nums_time[num] = nums_time.get(num, 0) + 1
        nums = sorted(list(nums_time.keys()))
		# 首先第一层循环，判断是否有[1, 1, 1, 1]
        for pst_0, item_0 in enumerate(nums):
            if item_0 * 4 == target and nums_time[item_0] >= 4:
                final.append([item_0, item_0, item_0, item_0])
            # 第二层循环，判断是否有[1, 1, 1, 2],[1, 2, 2, 2]和[1, 1, 2 ,2]
            for pst_1, item_1 in enumerate(nums[pst_0 + 1:]):
                if item_0 * 3 + item_1 == target and nums_time[item_0] >= 3:
                    final.append([item_0, item_0, item_0, item_1])
                elif item_1 * 3 + item_0 == target and nums_time[item_1] >= 3:
                    final.append([item_0, item_1, item_1, item_1])
                elif item_1 * 2 + item_0 * 2 == target and  nums_time[item_0] >= 2 and nums_time[item_1] >= 2:
                    final.append([item_0, item_1, item_0, item_1])
                # 第三层循环，判断是否有[1, 2, 3, 1],[1, 2, 3, 2]和[1, 2, 3 ,3]
                for item_2 in nums[pst_0 + pst_1 + 2:]:
                    temp = [item_0, item_1, item_2]
                    sum = item_1 + item_2 + item_0
                    for i in temp:
                        if sum + i == target and nums_time[i] >= 2:
                            final.append([item_0, item_1, item_2, i])
                    item_3 = target - sum
                    # 判断是否有[1, 2, 3, 4]情况
                    if item_3 > item_2 and item_3 in nums_time:
                        final.append([item_0, item_1, item_2, item_3])


        return final
        
```

第二版方法：

​	考虑使用通用版的方法，不需要考虑各种情况

```python
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        nums = sorted(nums)
        final = []
        # 第一层循环，选取item_0
        for pst_0, item_0 in enumerate(nums):
            if pst_0 > 0 and item_0 == nums[pst_0 - 1]: continue
            threeSum = target - item_0
			# 变成3数之和
            for pst_1, item_1 in enumerate(nums[pst_0 + 1:]):
                # 去重，注意这个位置
                if pst_1 > 0 and item_1 == nums[pst_0 + pst_1 + 1 - 1]: continue
				# 变成两数之和
                twoSum = threeSum - item_1
                les = pst_0 + pst_1 + 2
                big = len(nums) - 1
                while les < big:
                    if nums[big] + nums[les] < twoSum:  les += 1
                    elif nums[big] + nums[les] > twoSum:  big -= 1
                    else:
                        final.append([item_0, item_1, nums[big], nums[les]])
                        while les < big and nums[les] == nums[les + 1]: les += 1
                        while big > les and nums[big] == nums[big - 1]: big -= 1
                        les += 1
                        big -= 1
        return final
        
```

