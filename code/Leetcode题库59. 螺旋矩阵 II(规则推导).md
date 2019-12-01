#### Leetcode题库59. 螺旋矩阵 II(规则推导)

给定一个正整数 n，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的正方形矩阵。

示例:

输入: 3
输出:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

思路：

​	这道题目和Leetcode题库54.螺旋矩阵很相似，只不过这题返回结果是矩阵，另一个是列表，中间难点都是在于将矩阵元素顺时针获取而已。

​	所以使用4个循环操作将循环一圈，向左，向下，向右和向上的操作实现。

​	同时由于矩阵只能是方阵，所以不需要考虑m * n 情况，减少判断条件。	

第一版：

```python
class Solution(object):
    def generateMatrix(self, n):
        """
        :type n: int
        :rtype: List[List[int]]
        """
        matrix = [[0 for i in range(n)] for j in range(n)]
        lis = [x + 1 for x in range(n * n)]
        time = int(n / 2) + n % 2
        for i in range(time):
            # 使用pop操作顺序将数组内的数排序
            row, col = i, i
            matrix[row][col] = lis.pop(0)
            while col + 1 < n - i:
                col += 1
                matrix[row][col] = lis.pop(0)
            while row + 1 < n - i:
                row += 1
                matrix[row][col] = lis.pop(0)
            while col - 1 > -1 + i:
                col -= 1
                matrix[row][col] = lis.pop(0)
                # 向上主要是会多了最初始的一个点，需要加1
            while row - 1 > -1 + 1 + i:
                row -= 1
                matrix[row][col] = lis.pop(0)
        return matrix

```





​				

