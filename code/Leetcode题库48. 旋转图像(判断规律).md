#### Leetcode题库48. 旋转图像(判断规律)

给定一个 n × n 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

说明：

你必须在原地旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要使用另一个矩阵来旋转图像。

示例 1:

给定 matrix = 

```
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],
```

原地旋转输入矩阵，使其变为:

```
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

示例 2:

给定 matrix =

```
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 
```

原地旋转输入矩阵，使其变为:

```
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

思路：

​	这种题目一开始想法是找规律，因为不能使用多余的矩阵空间，所以在找元素交换时候的位置规律。

​	之后发现，元素替换发生在同一个方框边界中，以最外层边界往中心靠，同一个方框中的元素不会于其他方框冲突。

​	所以接下来就是通过变量控制边界了。

第一版：

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        # n为检索的指示，m为边界方框的层数，time表示搜寻的次数（每搜一次边界会变小）。
        n = m = len(matrix)
        time = 0
        while m > 0:
            for i in range(m - 1):
                # 使用swap方法交换位置，规律如下
                # (0, 0) <- (n, 0) <- (n, n) <- (0, n) <- (0, 0)
                temp = matrix[0 + time][i + time]
                matrix[0 + time][i + time] = matrix[n - 1 - i - time][0 + time]
                matrix[n - 1 - i - time][0 + time] = matrix[n - 1 - time][n - 1 - i -time]
                matrix[n - 1 - time][n - 1 - i - time] = matrix[i + time][n - 1 - time]
                matrix[i + time][n - 1 - time] = temp
            time += 1
            m -= 2
```

第二版：

​	LeetCode大神的版本，python中执行用时为 16 ms 的范例

​	只要对矩阵进行倒置和转置就可以达到旋转的效果

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        matrix[:] = matrix[::-1]
        for i in range(0, len(matrix)):
            for j in range(i+1, len(matrix)):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```

