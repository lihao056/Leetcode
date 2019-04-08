#### Leetcode周赛-5017. 从根到叶的二进制数之和

给出一棵二叉树，其上每个结点的值都是 `0` 或 `1` 。每一条从根到叶的路径都代表一个从最高有效位开始的二进制数。例如，如果路径为 `0 -> 1 -> 1 -> 0 -> 1`，那么它表示二进制数 `01101`，也就是 `13` 。

对树上的每一片叶子，我们都要找出从根到该叶子的路径所表示的数字。

以 **10^9 + 7** 为**模**，返回这些数字之和。

 

**示例：**

![image](https://github.com/lihao056/Leetcode/blob/master/code/picture/1554715812239.png)

```
输入：[1,0,1,0,1,0,1]
输出：22
解释：(100) + (101) + (110) + (111) = 4 + 5 + 6 + 7 = 22
```

 

**提示：**

1. 树中的结点数介于 `1` 和 `1000` 之间。
2. node.val 为 `0` 或 `1` 。

思路：

​	这道题目考了数据结构中的二叉树内容，通过遍历叶结点，可以并将节点数据存储后就可以进行计算。

​	首先我是去网络上找的深度遍历二叉树算法的代码(没学过数据结构)

​	之后考虑的是怎么将数据进行保存，后求解。

​	我考虑使用字典将数据进行存储，因为还需要将顺序列出来，所以逻辑和程序比较难看。

​	另外，注意题目的审题，取模一定要记得取。

程序如下：

```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
								
class Solution(object):								# 上面这一串是关于树的的代码，题目提供
    def sumRootToLeaf(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if root == None:							# 若没有树则返回空
            return
        stack = []									# 使用栈进行二叉树的遍历
        position = []								# 存储节点的层数信息
        dic = {}									# 存储节点值
        temp = 0
        final = 0
        stack.append(root)
        position.append(0)
        while stack:								# 遍历完成后自动结束循环
            now_node = stack.pop()
            now_pos = position.pop()		
            dic[now_pos] = now_node.val				# dic纪录位置及数值信息

            if now_node.right:
                stack.append(now_node.right)
                position.append(now_pos + 1)
            if now_node.left :
                stack.append(now_node.left)
                position.append(now_pos + 1)
            if now_node.left == None and now_node.right == None:	# 当左右都无下一节点时
                for i in range(now_pos + 1):						# 才进行相加计算，位置是
                    temp = temp * 2 + dic[i]						# 从0到当前层数，
                final = (final + temp) % (10 ** 9 + 7)				# 题目要求取模
                temp = 0

        return final
```

另外附上大神的代码，膜拜！

https://leetcode.com/problems/sum-of-root-to-leaf-binary-numbers/discuss/270025/JavaC%2B%2BPython-Recursive-Solution

```
   def sumRootToLeaf(self, root, val=0):
        if not root: return 0
        val = (val * 2 + root.val) % (10**9 + 7)
        if root.left == root.right: return val			
        return (self.sumRootToLeaf(root.left, val) + self.sumRootToLeaf(root.right, val)) % (10**9 + 7)
```

​	使用迭代法进行求解，并将数值通过参数进行迭代，减少代码数量。

​	这里的判断语句非常巧妙，由于root.left和root.right不是以数值方式表示的，所以只有一种情况他们是相等的，就是都为None的时候，也就是叶结点，通过迭代既可以进行将所有的叶结点遍历。

```
<precompiled.treenode.TreeNode object at 0x7fdd0523e750>
<precompiled.treenode.TreeNode object at 0x7fdd0523e790>
None
None
```

​	
