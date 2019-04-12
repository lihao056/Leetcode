#### Leetcode题库14. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1:**

```
输入: ["flower","flow","flight"]
输出: "fl"
```

**示例 2:**

```
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
```

**说明:**

```
所有输入只包含小写字母 `a-z` 。
```

思路：

​	检查公共前缀，说明每个字符串都有相同的开头，遍历每个即可。

​	考虑使用迭代法进行解题，将列表中每个字符串进行遍历，判断第一个字符是否相等，并同时将字符缩减，不相等的直接返回空即可。只当数组遍历完成后才继续进行下一步的迭代操作。

​	

```
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        final = ""
        if len(strs) == 0:
            return ""
        for i in range(len(strs)):				
            if len(strs[i]) == 0:
                return ""
            if final == "":
                final = strs[0][0]
            if final != strs[i][0]:
                return ""
            strs[i] = strs[i][1:]							
        return final + self.longestCommonPrefix(strs)	# 只有遍历完成后才将加入当前字符
        												# 并进行下一步迭代
```

