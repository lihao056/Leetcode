#### Leetcode题库17. 电话号码的字母组合

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![1555048271099](https://github.com/lihao056/Leetcode/blob/master/code/picture/1555048271099.png)

**示例:**

```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

**说明:**
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。



思路：

​	首先题目要求返回所有能表达的组合，可以通过分解的思路进行求解，将每个数字可能的组合表示出来，在进行组合，由于一步的操作是相同的，所以尝试考虑使用迭代法进行求解。

​	1.当输入长度为0，则返回空。

​	2.当输入长度为1，则通过list.append()方法，将元素添加至list。

​	3.当输出长度大于1，就需要迭代操作，首先我们提取出输入第一个数字，并将抽取后的输入放入迭代中继续迭代，直至长度等于1为止。

以下为程序

```python
class Solution(object):
    # 设定一个全局变量，方便迭代的时候读取数据
    dic = {
        '2':"abc", '3':"def", '4':"ghi", '5':'jkl', '6':"mno", '7':"pqrs", '8':"tuv", '9':"wxyz"
    }
    def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        final = []
        length = len(digits)
        if length == 0:
            return ""
        # 当等于1的时候，通过append.方法将字母添加
        elif length == 1:
            num = digits[0]
            for item in self.dic[num]:
                final.append(item)
            return final
        # 这个是比较重要一步，注意for循环里的操作
        else:
            num = digits[0]
            digits = digits[1:]
            for item in self.dic[num]:
                # expression for x in a 是一种python循环操作
                # 将下一层迭代后的list中的每个元素加入当前数字代表的元素即可
                final += [item + x for x in self.letterCombinations(digits)]
        return final
```

