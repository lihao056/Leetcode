#### Leetcode题库53. 最大子序和(动态规划)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:

如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

思路：

​	一开始的思路是使用双指针由头尾进行遍历的方式获取最大子序列和，但是最后这个方法无法判断一种情况，导致前期工作白费。

​	这道题其实对熟悉动态规划的人应该很容易想到。

​	求最大连续子序列和和动态规划肯定是有关系的，只要找到递进公式即可。

第一版：

​	参照[博客](<https://blog.csdn.net/LetJava/article/details/95500969>)思路

​	思路：当到达一个位置时，如果此时的子序列之和小于 0 的话，那么从当前位置开始的新子序列一定比保留原来的子序列的和更大。

​	递推公式：dp[i] = Math.max(dp[i - 1] + nums[i], nums[i])

​	下面代码是Leetcode评论的代码，我转过来的。

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == None or len(nums) == 0:
            return 0

        maxsum = nums[0]
        tempsum = 0
        for num in nums:
            tempsum += num
            # 如果加入num的和小于num，说明之前的子序列和是负数
            if tempsum < num:
                tempsum = num
            # 判断大小
            if maxsum < tempsum:
                maxsum = tempsum
        return maxsum
```

第二版：

​	参照博客的Java版本修改的Python版本，使用分治法。

```python
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return self.find(nums, 0, len(nums) - 1)

    def find(self, nums, start, end):
        if start == end:
            return nums[start]
        if start > end:
            # 输出最小的整数
            return -sys.maxsize

        mid = int(start + (end - start) / 2)
        leftMax = self.find(nums, start, mid - 1)
        rightMax = self.find(nums, mid + 1, end)
		
        # 求将中间加上呃连续子序列和
        maxl = 0
        sum = 0
        for i in range(mid - 1, start - 1, -1):
            sum += nums[i]
            maxl = max(maxl, sum)

        maxr = 0
        sum = 0
        for i in range(mid + 1, end + 1, 1):
            sum += nums[i]
            maxr = max(maxr, sum)

        return max(leftMax, rightMax, maxl + maxr + nums[mid])
```



​				

