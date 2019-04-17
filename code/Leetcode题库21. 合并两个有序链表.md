#### Leetcode题库21. 合并两个有序链表

将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

**示例：**

```
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
```

思路：

​	由于只有两个链表，所以对两个链表进行遍历即可

​	由于链表结构的特殊性，通常先定义一个空节点，再在.next新建链表。保证不会出现某些异常情况

​	在某个链表遍历完之后，直接将结果指针指向未遍历完的链表即可

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        # 设定两个指针，一个用来返回结果，另一个用来操作
        final = ListNode(-1)
        head = final
        while l1 != None and l2 != None:
            temp1 = l1.val
            temp2 = l2.val
            # 比较过后读取指针
			if temp1 < temp2:
                head.next = ListNode(temp1)
                l1 = l1.next
            else:
                head.next = ListNode(temp2)
                l2 = l2.next
            # 指向next节点
            head = head.next
        if l1 != None:
            head.next = l1
        if l2 != None:
            head.next = l2
        # 最后返回结构指针指向的结果
        return final.next
        
```
