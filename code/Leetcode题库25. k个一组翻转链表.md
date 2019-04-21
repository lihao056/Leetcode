#### Leetcode题库25. k个一组翻转链表

给出一个链表，每 *k* 个节点一组进行翻转，并返回翻转后的链表。

*k* 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 *k* 的整数倍，那么将最后剩余节点保持原有顺序。

**示例 :**

给定这个链表：`1->2->3->4->5`

当 *k* = 2 时，应当返回: `2->1->4->3->5`

当 *k* = 3 时，应当返回: `3->2->1->4->5`

**说明 :**

- 你的算法只能使用常数的额外空间。
- **你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。



思路：

​	这个思路与链表两两翻转的思路差不多，但是要求变为k个。

​	所以需要用数组存储k个链表，并按照类似24. 两两交换链表中的节点的操作对k个链表进行操作。

​	主要还是头尾两个链表的操作，剩下中间的都是一样的。

第一版：

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        final = ListNode(-1)
        final.next = head
        recond = final
        lis = []
        while True:
            for i in range(k):
                if head == None:
                    break
                lis.append(head)
                head = head.next
            # 如果剩下的链表不足k个则跳出循环
            if len(lis) < k:
                break
            # 对头尾进行操作
            i = k
            recond.next = lis[k - 1]
            lis[0].next = lis[k - 1].next
            # 对中间进行操作
            while i >= 2:
                lis[i -1].next = lis[i - 2]
                i -= 1
            recond = lis[0]
            # 清空列表为了下一次循环
            lis = []
        return final.next 
```
