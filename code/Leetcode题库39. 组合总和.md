#### Leetcode题库39. 组合总和

给定一个**无重复元素**的数组 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的数字可以无限制重复被选取。

**说明：**

- 所有数字（包括 `target`）都是正整数。
- 解集不能包含重复的组合。 

**示例 1:**

```
输入: candidates = [2,3,6,7], target = 7,
所求解集为:
[
  [7],
  [2,2,3]
]
```

**示例 2:**

```
输入: candidates = [2,3,5], target = 8,
所求解集为:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```

思路：

​	题目要求求出数字之和，且数字可以**无限制重复使用**。由于受到[解数独](https://github.com/lihao056/Leetcode/blob/master/code/Leetcode%E9%A2%98%E5%BA%9337.%20%E8%A7%A3%E6%95%B0%E7%8B%AC.md)的启发，考虑能不能使用深度优先搜索来做这个题目。即若target - 选择数之差大于0，则继续迭代。

​	在这里会出现重复的现象，我的方法是在最后的时候去重，其实这样子很浪费时间，希望能看看其他人的代码学习一下。

第一版：

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        # 求出每个candidates的最小值
        result = []
        min_num = min(candidates)
        for item in candidates:
            # 如果target == 数组里的数，则直接添加进result即可
            if target == item:
                result.append([item])
            else:
                # 若target - item小于数组最小的数字，说明该item不可能，跳过 
                if target - item < min_num:
                    continue
				# 若不小于，则可以进行下一步迭代，将相减值进行迭代，由于返回的是列表，使用。append()方法即可
                for item_1 in self.combinationSum(candidates, target - item):
                    item_1.append(item)
                    result.append(item_1)
        # 最后使用collection,Counter进行重复值的删除
        result_final = []
        for item in result:
            tmp = collections.Counter(item)
            if tmp not in result_final:
                result_final.append(tmp)
        # result_final中都是不重复的Counter方法的返回值，将其拆开并重新组成数组即可。
        result = []
        for item in result_final:
            if list(item.elements()) not in result:
                result.append(list(item.elements()))
        return result
```

