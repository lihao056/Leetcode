#### Leetcode题库31. 下一个排列

实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须**原地**修改，只允许使用额外常数空间。

以下是一些例子，输入位于左侧列，其相应输出位于右侧列。

```
1,2,3` → `1,3,2`
3,2,1` → `1,2,3`
1,1,5` → `1,5,1
```

`

思路：

​	题目有点难读懂，从网上找的关于题目的解释。

​	什么是一个排列？什么是一个更大的排列？把我搞懵逼啦，搜了一资料，其实排列就是数学中的排列组合，只是参与排列的元素是一个个整数，而这些整数从高位到底为组合起来就是一个多位数的整数，所谓下一个更大的排列即为比这个多位整数大一点数。例如：输入序列为 1,2,3 即组合起来的多位整数为123，而比这个多位整数大一点的整数为132，其元素排列为1,3,2；[参考链接](https://blog.csdn.net/qustdrjhj/article/details/76534933)

​	按照解释，很容易想到对于降序排列，它是最大值，对于升序排列，它是最小值。

​	所以题目就转换为判断排列升降序的问题，通过交换位置，即可完成比原整数大一点的操作

​	以下是程序，使用的是冒泡排序法

第一版:

```python
class Solution(object):
    def nextPermutation(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        if len(nums) == 0:
            return
        for i in range(len(nums) - 1, 0, -1):
            # 由后往前判断是否按照降序排列,都是降序排列,则跳出循环后进行处理
            if nums[i] <= nums[i - 1]:
                continue
            else:
                # 找到第一个不符合降序的数,需要在前面替换为比这个数大一点的数
                temp = None
                pos = 0
                for j in range(i - 1, len(nums)):
                    if nums[i - 1] >= nums[j]:
                        continue
                    else:
                        # 使用pos表示稍微大一点的数的坐标
                        if temp == None:
                            temp = nums[j] - nums[i - 1]
                        if temp > nums[j] - nums[i - 1]:
                            temp = nums[j] - nums[i - 1]
                        pos = j
				# 将两个数替换
                temp = nums[i - 1]
                nums[i - 1] = nums[pos]
                nums[pos] = temp
				# 之后对剩余的数按照升序排序
                for j in range(i, len(nums)):
                    for k in range(len(nums) - 1, j, -1):
                        if nums[k] < nums[k - 1]:
                            temp = nums[k]
                            nums[k] = nums[k - 1]
                            nums[k - 1] = temp
                return nums
		# 若都是降序序，按照升序进行排列
        for j in range(0, len(nums)):
            for k in range(len(nums) - 1, j, -1):
                if nums[k] < nums[k - 1]:
                    temp = nums[k]
                    nums[k] = nums[k - 1]
                    nums[k - 1] = temp
        return nums
```

