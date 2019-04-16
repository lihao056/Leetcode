#### Leetcode题库19. 删除链表的倒数第N个节点

给定一个链表，删除链表的倒数第 *n* 个节点，并且返回链表的头结点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.
```

思路：

​	首先，这对于熟悉数据结构的人来说，这道题非常简单。因为直接是链表删除的要求，找到这个节点就好了。对于不熟悉数据结构的，可能就比较难办，所以分为两种解法，第一种对于不熟悉数据结构的人来说可能容易接受一点，第二种是采用链表删除的方法。

​	方法一：

​	1.找出链表中倒数第n个节点，并将之后的节点保存

​	2.剔除要求节点后，用另一个指针遍历，将删除节点前面的链表存储。

​	3.通过拼接即可获得

​	4.需要注意特殊的情况，如下面三种情况

​	链表：[1]		链表：[1,2]	链表：[1,2]	

​	n = 1			n = 2		   n = 1

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        
        length =  0
        temp = []
        head_copy = head
        # 通过遍历链表，读取链表长度，并将链表的倒数N个节点求出
        while head:
            length += 1
            # 采用队列结构存储，先入先出
            if len(temp) == n:
                temp.pop(0)
            temp.append(ListNode(head.val))
            head = head.next
        # 考虑一些特殊情况，如果链表为1，还要删除1个节点，直接返回空，
        if length == 1:
            return
        # 因为这个是要删除的节点，直接剔除
        temp.pop(0)
        # 剔除后会出现问题，当只删除倒数一个的时候，del_list就为None
        if len(temp) == 0:
            del_list = None
        else:
            # 若temp还有值，则组成新的链表，这就是删除了目标节点的链表
            del_list = temp[0]
            cur_node = del_list
            for i in range(len(temp) - 1):
                cur_node.next = temp[i + 1]
                cur_node = cur_node.next
        # 寻找删除节点前的节点，需要判断若删除的节点是首节点的情况
        i = 0
        if length - n != 0:
            new_list = ListNode(head_copy.val)
            cur_node = new_list
            while head_copy:
                # 注意这里的值，当到达节点后跳出循环
                if i >= (length - n - 1):
                    break
                # 指向下一个链表操作
                cur_node.next = head_copy.next
                head_copy = head_copy.next
                cur_node = cur_node.next
                i += 1

            cur_node.next = del_list
            return new_list
        # 如果删除的是首节点，则返回del_list
        else:
            return del_list
```

​	方法二(参照网络上的方法)：

​	采用链表删除的方法，将当前链表.next指向当前链表.next.next中。

```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        # 设立初始节点，并将head指向这个链表
        dummy = ListNode(-1)
        dummy.next = head
        # 设定3个指针，这三个指针指向的都是相同的存储空间
        fast = slow = dummy
        # 首先遍历第一个指针，将遍历至n次，到某节点(无含义)
        while n and fast:
            fast = fast.next
            n -= 1
        '''
        在第一个指针的前提上，第二个指针继续遍历，当第一个指针遍历完后，
        第二个指针恰好到达被删除的节点前，之后当前链表.next指向当前链表.next.next中。
        即可完成删除的操作，如有不懂可以查阅数据结构关于链表删除部分。
        '''
        while fast.next and slow.next:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next
        return dummy.next
```

