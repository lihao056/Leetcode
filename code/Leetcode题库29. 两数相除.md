#### Leetcode题库29. 两数相除  

给定两个整数，被除数 `dividend` 和除数 `divisor`。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 `dividend` 除以除数 `divisor` 得到的商。

**示例 1:**

```
输入: dividend = 10, divisor = 3
输出: 3
```

**示例 2:**

```
输入: dividend = 7, divisor = -3
输出: -2
```

**说明:**

- 被除数和除数均为 32 位有符号整数。
- 除数不为 0。
- 假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−2^31,  2^31 − 1]。本题中，如果除法结果溢出，则返回 231 − 1。

思路：

​	题目要求不能出现乘法和除法运算，所以一开始是想到使用减法运算替代除法进行求解，但是缺点是时间太长了，提交不成功。

​	之后参照了网络上的方法，发现可以使用位运算的方法进行加减乘除的运算。这种方式快捷，也是计算机极性运算中使用的方法，有想了解的可以参照这篇[博文](https://www.jianshu.com/p/7bba031b11e7)。

第一版：

```python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
		# 判断特殊情况
        if divisor == 0:    return -1
        if dividend == 0:   return 0            
        sign1 = -1 if max(0, dividend) == 0 else 1
        sign2 = -1 if max(0, divisor) == 0 else 1
        # 使用max方法判断符号位
        sign = 1 if abs(sign1 + sign2) else -1
        dividend = abs(dividend)
        divisor = abs(divisor)
        i = 0
        # 使用减法进行判断
        while dividend - divisor >= 0:
            i += 1
            dividend -= divisor
        i = i if sign == 1 else -i
        # 判断超出范围的情况
        if  i > 2 ** 31 - 1:
            return 2 ** 31 - 1
        if  i < -2 ** 31:
            return -2 ** 31
        return i
```

第二版：

​	使用位运算的思想做除法，由于由于为了加速运算，所以使用移位操作，以2的倍数为进行判断。通过向左向右移位可以当成乘以2或者除以2。

​	整个方法的思路如下，通过异或方式判断符号位，因为如果符号位为一正一副的话，异或的结果永远是负数。

```python
class Solution(object):
    def divide(self, dividend, divisor):
        """
        :type dividend: int
        :type divisor: int
        :rtype: int
        """
        if dividend == 0:
            return 0
        # 判断符号
        sign = -1 if dividend ^ divisor < 0 else 1
        dividend = abs(dividend)
        divisor = abs(divisor)
        result = 0
        # 这个是range的操作，从31开始，至-1结束(不包括-1)，每次减1
        for i in range(31, -1, -1):
            # 若分子除以2的i次方后还能大于分母，则再结果加入2的i次方，并减去分母乘以2的倍数
            if dividend >> i >= divisor:
                result += 1 << i
                dividend -= divisor << i
        if sign == -1:
            result = -result
        if result > 2**31 - 1:
            return  2**31 - 1
        if result < -2**31:
            return -2**31
        return result
```

