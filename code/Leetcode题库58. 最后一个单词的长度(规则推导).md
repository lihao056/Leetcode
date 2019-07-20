#### Leetcode题库58. 最后一个单词的长度(规则推导)

给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。

如果不存在最后一个单词，请返回 0 。

说明：一个单词是指由字母组成，但不包含任何空格的字符串。

示例:

```
输入: "Hello World"
输出: 5
```

思路：

​	题目比较简单，没有什么设置难度的地方，当字符串为空或者都为空格的时候才会出现不存在最后一个单词的情况，只要字符串中含有一个字母，它都是最后一个单词。

​	所以直接倒序遍历，遇到空格字符则跳过。当遇到字符后，再次遇到空格则停止。

第一版：

​	这个版本没有使用.split方法，使用了这些方法更加简单。

```python
class Solution(object):
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) == 0:
            return 0
        times = 0
        # 使用倒序的方法，其实可以直接使用python操作s[::-1]将数组倒序
        for i in range(len(s) - 1, -1, -1):
            if s[i] == " " and times == 0:
                continue
            elif s[i] == " " and times != 0:
                return times
            else:
                times += 1
        return times
```

第二版：

​	Leetcode耗时最短版本，使用python内置方法解决，速度快。

```python
class Solution(object):
    def lengthOfLastWord(self, s):
        """
        :type s: str
        :rtype: int
        """
        count = 0
        #rstrip方法消除字符串末尾的空格，isalpha函数判断是否是字母组成
        for i in s.rstrip()[::-1]:
            if i.isalpha():
                count += 1
            else:
                return count
        return count
```

