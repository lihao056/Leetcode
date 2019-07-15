#### Leetcode题库50. Pow(x, n)(快速幂算法)

实现 pow(x, n) ，即计算 x 的 n 次幂函数。

示例 1:

```
输入: 2.00000, 10
输出: 1024.00000
```


示例 2:

```
输入: 2.10000, 3
输出: 9.26100
```


示例 3:

```
输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
```


说明:

```
-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。
```

思路：

​	一开始想着是暴力法解问题，但是肯定会超时的，所以写了一版暴力法的版本去测试，果然是超时的，所以开始去找有没有什么简化的算法，最终在网络上找到快速幂算法用在本题上。可以参照[博客]([https://blog.csdn.net/qq_19782019/article/details/85621386#%E4%BB%80%E4%B9%88%E6%98%AF%E5%BF%AB%E9%80%9F%E5%B9%82%E7%AE%97%E6%B3%95](https://blog.csdn.net/qq_19782019/article/details/85621386#什么是快速幂算法))

​	通过增加底数减少幂次的方式来降低循环次数，最终达到减少时间的效果。

第一版：

```python
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        result = float(1.0)
        # 特殊情况处理
        if n == 0: return 1
        elif n < 0:
            x = 1/x
            n = -n
        res = x
        # 当幂为奇数时，可以变成(x*x)**(n/2)*x**1的形式，通过不断循环，保证值运行时间变短。
        while n != 1:
            if n % 2:
                result *= res
            n = int(n/2)
            res *= res
        result *= res
        return result
```

​				

