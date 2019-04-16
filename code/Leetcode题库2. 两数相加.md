#### Leetcode题库2. 两数相加

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例：**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

思路：

​	这道题采用了链表的结构，所以和平常的题目稍微不一样。

​	思路还是相同的，首先读取l1和l2的数据，求出和之后再做出处理。

​	求出和之后，转换为字符串处理。

​	可以有以下两种方法处理

​	1.不处理字符串，但题目要求逆序，步骤比较复杂

​	2.将字符串逆序，之后按照链表方式处理(推荐)

以下为程序

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        temp1 = l1.val
        temp2 = l2.val
        i = 1
        while (l1.next != None):
            l1 = l1.next
            temp1 += 10 ** i * l1.val
            i += 1
        i = 1
        while (l2.next != None):
            l2 = l2.next
            temp2 += 10 ** i * l2.val
            i += 1
        final = temp1 + temp2
		# 方法一，不做逆序处理
        temp = []
        # 构建列表存储单个链表
        for i in range(len(str(final))):
            temp.append(ListNode(0))
        # 将每个链表赋值
        for pst, i in enumerate(str(final)):
            temp[pst].val = int(i)
        # 将前一个链表的赋予后一个链表的next方法
        for i in range(len(temp) - 1):
            temp[i + 1].next = temp[i]
        return temp[len(temp) - 1]
   	'''
    	# 方法二，对字符串做逆序处理
        c = ([x for x in str(final)][::-1])
        # 首先定义一个链表，并赋予值
        head = ListNode(c[0])
        # 通过中间变量处理增加链表操作
        cur_node = head
        for i in c[1:]:
        	# 将.next创建为一个链表，并赋值
            cur_node.next = ListNode(i)
            # 将中间变量变为.next的链表
            cur_node = cur_node.next
        return head   
	’‘’
```

