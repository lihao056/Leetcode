#### Leetcode题库40. 组合总和 II

给定一个数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用一次。

**说明：**

- 所有数字（包括目标数）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
所求解集为:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```

**示例 2:**

```
输入: candidates = [2,5,2,1,2], target = 5,
所求解集为:
[
  [1,2,2],
  [5]
]
```

思路：

​	这道题目和[组合总和](https://leetcode-cn.com/problems/combination-sum)很相似。可以使用它的方法。

​	它的区别在于每个数字只能使用一次，并且数组中是可以有重复的数字。

​	所以需要纪录位置信息。

第一版：

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        # 在类内定义希望可以减少时间，但是问题不在这里
        def combinationSum_test(candidates, target):
            # 当迭代到空的时候返回空
            if candidates == []:
                return []
            result = []
            min_num = min(candidates)
            # 利用enumerate获取位置信息
            for i, item in enumerate(candidates):
                if target == item:
                    result.append([item])
                else:
                    if target - item < min_num:
                        continue
                    temp = candidates[:]
                    # 删除当前位置的数字，保证使用一次
                    temp.pop(i)
                    for item_1 in combinationSum_test(temp, target - item):
                        item_1.append(item)
                        result.append(item_1)
            return result
		# 去重复操作
        result = combinationSum_test(candidates, target)
        result_final = []
        for item in result:
            tmp = collections.Counter(item)
            if tmp not in result_final:
                result_final.append(tmp)
        result = []
        for item in result_final:
            if list(item.elements()) not in result:
                result.append(list(item.elements()))
        return result
```

第二版

​	摘抄至LeetCode上耗时48ms的例程。

​	与39最快的例程很像。

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        # 采取排序及后续处理减少重复次数
        candidates.sort()
        tmp,res=[],[]
        self.dfs(candidates,0,tmp,res,target)
        return res
    
    def dfs(self,candidates,start,tmp,res,target):
        if target==0:
            # 存储合适的数组信息
            res.append(tmp[:])
            return
        for i in range(start,len(candidates)):
            # 因为是排序数组，第一个不可能后面都不可能
            if candidates[i]>target:return
            # i > start表明至少已经完成了1次dfs，后一个是跳去重复的数值
            if i>start and candidates[i]==candidates[i-1]:
                continue
            tmp.append(candidates[i])
            self.dfs(candidates,i+1,tmp,res,target-candidates[i])
            tmp.pop()
        return
```

