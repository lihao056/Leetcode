#### Leetcode题库24. 两两交换链表中的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

 

**示例:**

```
给定 1->2->3->4, 你应该返回 2->1->4->3.
```

思路：

​	对于链表交替需要考虑的是指向链表的表头的位置。假设设置一个final链表，将其的next节点指向head链表。后单独进行交换的话会出现以下结果

​	![1555552467443](G:\typora图片\1555552467443.png)

​	可以看到，我的final节点会将删除链表2。所以需要考虑增加一个record节点替代final节点操作。

​	本题的关键是如何**避免节点的删除**，同时考虑**当前节点指向链表的位置**。

​	整个过程如下

![1555553338461](G:\typora图片\1555553338461.png)

第一版：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        # 设置final节点用于输出，设置recond节点用于循环翻转操作
        final = ListNode(-1)
        final.next = head
        recond = final
        # 设置cur_list表示遍历到的节点
        cur_list = head
        # 注意，若没有两个链表，则不需要任何操作
        while cur_list != None and cur_list.next != None:
            first = cur_list
            second = cur_list.next
            # 翻转操作
            recond.next = second
            first.next = second.next
            second.next = first
            # 这个是为了下一次循环jin
            cur_list = cur_list.next
            recond = recond.next.next
        return final.next
            
```
