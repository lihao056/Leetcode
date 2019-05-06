#### Leetcode题库37. 解数独

编写一个程序，通过已填充的空格来解决数独问题。

一个数独的解法需**遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。

空白格用 `'.'` 表示。

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)

一个数独。

![img](http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)

答案被标成红色。

**Note:**

- 给定的数独序列只包含数字 `1-9` 和字符 `'.'` 。
- 你可以假设给定的数独只有唯一解。
- 给定数独永远是 `9x9` 形式的。

思路：

​	首先我玩数独的时候只是通过投机取巧来的，所以这道题我参考了网络上的解法。

​	做这题首先要返回到如何解数独的思想上，正常来说，每个数独的空白项都有可选择的数字，从理论上说，只要每次在当前空格下选择一个数字，判断是否合适，有效则继续选择下一个空格的数字，否则跳回上一个空格(因为当前空格没有有效的数字)，直到所有空格都被填完且没有错误后，数独已经解出来了。

​	所以这可以转换成一个深度优先搜索的问题，在某一个条件下选择这个值，并基于这个条件下判断，直至最后解决完所有空格。

第一版：

​	使用深度优先搜索和递归的方式解决，[链接](https://www.cnblogs.com/zuoyuan/p/3770271.html)

```python
class Solution:
    def solveSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        # isValid函数判断坐标为x,y的值是否有效
        def isValid(x,y):
            tmp=board[x][y]; board[x][y]='D'
            for i in range(9):
                if board[i][y]==tmp: return False
            for i in range(9):
                if board[x][i]==tmp: return False
            for i in range(3):
                for j in range(3):
                    if board[(x/3)*3+i][(y/3)*3+j]==tmp: return False
            board[x][y]=tmp
            return True
        # 深度优先搜索的函数
        def dfs(board):
            for i in range(9):
                for j in range(9):
                    # 遍历数独中所有的空格，跳过已经存在数字的格子
                    if board[i][j]=='.':
                        for k in '123456789':
                            board[i][j]=k
                            # 判断board[i][j]=k以及基于board[i][j]的情况下的继续搜索是否符合
                            if isValid(i,j) and dfs(board):
                                return True
                            # 若不符合，则当前空格变为'.'
                            board[i][j]='.'
                        return False
            # 这里是基于所有空格都填写完的情况，返回True
            return True
        dfs(board)
```

