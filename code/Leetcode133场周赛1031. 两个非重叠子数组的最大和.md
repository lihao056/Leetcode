#### Leetcode133场周赛1031. 两个非重叠子数组的最大和

给出非负整数数组 `A` ，返回两个非重叠（连续）子数组中元素的最大和，子数组的长度分别为 `L` 和 `M`。（这里需要澄清的是，长为 L 的子数组可以出现在长为 M 的子数组之前或之后。）

从形式上看，返回最大的 `V`，而 `V = (A[i] + A[i+1] + ... + A[i+L-1]) + (A[j] + A[j+1] + ... + A[j+M-1])` 并满足下列条件之一：

 

- `0 <= i < i + L - 1 < j < j + M - 1 < A.length`, **或**
- `0 <= j < j + M - 1 < i < i + L - 1 < A.length`.

 

**示例 1：**

```
输入：A = [0,6,5,2,2,5,1,9,4], L = 1, M = 2
输出：20
解释：子数组的一种选择中，[9] 长度为 1，[6,5] 长度为 2。
```

**示例 2：**

```
输入：A = [3,8,1,3,2,1,8,9,0], L = 3, M = 2
输出：29
解释：子数组的一种选择中，[3,8,1] 长度为 3，[8,9] 长度为 2。
```

**示例 3：**

```
输入：A = [2,1,5,6,0,9,5,0,3,8], L = 4, M = 3
输出：31
解释：子数组的一种选择中，[5,6,0,9] 长度为 4，[0,3,8] 长度为 3。
```

 

**提示：**

1. `L >= 1`
2. `M >= 1`
3. `L + M <= A.length <= 1000`
4. `0 <= A[i] <= 1000`

思路：

​	没有注意审题，但是做的时候以为是两个不重复的数组之和，当时还在想着还要判断不重复，还要求最大和，有点难的就没管。

​	回过头来看大神的思路之后，发现只要非重叠就好。

​	所以第一版是Leetcode大神一个繁琐的方法，将L在M左边和L在M右边的情况都遍历出来解决。

[第一版](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/discuss/278812/O(N2)-Prefix-sum-array-Python-solution)：(点击可以看原文)

```python
class Solution:
    def maxSumTwoNoOverlap(self, A: List[int], L: int, M: int) -> int:
        prefix = [0]
        # 使用列表存储列表中所有数的相加和，通过这个形式求数组和
        for i in A:
            prefix.append(i + prefix[-1])
            
        res = 0
        # 考虑L在M右边的情况，
        for i in range(M - 1, len(A)):
            for j in range(i + L, len(A)):
                res = max(res, prefix[i + 1] - prefix[i - M + 1] + prefix[j + 1] - prefix[j - L + 1])
        # 考虑L在M左边的情况
        for i in range(L - 1, len(A)):
            for j in range(i + M, len(A)):
                res = max(res, prefix[i + 1] - prefix[i - L + 1] + prefix[j + 1] - prefix[j - M + 1])
        return res
```

[第二版](https://leetcode.com/problems/maximum-sum-of-two-non-overlapping-subarrays/discuss/278883/DP-solution-using-java)：(点击可以看原文)

​	第二版是参照网络上Java的动态规划算法，将其修改为Python版本，时间比第一版时间要少。

​	本题的目的是为了找出两个数组，L，M其中L，M要不重叠，所以我们可以先把一个L数组的最大的情况写出来，再通过这个求出M的情况，一样的，需要考虑两边的情况。

```python
class Solution:
    def maxSumTwoNoOverlap(self, A, L, M):
        """
        :type A: List[int]
        :type L: int
        :type M: int
        :rtype: int
        """
        # dpLeft用来存储从左至右，L数组所能取到的最大情况
        dpLeft = []
        cur = 0
        for i, item in enumerate(A):
            cur += A[i]
            if (i >= L):
                cur -= A[i - L]
            if i < L - 1:
                dpLeft.append(0)
            else:
                dpLeft.append(max(dpLeft[i - 1] if i - L >= 0 else 0, cur))
		# dpReft用来存储从右至左，L数组所能取到的最大情况(这个位置中存储信息是：从该位置(包括这个位置)往右所有可能的连续L数组的最大和)
        # 注意区别于dplift，dpright数组的插入不是按照顺序来的，是按照逆序来的
        dpright = []
        cur = 0
        for i in range(len(A) -1, -1, -1):
            cur += A[i]
            if (i < len(A) - L):
                cur -= A[i + L]
            if i > len(A) - L:
                dpright.append(0)
            else:
                # 由于是逆序，所以使用insert，插入序号为0
                dpright.insert(0, max(dpright[0] if i + 1 < len(A) else 0, cur))

        max1 = 0
        cur = 0
        # 这个就是找最大可能的情况，
        for i in range(len(A)):
            cur += A[i]
            if i >= M:
                cur -= A[i - M]
            # 如果大于等于M，则可以考虑加入dpLeft的情况。
            if i >=  M:
                max1 = max(max1, cur + dpLeft[i - M])
            # 如果下雨len(A) - L,则可以考虑加入dpRight的情况。
            if i < len(A) - L:
                max1 = max(max1, cur + dpright[i + 1])
        return max1
```

