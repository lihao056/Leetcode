#### Leetcode题库49. 字母异位词分组(语言应用)

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

示例:

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"],
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```


说明：

```
所有输入均为小写字母。
不考虑答案输出的顺序。
```

思路：

​	看到题目中的重复想到是使用collections库中的Counter函数判断字符串出现个数，并将相同的Counter类看成一个相同的异构词。

​	将求出来的Counter类转换成字典的形式存储在列表中，通过in操作符判断剩余的字符串的Counter类是否包含在这个列表中。

​	在的话添加至确定位置，不在的话添加至新的位置

第一版：

```python
class Solution(object):
    def groupAnagrams(self, strs):
        """
        :type strs: List[str]
        :rtype: List[List[str]]
        """
        dic, pos = [], 0
        result = []
        for item in strs:
            if dict(collections.Counter(item)) not in dic:
                dic.append(dict(collections.Counter(item)))
                result.append([item])
                pos += 1
            else:
                result[dic.index(dict(collections.Counter(item)))].append(item)
        return result
```

第二版：

​	LeetCode官方解答的[版本](https://leetcode-cn.com/problems/two-sum/solution/zi-mu-yi-wei-ci-fen-zu-by-leetcode/)，python中执行用时为 96 ms 的范例

​	使用元组的形式作为字典的键，由于是字符串，可以使用sort的形式将元素进行排列(含有相同字符的字符串的排序肯定是一样的)并转换成元组作为字典的键，将字符串添加至相应的值中。

​	非常简洁明了。

​	关键点：1.使用collection.defaultdict设置字典的默认值，使其能直接添加。

​					2.使用元组作为字典的键。

​					3.使用排序算法将字符串统一。					

```python
class Solution(object):
    def groupAnagrams(self, strs):
        # 使用defaultdict是因为想让键值后
        ans = collections.defaultdict(list)
        for s in strs:
            ans[tuple(sorted(s))].append(s)
        return list(ans.values())
```
