#### Leetcode题库16. 最接近的三数之和

给定一个包括 *n* 个整数的数组 `nums` 和 一个目标值 `target`。找出 `nums` 中的三个整数，使得它们的和与 `target` 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

```
例如，给定数组 nums = [-1，2，1，-4], 和 target = 1.

与 target 最接近的三个数的和为 2. (-1 + 2 + 1 = 2).
```



思路：

​	题目提示，只存在唯一一个答案，所以可以直接通过判断三数之和是否接近target即可。

​	也是先对数组进行排序后进行处理，在第一层循环选定一个值之后，设定两个首尾指针用于表示3数之和。

​	没遍历一个新数字，先判断是否距离是否大于最近距离，若大于最近距离，则替换，之后判断三数之和与target的关系，并移动指针。

​	本题的关键是将最接近的三数之和和之前的三数之和题目联系起来。

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        final = None		# 用来存放和，由于不能有数值，用Null代替
        nums = sorted(nums)
        for pst_0, item_0 in enumerate(nums):
            less = pst_0 + 1
            big = len(nums) - 1
            while big > less:
                temp = item_0 + nums[less] + nums[big]
                
                if final == None:
                    final = temp
                else:
                    if abs(final - target) > abs(temp - target):
                        final = temp
                # 若三数之和小，则首指针移动
                if temp - target < 0:
                    less += 1
                elif temp - target > 0:
                    big -= 1
                # 若等于0了，直接返回即可
                else:
                    return target

        return final
```

