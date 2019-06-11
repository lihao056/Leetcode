#### Leetcode题库42. 接雨水

给定 *n* 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 **感谢 Marcos** 贡献此图。

**示例:**

```
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6
```

思路：

​	这个可以看成数列中前n项中所装的最大水滴数。(动态规划？)	

​	然后只有数列长度大于3以及数列中存在小于数列两端的数才能接到水。

​	另外，装水是由两端最短的柱子决定的，所以可以通过动态规划更新数值。	

​	写的很乱，等下去找个好一点的版本。

第一版：

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        # 其实长度小于3都要返回0
        if len(height) == 0: return 0
        start = 0
        # 起点开始跳到第一个不为0的数作为起始位置，这个数前面的数字都不能装水
        while start < len(height) and height[start] == 0:
            start += 1
        # 当数列中都为0，说明都不能装水返回0
        if start == len(height): return 0
        # 使用dp[i]表示前i个元素可以装的最大雨滴数
        dp = []
        for i in range(len(height)):
            dp.append(0)
       	# 从start+1开始计算
        for i in range(start + 1, len(height)):
            # 从当前位置M开始往start方向找有没有大过当前位置M元素的值
            for j in range(i - 1, start - 1, -1):
                # 由于装雨滴是由最小的端点决定，所以一开始先找有没有超过当前位置M元素的值
                if height[j] >= height[i]:
                    # 如果有，说明可以当前位置装的雨水只和最先大于这个位置N的数有关。
                    for k in range(j + 1, i):
                        # 装的雨滴数由最短M数减去中间值低于M的值
                        dp[i] += height[i] - height[k]
                    # 再加上由前N个位置的装的最大水滴值，可以得到当前位置M所能装的最大水滴数
                    dp[i] += dp[j]
                    break
            # 如果当前元素M前面没有大于M的柱子，说明装雨滴数M取决于前面最大值
            if j == start and dp[i] == 0:
                # 找到前面最大值
                tmp_max = max(height[start:i])
                # 从当前位置M往前找第一个最大的值的位置N
                for k in range(i - 1, start - 1, -1):
                    if height[k] == tmp_max:
                        break
                # 从这个值的位置N往后加雨滴数
                for l in range(k + 1, i):
                    dp[i] += height[k] - height[l]
                dp[i] += dp[k]
        # 最后返回最后位置的最大雨滴数
        return dp[-1]
```

第二版：

​	摘抄至LeetCode耗时44ms的程序。

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        # 当数组长度小于3则返回0
        h_len = len(height)
        if h_len < 3:
            return 0
        # 建立两个数组存储高度，
        volume = 0
        left_max = [0] * h_len
        right_max = [0] * h_len

        left_max[0] = height[0]
        # 先判断出从左数的最大值
        for i in range(1, h_len):
            left_max[i] = max(height[i], left_max[i - 1])
        # 找出从右数的最高值
        right_max[-1] = height[h_len - 1]
        for i in range(h_len - 2, -1, -1):
            right_max[i] = max(height[i], right_max[i + 1])
        # 位置0是不可能能装水的，所以直接从1开始
        # 另外当前位置如果能装水，肯定取决于当前位置左右最大的元素所以去当前位置的左右最大值
        # 首先我们取左右最大元素的最小值，因为容量取决于小值，再减去当前元素就可以得到这个位置装的容量
        for i in range(1, h_len - 1):
            volume += min(left_max[i], right_max[i]) - height[i]

        return volume
```

