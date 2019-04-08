#### Leetcode题库09. 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true
```

**示例 2:**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3:**

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

思路：

​	这题比较简单，直接按照题目的要求，将整型数组转换成字符串后，遍历一遍字符串，将位置i和位置len(S) - i -1的字符进行比较，若为相同则继续，若不相同直接返回False。遍历完成后如果都相同，则输出True。

程序如下

```
class Solution:
    def isPalindrome(self, x: int) -> bool:
    	'''
    	:type x : int
    	:rtype bool
    	'''
        x = str(x)
        i = 0
        while( i < len(x)):
            if x[i] == x[len(x) - 1 -i]:
                i += 1
                continue
            else:
                return False
        return True
```

