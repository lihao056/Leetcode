#### Leetcode题库07-整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。



**示例 1:**

```
输入: 123
输出: 321
```

 **示例 2:**

```
输入: -123
输出: -321
```

**示例 3:**

```
输入: 120
输出: 21
```

**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

​	这个题目就非常简单，最简单易想的方法是对整数进行整数求余，得到最后一位数字，再对新数进行相加。

​	其他思路还有使用字符串操作，将数组转换成字符串进行操作

```
class Solution:
    def reverse(self, x: int) -> int:
        '''
        :type x : int
        :rtyoe int
        '''
        f_num = 0
        sign = 1
        if x <= 0:
            sign = -1
            x *= sign

        while True:
            temp = x%10
            x = int(x/10)
            f_num = f_num * 10 + temp
            if x == 0:
                break
        f_num *= sign
        if (f_num >= pow(2, 31) - 1) | (f_num <= -pow(2, 31)):
            return 0
        return f_numclass Solution:
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

​	方法2

```
class Solution:
    def reverse(self, x: int) -> int:
        '''
        :type x : int
        :rtyoe int
        '''
        sign = 1
        if x <= 0:
            sign = -1
            x *= sign
        x = str(x)
        f_num = ""
        for i in range(len(x)):
            f_num += x[len(x) - 1 - i]

        f_num = int(f_num) * sign
        if (f_num > 2 ** 31 - 1) | (f_num < -2 ** 31):
            return 0
        return f_num
```

