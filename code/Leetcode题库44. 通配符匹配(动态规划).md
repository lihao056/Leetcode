#### Leetcode题库44. 通配符匹配(动态规划)

给定一个字符串 (`s`) 和一个字符模式 (`p`) ，实现一个支持 `'?'` 和 `'*'` 的通配符匹配。

```
'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
```

两个字符串**完全匹配**才算匹配成功。

**说明:**

- `s` 可能为空，且只包含从 `a-z` 的小写字母。
- `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `?` 和 `*`。

**示例 1:**

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```

**示例 2:**

```
输入:
s = "aa"
p = "*"
输出: true
解释: '*' 可以匹配任意字符串。
```

**示例 3:**

```
输入:
s = "cb"
p = "?a"
输出: false
解释: '?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
```

**示例 4:**

```
输入:
s = "adceb"
p = "*a*b"
输出: true
解释: 第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
```

**示例 5:**

```
输入:
s = "acdcb"
p = "a*c?b"
输入: false
```

思路：

​	这个和[10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)比较相似，其中正则化匹配的话使用的是递归的方法，将其迁移到这里会出现超时的错误，所以在网络上找了动态规划的方法进行解决。


第一版：

​	出现超时的程序，具体不知道正不正确，

```python
class Solution(object):
    def multiply(self, num1, num2):
        """
        :type num1: str
        :type num2: str
        :rtype: str
        """
        # 如果出现0，则返回0
        if num1 == "0" or num2 == "0":
            return "0"
        lis = []
      	# lis用于保存每个位相乘得到的结果
        for i in range(len(num2)):
            lis.append([])
        # 因为字符串的位是从后往前的，所以也要从后往前取位置
        for i in range(len(num2) - 1, -1, -1):
            # 这里可以用字符相减获得当前值，
            tem_2 = int(num2[i])
            bit = 0
            # 这里也是从后往前进位
            for j in range(len(num1) - 1, -1, -1):
                tem_1 = int(num1[j])
                tem = tem_1 * tem_2
                tem += bit
                # 这里使用bit纪录进位，并将进位记录到下一个位置中
                bit = int(tem/10)
                tem = tem % 10
                lis[i].append(tem)
            # 加入进位
            if bit != 0:
                lis[i].append(bit)
        # 列表中每一个元素是一个列表，每个列表中为元素。
        lis = lis[::-1]
        # i表示倒数第i位
        i = 1
        # 还是要设置进位
        bit = 0
        res = []
        # 只有当最后一位不为空的才停止循环
        while lis[-1] != []:
            tem_1 = 0 + bit
            for j in range(i):
                # 为空说明j位置的数值不包含当前位的信息
                if lis[j] == []:
                    continue
                # 使用pop方法提取所属位置的数，并相加
                tem = lis[j].pop(0)
                tem_1 += tem
            bit = int(tem_1 / 10)
            res.append(tem_1%10)
            # i必须<=列表长度
            i = min(len(lis), i + 1)
        # 保证最后的进位信息不丢失
        if bit != 0:
            res.append(bit)
        result = ""
        for i in range(len(res) - 1, -1, -1):
            result += str(res[i])
        return result
        
```

第二版：

​	参照网络上的写法，使用动态规划进行解题。

```python
class Solution:
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        # dp表，很常规，以p字符串作为列，以s作为行,dp[a][b]表示在p字符串中前n个字符是否能匹配s字符串中的前b个字符。
        dp = [[False for i in range(len(p) + 1)] for j in range(len(s) + 1)]
        # 双方都是空字符串，当然是true
        dp[0][0] = True

        for i in range(1, len(p) + 1):
            if p[i - 1] == '*':
                dp[0][i] = dp[0][i - 1]
                # 探讨的是当s为空字符串时，p的前j项是否匹配
                # 如果p的这一次对应的项是*,则看它前一项是否匹配，如果是其他，抱歉，还是false
        # 我们没有去填s不为空，p为空的情况，因为这样会刚好符合默认，也就是全为false。

        # 开始填dp表
        # 把字符串想象成未完成的状态，因为动态规划本身其实是不断在建设字符串的，所以p[j-1]为p的最后一项，因为后面的还未建设
        for i in range(1, len(s) + 1):
            for j in range(1, len(p) + 1):
                # 如果我们最后一项为*
                if p[j - 1] == '*':
                    # 第一，有可能它代表空字符串
                    # 第二，有可能它就代表一个字符串，也就是对应的那个
                    # 第三，有可能它代表多个字符
                    dp[i][j] = dp[i][j - 1] or dp[i - 1][j - 1] or dp[i - 1][j]
                else:
                    # 如果我们这一项为其他的，有可能是字母或'?'
                    # 必须要求各后退一项也能匹配，并且在这号位上也能匹配
                    dp[i][j] = (s[i - 1] == p[j - 1] or p[j - 1] == '?') and dp[i - 1][j - 1]
        # 返回前len(s)的s 与 前len(p)的匹对结果
        return dp[len(s)][len(p)]
```







