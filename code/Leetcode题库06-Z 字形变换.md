#### Leetcode题库06-Z 字形变换

将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 `"LEETCODEISHIRING"` 行数为 3 时，排列如下：

```
L   C   I   R
E T O E S I I G
E   D   H   N
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如：`"LCIRETOESIIGEDHN"`。

请你实现这个将字符串进行指定行数变换的函数：

```
string convert(string s, int numRows);
```

**示例 1：**

```
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
```

**示例 2：**

```
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
        L     D     R
        E   O E   I I
        E C   I H   N
        T     S     G
```

​	这种题目，一开始的思路可以从找规律摸清楚，第一行及最后一行字符的位置相隔2*(numRows - 1) ，中间的需要另外处理，这是一开始的思路。

​	后来我发现这个思路很有用，但是有一个很关键的点，怎么确定几个数为一个循环，并且能保证我们编写的方便。

​	一开始我是以一个Z为循环，发现并不行，反而更难去找到规律。

​	最后是在网上看到以下面的示例为一个循环，这样子可以很方便的进行处理。

```
	L     
	E   O 
	E C   
	T    
```

​	对于第一行和最后一行，一个循环共出现一个字符，每个字符与下一个循环的字符位置相差2*(numRows - 1) 

​	对于中间行，在一次循环中出现两次，第一次出现的字符与第二次出现字符之间差2*(numRows - 1 -j)，第二个字符与下一个循环中第一个出现的字符相差 2 * j。这样子就可以将代码编写出来 

```
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        '''
        :type s : str
        :type numRows : int
        :rtype str
        '''
        l = len(s)
        cop = int(l /numRows)						# 纪录需要循环的次数
        if (cop == 0) | (numRows == 1):				# 特例情况的处理，只有一行或者没有铺满第一列的情况
            return s
        new_s = ""
        for j in range(numRows):					# 对行数进行分别处理
            temp = j
            if (j == 0) | (j == numRows - 1):
                for k in range(cop + 1):			# 注意，当出现多余的数组不足以达到一个循环时需要对
                    if temp < l:				    # 循环次数加1，并以长度作为错误检测，排除错误
                        new_s += s[temp]
                        temp += 2 *(numRows - 1)
            else:									# 这个是对中间行的处理
                for k in range(cop + 1):
                    if temp < l:
                        new_s += s[temp]
                        temp += 2 *(numRows - 1 - j)
                        if temp < l:
                            new_s += s[temp]
                            temp += 2 * j
        return new_s
```

