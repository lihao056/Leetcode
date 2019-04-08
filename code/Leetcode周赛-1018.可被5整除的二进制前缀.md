#### Leetcode周赛-1018. 可被 5 整除的二进制前缀

给定由若干 `0` 和 `1` 组成的数组 `A`。我们定义 `N_i`：从 `A[0]` 到 `A[i]` 的第 `i` 个子数组被解释为一个二进制数（从最高有效位到最低有效位）。

返回布尔值列表 `answer`，只有当 `N_i` 可以被 `5` 整除时，答案 `answer[i]` 为 `true`，否则为 `false`。

**示例 1：**

```
输入：[0,1,1]
输出：[true,false,false]
解释：
输入数字为 0, 01, 011；也就是十进制中的 0, 1, 3 。只有第一个数可以被 5 整除，因此 answer[0] 为真。
```

**示例 2：**

```
输入：[1,1,1]
输出：[false,false,false]
```

**示例 3：**

```
输入：[0,1,1,1,1,1]
输出：[true,false,false,false,true,false]
```

**示例 4：**

```
输入：[1,1,1,0,1]
输出：[false,false,false,false,false]
```

**提示：**

```
1. 1 <= A.length <= 30000`
2. `A[i]` 为 `0` 或 `1`
```

​	这个题目一开始是想通过读取数组序列的形式将数字一个一个进行相加并判断，并输出数组。

​	但是这样子的计算复杂度为O(N<sup>2</sup>)，时间是超时的

​	由于是二进制数，每次读取的是最后的个位数，可以乘以进制数将前面的数移位，并加入最后一位数组成新的数值。这样子可以变成O(N)

方法一：

```
class Solution:
    def prefixesDivBy5(self, A: List[int]) -> List[bool]:
    	'''
    	:type A:List[int]
    	:rtype List
    	'''
        result = []							# 通过存储数据
        temp = 0
        for item in (A):
            temp *= 2						# 乘以进制数得到新的数字
            temp += int(item)
            if temp % 5 == 0:
                result.append(True)			
            else:
                result.append(False)
        return result
```



