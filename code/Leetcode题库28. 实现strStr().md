#### Leetcode题库28. 实现strStr()

实现 [strStr()](https://baike.baidu.com/item/strstr/811469) 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  **-1**。

**示例 1:**

```
输入: haystack = "hello", needle = "ll"
输出: 2
```

**示例 2:**

```
输入: haystack = "aaaaa", needle = "bba"
输出: -1
```

**说明:**

当 `needle` 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 `needle` 是空字符串时我们应当返回 0 。这与C语言的 [strstr()](https://baike.baidu.com/item/strstr/811469) 以及 Java的 [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)) 定义相符。

思路：	

​	按照题目要求，只需要求出haystack和needle中相同的字符串即可。

​	1.首先对一些特殊情况进行处理，如needle = ""的情况。

​	2.在haystack字符串中搜寻第一个与needle相同的字符，之后再进行对比

第一版：

```python
class Solution(object):
    def strStr(self, haystack, needle):
        """
        :type haystack: str
        :type needle: str
        :rtype: int
        """
        if needle == "":
            return 0
        for i in range(len(haystack)):
            if haystack[i] == needle[0]:
                for j in range(len(needle)):
                    # 对应于剩余数组haystack不足needle长度
                    if i + j == len(haystack):
                        return -1
                    # 若想等，则继续判断之后的顺序，若都相等则返回i，若不行则跳出该循环。
                    if haystack[i + j] == needle[j]:
                        if j == len(needle) - 1:
                               return i
                        continue
                    else:
                        break
        return -1
```
