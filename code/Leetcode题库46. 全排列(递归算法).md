#### Leetcode题库46. 全排列(递归算法)

给定一个没有重复数字的序列，返回其所有可能的全排列。

示例:

```
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

思路：

​	首先，题目限定没有重复数字的数组，那我只要考虑数组的位数就好。由于3个数字的排列可以由两个数字加剩余的字进行组合排列得到，所以可以直接使用递归的方式进行解答，通过不断增加递归的长度达到要求的排列


第一版：

​	出现超时的程序，具体不知道正不正确，

```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        result = []
        # 如果等于1，只有一种排列
        if len(nums) == 1:
            return [nums]
        if len(nums) >= 2:
            # 对nums里所有的数字进行一次遍历
            for item in nums:
                temp = nums[:]
                temp.remove(item)
                # 抽取出一个数字后将其放入递归算法中
                b = self.permute(temp)
                for item_1 in b:
                    # b为抽取数字后所有不重复的排列，对每个排列中加入抽取后的数字，即可成为新的排列，并加入到result中
                    item_1.insert(0, item)
                    result.append(item_1)
			# 最后返回的result即为所有排列的数组
            return result
```
