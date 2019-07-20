#### Leetcode题库54. 螺旋矩阵

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

示例 1:

```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```


示例 2:

```
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]
```



思路：

​	首先这道题的难度在于只有单行或者单列的时候怎么去判断。

​	方法先把外面一圈消灭，之后处理内圈的操作。

​	在处理只剩一行或者只有一列的时候，我们可以使用集合判断，如果重复出现的话则跳过，基于此写出了暴力版本的代码。

​	优化方法可以通过操作减少在集合判断的操作，可以直接在nums数组上进行数组的删减。

第一版：

```python
class Solution(object):
    def spiralOrder(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[int]
        """
        if matrix == []: return []
        lim = set()
        result = []
        m = len(matrix)
        n = len(matrix[0])
        times = int(min(m, n) / 2) + min(m, n) % 2
        for i in range(times):
            # 每一轮的起点都是(0,0),(1,1)...
            row, col = i, i
            加入一点后，并在set中插入作为后面的判断。
            lim.add((row, col))
            result.append(matrix[row][col])
            # 先进行+1的判断，如果直接+1会出错，
            while col + 1 < n - i and (row, col + 1) not in lim:
                col += 1
                lim.add((row, col))
                result.append(matrix[row][col])

            while row + 1< m - i and (row + 1, col) not in lim:
                row += 1
                lim.add((row, col))
                result.append(matrix[row][col])

            while col - 1 > -1 + i and (row, col - 1) not in lim:
                col -= 1
                lim.add((row, col))
                result.append(matrix[row][col])

            while row - 1 > 0 + i and (row - 1, col) not in lim:
                row -= 1
                lim.add((row, col))
                result.append(matrix[row][col])

        return result
```





​				

