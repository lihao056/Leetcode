#### Leetcode题库38. 报数

报数序列是一个整数序列，按照其中的整数的顺序进行报数，得到下一个数。其前五项如下：

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

`1` 被读作  `"one 1"`  (`"一个一"`) , 即 `11`。
`11` 被读作 `"two 1s"` (`"两个一"`）, 即 `21`。
`21` 被读作 `"one 2"`,  "`one 1"` （`"一个二"` ,  `"一个一"`) , 即 `1211`。

给定一个正整数 *n*（1 ≤ *n* ≤ 30），输出报数序列的第 *n* 项。

注意：整数顺序将表示为一个字符串。

 

**示例 1:**

```
输入: 1
输出: "1"
```

**示例 2:**

```
输入: 4
输出: "1211"
```

思路：

​	首先是要读懂这个问题，才能解决问题。

​	参照[链接](https://blog.csdn.net/u010281829/article/details/80105033)中的解释。

	第一项是1，第二项是11，代表1(count)个1(element)，然后第三项是描述第二项，有2(count)个1(element)，第四项描述第三项，有1(count)个2(element)和1(count)个1(element)，到这里可能就觉得这个多简单啊，每个元素搞个计数器，遇到就++，最后输出，等我们看到第五项的时候就发现不是这样的，并没有出现3(count)个1(element)，在2之后做了分割，也就是就算是相同的元素，中间被不一样的元素隔开以后就要重新计数。所以第五项描述为1(count)个1(element)，1(count)个2(element)，2(count)个1(element)。
​	有了count和element这两个解释后可以很方便的了解题目了，就是对于相同元素计算并将count和element转换成字符串形式。

​	对此，考虑使用迭代方法，通过将迭代将n - 1的值返回，在这个基础上进行处理。

第一版：

```python
class Solution(object):
    def countAndSay(self, n):
        """
        :type n: int
        :rtype: str
        """
        res = ""
        # 对n = 1和n = 2的情况特殊考虑
        if n == 1:
            return "1"
        if n == 2:
            return '1' + self.countAndSay(1)
        # 对n > 2，使用迭代方法
        if n > 2:
            # 返回n-1时候的值
            temp = self.countAndSay(n - 1)
            count = 0
            for i in range(len(temp)):
                if i == 0:
                    count += 1
                    continue
                # 当元素相同，则加1
                if temp[i] == temp[i - 1]:
                    count += 1
                # 元素不同，则将当前的值输出res,并重置count
                else:
                    res += str(count) + str(temp[i - 1])
                    count = 1
            res += str(count) + str(temp[-1])
        return res
```

