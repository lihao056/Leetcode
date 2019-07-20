#### Leetcode题库55. 跳跃游戏(贪心算法)

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

示例 1:

```
输入: [2,3,1,1,4]
输出: true
解释: 从位置 0 到 1 跳 1 步, 然后跳 3 步到达最后一个位置。
```


示例 2:

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

思路：

​	一开始想用DFS加回溯算法实现，但是肯定是超时的，所以需要换一种方法。

​	由于这个和45题跳跃游戏2很相似，再加上看了其他人的思路，最终确定使用贪心算法来实现功能。

第一版：

​	时间耗时长，同时需要考虑很多边界情况，提交了多次。

​	贪心的原则：我可以选择的下一步中要使我的下下一步的距离要最远，这是考虑了两步的情况。

​	当最远的情况都不能到终点，说明肯定到不了

```python
class Solution(object):
    def canJump(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        max = 0
        pos = 0
        old_pos = 0
        index = 0
        while True:
            # 如果max大于数组长度，说明可以达到
            if max >= len(nums) - 1:
                return True
            # 规避可能出现i超出数组长度的情况
            for i in range(pos + 1, pos + nums[pos] + 1):
                if i < len(nums) and max < nums[i] + i:
                    max = nums[i] + i
                    index = i
            # 目前只能通过判断index与上次不变后才能确定达不到
            if index == pos:
                return False
            pos = index
```

第二版：

​	这是Leetcode中耗时72ms的代码，其中它是使用反遍历的方法由后往前遍历，当当前位置可以达到目的点时，k则重新从1开始计数，目的点变成当前位置，若不能达到目的地，则k+1，往下一个位置判断，如果最后K 不为1，说明无法到达。

```python
class Solution(object):
    def canJump(self, nums):
        k = 1
        for i in range(len(nums)-2, -1, -1):
            k = k + 1 if nums[i] < k else 1
        return k == 1
```







