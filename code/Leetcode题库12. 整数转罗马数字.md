#### Leetcode题库12. 整数转罗马数字

- 罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

  ```
  字符          数值
  I             1
  V             5
  X             10
  L             50
  C             100
  D             500
  M             1000
  ```

  例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

  通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

  - `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
  - `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
  - `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

  给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

  **示例 1:**

  ```
  输入: 3
  输出: "III"
  ```

  **示例 2:**

  ```
  输入: 4
  输出: "IV"
  ```

  **示例 3:**

  ```
  输入: 9
  输出: "IX"
  ```

  **示例 4:**

  ```
  输入: 58
  输出: "LVIII"
  解释: L = 50, V = 5, III = 3.
  ```

  **示例 5:**

  ```
  输入: 1994
  输出: "MCMXCIV"
  解释: M = 1000, CM = 900, XC = 90, IV = 4.
  ```

思路：

​	一开始是想着将每一位判断出来，再进行判断即可。后来发现不对，个十百位的代码各个不同，所以这个方法行不通，同时对于4,9这两个数不知道怎么判断。

​	之后发现，罗马数字中，数字1~9，10 ~ 90,100 ~ 900是类似的，可以使用迭代算法进行求解。可以将当前方法进行迭代计算(观摩大神方法之后尝试使用一下)。

​	定义一个全局变量lis存储字符代码，方便迭代使用。

​	由于区间为1-3999，所以对于千位进行特殊判断。

​	终止迭代的判断为当输入为0时即可终止

程序如下

注意：这一个方法没有错误检验

使用迭代算法记得一下几点：

1. 必须能达到终止迭代的条件，否则会崩溃
2. 在类中使用方法记得加上self
3. 可以在类中进行全局变量的设置

```
class Solution(object):
    lis = ['I', 'V', 'X', 'L', 'C', 'D', 'M']		# 设置全局变量
    def intToRoman(self, num):
        """
        :type num: int
        :rtype: str
        """
        temp = ""
        length = len(str(num))
        if num == 0:
            return ""
        j = int(num / 10 ** (length - 1))			# 存储当前num的最高位
        num %= 10 ** (length - 1)					# 将num取余后进行下一次迭代
        if length == 4:								# 对千位判断
            for i in range(j):
                temp += self.lis[6]
            return temp + self.intToRoman(num)
        if j == 9:														# 对其余位判断
            temp = self.lis[2 * (length - 1)] + self.lis[2 * length]
            return temp + self.intToRoman(num)
        elif j == 4:
            temp = self.lis[2 * (length - 1)] + self.lis[2 * length - 1]
            return temp + self.intToRoman(num)
        else:
            if (j / 5):
                temp += self.lis[2 * length - 1]
                j -= 5
            for i in range(j):
                temp += self.lis[2 * (length - 1)]
            return temp + self.intToRoman(num)
```

