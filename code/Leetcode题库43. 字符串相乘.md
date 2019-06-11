#### Leetcode题库43. 字符串相乘

给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

**示例 1:**

```
输入: num1 = "2", num2 = "3"
输出: "6"
```

**示例 2:**

```
输入: num1 = "123", num2 = "456"
输出: "56088"
```

**说明：**

1. `num1` 和 `num2` 的长度小于110。
2. `num1` 和 `num2` 只包含数字 `0-9`。
3. `num1` 和 `num2` 均不以零开头，除非是数字 0 本身。
4. **不能使用任何标准库的大数类型（比如 BigInteger）**或**直接将输入转换为整数来处理**。

思路：

​	首先不能使用转换为整数处理，考虑将字符串做一位一位处理，这样子不违背题目的要求。

​	这样子需要考虑近进位的情况。

第一版：

​	耗时较长，LeetCode上最短的版本都是直接转换类型的，所以不理。

​	取一个数num1的一位M，与另一个数num2的每一位相乘，通过进位获得M与num相乘的值。将num1的每一位相乘后的结果相加得到最后的结果。

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

