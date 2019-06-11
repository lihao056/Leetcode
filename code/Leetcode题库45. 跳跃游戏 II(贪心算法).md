#### Leetcode题库45. 跳跃游戏 II(贪心算法)

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

你的目标是使用最少的跳跃次数到达数组的最后一个位置。

**示例:**

```
输入: [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**说明:**

假设你总是可以到达数组的最后一个位置。

思路：

​	一开始想到使用动态规划的算法做，通过遍历之前位置是否可以跳到当前位置上的方式，在dp数组的基础上加1表示当前位置所能达到的最小值。但是还是太慢了。


第一版：

​	出现超时的程序，具体不知道正不正确，

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) == 0:  return 0
        dp = []
        for i in range(len(nums)):
            dp.append(0)
        # dp表示能达到当前位置最少的步数
        for i in range(1, len(nums)):
            min_test = []
            # 对所有可能的情况进行搜寻
            for j in range(i):
                if (j + nums[j]) >= i:
                    min_test.append(dp[j] + 1)
            # 返回最少的步数
            dp[i] = min(min_test)

        return dp[-1]
        
```

第二版：

​	参照网络上的写法，使用贪心算法进行解题，因为题目给出解释，总是能达到最后一个位置，所以只要考虑当前位置中能达到的位置里下一次再跳可以到达的最远的位置即可。

```python
class Solution(object):
    def jump(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        p = 0
        max = 0
        step = 0
        index = 0
        # 排除只有0的情况
        if len(nums) == 1:  return 0
        while p < len(nums):
            # 如果当前位置和相加大于最大长度，则返回
            if p + nums[p] >= len(nums) - 1:
                return step + 1
            for i in range(p + 1, p + nums[p] + 1):
                if max < (nums[i] + i):
                    max = nums[i] + i;
                    index = i
            step += 1
            p = index
        return step
```







