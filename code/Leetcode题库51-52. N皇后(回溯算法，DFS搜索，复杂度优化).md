#### Leetcode题库51-52. N皇后(回溯算法，DFS搜索，复杂度优化)

n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/8-queens.png)

上图为 8 皇后问题的一种解法。

给定一个整数 n，返回所有不同的 n 皇后问题的解决方案。

每一种解法包含一个明确的 n 皇后问题的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

示例:

```
输入: 4
输出: [
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
解释: 4 皇后问题存在两个不同的解法。

思路：
```

​		一开始是想用跟解数独的方法来做这道题，通过DFS和回溯进行求解。但是之前是没有优化的，所以时间上是超时了的，现在需要对回溯算法以及DFS做优化处理。

​	最终参照了官方题解上的解决方法。

​	官方上的方法有以下关键的想法。

​	1.通过约束下一个Queen的方法，减少循环次数，由于每行，每列，两个斜边都需要没有Queen，所以，在进行dfs搜寻中，只需要扫描1行的次数即可，若这行不存在合适的位置，说明这个放置位置不行，直接跳过这种情况，回溯即可。

​	2.找到斜边的对应关系，通过list列表判断当前节点是否存在冲突。

​	3.使用集合的方式，存放n皇后的位置信息，减少存储空间使用。

```python
class Solution(object):
    def solveNQueens(self, n):
        """
        :type n: int
        :rtype: List[List[str]]
        """
        result = []
        # 使用temp作为展示棋盘的变量
        temp = ["." * n] * n
        # 使用3个list判断位置信息
        col = [0] * n
        left_dig = [0] * (n * 2 - 1)
        right_dig = [0] * (n * 2 - 1)
		# 回溯算法主要是回溯的条件问题
        def dfs(temp, row):
            if row == n:
                res = temp[:]
                if res not in result:
                    result.append(res)
                return
            for j in range(n):
                if checkpoint(row, j):
                    placeQueen(row, j)
                    dfs(temp, row + 1)
                    removeQueen(row, j)

		# 检测当前节点位置信息
        def checkpoint(x, y):
            return not col[y] + left_dig[x - y] + right_dig[x + y]
		
        # 移除当前节点，并减少约束
        def removeQueen(x, y):
            tmp = list(temp[x])
            tmp[y] = '.'
            temp[x] = "".join(tmp)
            col[y] = 0
            left_dig[x - y] = 0
            right_dig[x + y] = 0
		
        # 增加当前节点，并更新约束
        def placeQueen(x, y):
            tmp = list(temp[x])
            tmp[y] = 'Q'
            temp[x] = "".join(tmp)
            col[y] = 1
            left_dig[x - y] = 1
            right_dig[x + y] = 1
		
        dfs(temp, 0)
        return result
```

第二版

​	[LeetCode官方解答](<https://leetcode-cn.com/problems/n-queens-ii/solution/nhuang-hou-ii-by-leetcode/>)

```java
class Solution:
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        def is_not_under_attack(row, col):
            return not (rows[col] or hills[row - col] or dales[row + col])
        
        def place_queen(row, col):
            rows[col] = 1
            hills[row - col] = 1  # "hill" diagonals
            dales[row + col] = 1  # "dale" diagonals
        
        def remove_queen(row, col):
            rows[col] = 0
            hills[row - col] = 0  # "hill" diagonals
            dales[row + col] = 0  # "dale" diagonals
        
        def backtrack(row = 0, count = 0):
            for col in range(n):
                if is_not_under_attack(row, col):
                    place_queen(row, col)
                    if row + 1 == n:
                        count += 1
                    else:
                        count = backtrack(row + 1, count)
                    remove_queen(row, col)
            return count
        
        rows = [0] * n
        hills = [0] * (2 * n - 1)  # "hill" diagonals
        dales = [0] * (2 * n - 1)  # "dale" diagonals
        return backtrack()
```



