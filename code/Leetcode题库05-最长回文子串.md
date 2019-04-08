#### Leetcode题库05-最长回文子串

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 1000。

**示例 1：**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例 2：**

```
输入: "cbbd"
输出: "bb"
```

​	这个一开始我是一点头绪都没有的，完全是参照网络上的方法进行撰写的

​	https://blog.csdn.net/qq_32354501/article/details/80084325

​	其实解决这道题最久的地方是出现了这个错误：

![1553673013612](G:\typora图片\1553673013612.png)

​	这个输入不是“”，而是空，然后在调试的时候注意return返回的数值，这个是新手容易犯的操作，我耗了好久。

暴力解法，不通过是时间太长了，如果解除时间限制不知道代码有没有问题

​	思路：首先是从大到小进行会回文的判断，这样子直接可以输出当前字符串中最大的字符。

​	回文判断的依据是遍历字符串，选取最后和最前的字符判断，若都为相等的，则是一个回文字符串。

```
class Solution:
    def longestPalindrome(self, s):
        l = len(s)
        if l <= 1:
            return s
        for i in range(l):
            temp_len = l - i - 1
            for j in range(l -temp_len):
                sub_s = s[j:(j+temp_len +1)]
                count = 0
                for k in range(int(len(sub_s)/2)):
                    if sub_s[k] == sub_s[len(sub_s) - k - 1]:
                        count += 1
                    if count == int(len(sub_s)/2):
                        return sub_s
        return s[0]
```

这个是参照网络上的方法进行求解

将字符串每个字符进行遍历，对每个字符可能的回文情况进行判断，并将最长的回文字符串的位置记录下来。

```
class Solution:
    def longestPalindrome(self, s):
        '''
        :type s:str
        :rtype str
        '''
        l = len(s)
        long = 0
        best_low = best_high = 0
        if l <= 1:
            return s
        for i in range(l):				# 这里分成单核和双核两种情况讨论
            low = high = i
            while (low >= 0) & (high < len(s)):
                if s[low] == s[high]:
                    if best_high - best_low < high - low:
                        best_low = low
                        best_high = high
                    low -= 1
                    high += 1
                else:
                    break
            low,high = i, i+1
            while (low >= 0) & (high < len(s)):
                if s[low] == s[high]:
                    if best_high - best_low < high - low:
                        best_low = low
                        best_high = high
                    low -= 1
                    high += 1
                else:
                    break
        return s[best_low : best_high+1]		# 注意使用[a:b]的时候，表示的是从位置a到位置b-1的字符串信息
```

