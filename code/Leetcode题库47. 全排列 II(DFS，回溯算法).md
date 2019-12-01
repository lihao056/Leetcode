#### Leetcode题库47. 全排列 II(DFS，回溯算法)

给定一个可包含重复数字的序列，返回所有不重复的全排列。

示例:

```
输入: [1,1,2]
输出:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```

思路：

​	这个完全没有想法，因为考虑的太复杂了，所以在网上找的版本。

​	参照如下博客的[思路](https://leetcode-cn.com/problems/two-sum/solution/hui-su-suan-fa-by-powcai-3/)说明

```java
if i in visited or (i > 0 and i - 1 not in visited and nums[i-1] == nums[i]):
```

​	首先，这个的判断是回溯算法的关键。

​	1.使用visited作为存储数组位置的变量，如果当前位i已经出现在visited中，则跳过当前位判断下一位。

​	2.or后的条件判断是判断连续两个位置数值相同的情况，因为首先对数组进行过排序，所以相同数字肯定是连续的。另由于我们是从首开始遍历的，当当前位置i和之前位置i-1的数值相同的话，说明这种情况我们已经考虑过了，可以直接跳过这种情况。

​	通过这个判断就可以去重。

第一版：

​	大神的版本，转载过来写一些注解。

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        # 首先进行数组排列，方便去重，另定义全局变量方便回溯的时候判断
        nums.sort()
        res = []
        visited = set()
        # 定义回溯算法
        def backtrack(nums, tmp):
            # 当tmp即遍历的情况长度与它相同时，说明判断完成，添加至res中
            if len(nums) == len(tmp):
                res.append(tmp)
                return
            # 对nums进行遍历，判断这个元素是否存在重复的现象，无则进行下一步判断，有则不进行下一步操作
            for i in range(len(nums)):
                if i in visited or (i > 0 and i - 1 not in visited and nums[i-1] == nums[i]):
                    continue
                visited.add(i)
                backtrack(nums, tmp + [nums[i]])
                visited.remove(i)
        backtrack(nums, [])
        return res
```
