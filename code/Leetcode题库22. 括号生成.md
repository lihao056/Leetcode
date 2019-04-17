#### Leetcode题库22. 括号生成

给出 *n* 代表生成括号的对数，请你写出一个函数，使其能够生成所有可能的并且**有效的**括号组合。

例如，给出 *n* = 3，生成结果为：

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

思路：

​	因为输入参数只有一个，同时生成括号的方式是有规律可寻找的，所以考虑使用迭代法进行编写，

​	当参数+1时，可以想象成在原参数结果的基础上增加符合的情况。

​	当n = 1时，只有一种

​	当n = 2时，可以想成在n=1的基础上添加情况

​	当n = 3时，可以为(n = 1) + (n = 2)，(n = 2) + (n = 1)的情况(**注意，1 + 2和 2 + 1是两种不同的情况**)

​	...

​	补充：还有一种在原结果的两边加上括号的情况，如”( + (n = 2) + )“的情况

​	按照这种想法，写出了第一版代码，耗时量大

第一版：

```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        final = []
        if n == 0:
            return final
        if n == 1:
            final.append("()")
            return final
        # n = 2返回两种情况
        if n == 2:
            final += ['(' + str(self.generateParenthesis(1)[0]) + ')']
            final += ["()" + str(self.generateParenthesis(1)[0])]
            return final
        # 当n > 2时，需要考虑多种情况
        if n > 2:
            # 在原结果两边加上括号的情况
            final += ['(' + x + ')' for x in self.generateParenthesis(n - 1)]
            for i in range(n - 1):
                # n = i + n = j的情况
                for item in self.generateParenthesis(i + 1):
                    final += [item + x for x in self.generateParenthesis(n - i - 1)]
            # 这个是去重的操作，将每种情况记录至字典中，将键取出作为数组
            dic = {}
            for item in final:
                dic[item] = dic.get(item, 0) + 1
            final = list(dic.keys())
            return final
```

第二版：

​	使用动态规划的思想，因为在网上看到博文，所以想着实现一下。

​	[算法-动态规划 Dynamic Programming--从菜鸟到老鸟](https://blog.csdn.net/u013309870/article/details/75193592)

​	使用自顶向下的备忘录法，时间比第一版缩短10倍，同时在类内进行函数迭代比self.method要快

```python
class Solution(object):
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        # 用来存储n = i的情况
        m = []
        for i in range(n):
            m.append("")
		# 定义迭代的方式，对第一版的方法做了点修改，加入存储到m的步骤
        def support(n, m):
            # 如果m存在这种情况了，就不要计算直接返回结果
            if m[n - 1] != "":
                return m[n - 1]
            final = []
            if n == 0:
                return final
            if n == 1:
                final.append("()")
                m[0] = final
                return final
            if n == 2:
                final += ['(' + str(support(1, m)[0]) + ')']
                final += ["()" + str(support(1, m)[0])]
                m[1] = final
                return final
            if n > 2:
                final += ['(' + x + ')' for x in support(n - 1, m)]
                for i in range(n - 1):
                    for item in support(i + 1, m):
                        final += [item + x for x in support(n - i - 1, m)]
                dic = {}
                for item in final:
                    dic[item] = dic.get(item, 0) + 1
                final = list(dic.keys())
                m[n - 1] = final
                return final
        return support(n, m)
```



