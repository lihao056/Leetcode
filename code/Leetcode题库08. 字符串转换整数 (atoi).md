#### Leetcode题库08. 字符串转换整数 (atoi)

请你来实现一个 `atoi` 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

**说明：**

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，qing返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

**示例 1:**

```
输入: "42"
输出: 42
```

**示例 2:**

```
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
```

**示例 3:**

```
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
```

**示例 4:**

```
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
```

**示例 5:**

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
```

题目很长，其实也就有以下的注意事项。

​	1.剔除有效字符前的空格

​	2.判断+，-号及将有效的整数进行转换

​	3.判断在进行字符串时如何截止

​	4.判断是否会存在超出范围的情况

所以思路也是按照这样子进行判断

​	1.若在没有符号及数字的情况下，将空格字符略过

​	2.在首次遇到+，-号，并且无数字的情况下，将符号添加至输出字符串中，并设置标识符

​	3.首先根据题目要求的特殊性，只要是有效的数字，都是连续的，中间不会有非数字的字符，基于这一点，当遇到数字字符时，就直接添加即可，并将标识符置1

​	4.遇到其他字符直接跳出循环，无法转换为有效数字

​	5.跳出循环后，判断数字标识符，无直接输出0，若有的话继续判断有无超出阈值。（Python比较方便的可以将str转int，方便计算）

​	程序如下

```
class Solution:
    def myAtoi(self, str: str) -> int:
    	'''
    	:type str: str
    	:rtype int
    	'''
        final = ""
        sym_sign = 0
        num_sign = 0

        for i in str:
            if (i == " ") & (sym_sign == 0) & (num_sign == 0):
                continue
            elif ((i == "-")|(i == "+"))  & (sym_sign == 0) & (num_sign == 0):
                sym_sign = 1
                final += i
            elif ((i >= '0') & (i <= '9')):
                final += i
                num_sign = 1
            else:
                break
        if num_sign == 0:
            return 0

        if (int(final)< -2 ** 31):
            return -2 ** 31
        elif (int(final)> 2 ** 31 - 1):
            return 2 ** 31 - 1
        return int(final)
        
```

