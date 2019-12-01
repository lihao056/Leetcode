#### Leetcode题库60. 第k个排列(规律推导)

给出集合 [1,2,3,…,n]，其所有元素共有 n! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

```
"123"
"132"
"213"
"231"
"312"
"321"
```


给定 n 和 k，返回第 k 个排列。

说明：

给定 n 的范围是 [1, 9]。
给定 k 的范围是[1,  n!]。
示例 1:

```
输入: n = 3, k = 3
输出: "213"
```


示例 2:

```
输入: n = 4, k = 9
输出: "2314"
```

思路：

​	全排列的题目包括LeetCode31下一个排列，LeetCode46,47全排列等等。

​	排列的顺序是按照是否为降序排列进行判断（参考之间算法）

​	首先题目要求找到排列当中第几个的数组，问题是这个如何确定呢。一开始我还想直接暴力法做，但是肯定是超时的，因为需要一个一个推出来当面对9！的时候肯定会错。所以需要考虑另一种方法。

第一版：

​	暴力法，使用数组方便操作，str不方便拆分替换等操作。

​	将数组倒置方便操作。

```python
class Solution(object):
    def getPermutation(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: str
        """
        result = ""
        lis = [x + 1 for x in range(n)]
        lis = lis[::-1]
        i = k - 1
        while i:
            # 使用enumerate纪录位置信息
            for pos, item in enumerate(lis):
                if pos == 0: continue
                # 若不按照降序排列，进行处理
                # 先提取需要排列的元素按照升序排列，获取当前位置pos数值大的另一个值a
                # 将a从临时数组删除，并将lis[pos]用a替换
                # 最后将数组的后置位和tmp替换
                if lis[pos - 1] >= item:
                    tmp = sorted(lis[:pos+1])
                    a = tmp[tmp.index(item) + 1]
                    tmp.remove(a)
                    lis[pos] = a
                    lis[:pos] = tmp[::-1]
                    i -= 1
                    break
        for x in lis[::-1]:
            result += str(x)
        return result
```

第二版：

​	参照LeetCode题解的方法，进行规律推导。

​	链接如下：https://leetcode-cn.com/problems/two-sum/solution/zhao-gui-lu-zhi-jie-ji-suan-da-an-by-zhangll/

​	作者将题目做出了几点简化，首先每位的数字可以通过除法运算确定，确定位置后下一位可以通过余数进一步确定。这里的关键是确定除数已经初始数值。

```
1.需要先定义从1到n的数组。上文中说的 k/(n-1)! 和 mod/(n-2)! 其实并不严谨，第二位实际上应该是算完第一位后排除第一位的答案后，剩余数组的第 mod/(n-2)! 个元素。
2.由于引入了数组，第一位计算前mod 应该为 k-1。（因为第一个排列就是数组本身）
3.当余数为0时，实际上没有必要继续计算了，只需将剩余数组元素，依次添加进答案即可。
```



```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        result = ''
        mod = k - 1
        if n == 1: return '1'
        n_set = [str(i) for i in range(1, n + 1)]
        # fac为被除数，即为(n-1)!
        fac = 1
        for i in range(n - 1):
            fac *= i + 1
        for i in range(n):
            # 得到val和mod算出当前位的值和继续下一步计算
            val = int(mod / fac)
            mod = mod % fac
            fac = fac / (n - 1 - i)
            result += n_set[val]
            del n_set[val]
            if mod == 0:
                for i in n_set: result += i
                return result

        return result
```



​	

