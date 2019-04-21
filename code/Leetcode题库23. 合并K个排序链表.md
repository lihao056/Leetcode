#### Leetcode题库23. 合并K个排序链表

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

**示例:**

```
输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6
```

思路：

​	需要对k个链表进行排序，就有点想法，这和21.合并两个有序链表类似，只不过两个变成k个了。

​	所以一开始想的是暴力法，对所有的进行排序，找出最小值后将取值并将该链表指针指向下一个节点。

​	1.利用字典存储每个链表的位置信息和当前指向链表的值

​	2.取出字典中的values，组成列表后进行排序，找到最小值。

​	3.遍历字典的values，找到最小值对应的链表的位置

​	4.之后更新字典继续下一次循环

​	这个思路本质上是没有问题的，就是复杂度太高，会超时，所以查阅了一下其他方法	

第一版：

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        final = ListNode(-1)
        head = final
        dic = {}
        # 利用字典存储每个链表的位置信息和当前指向链表的值
        for pos, item in enumerate(lists):
            if item != None:
                dic[pos] = item.val
        while dic:
            # 取出字典中的values，组成列表后进行排序，找到最小值
            mi = sorted(list(dic.values()))[0]
            # 遍历字典的values，找到最小值对应的链表的位置
            for i in dic:
                if dic[i] == mi:
                    head.next = ListNode(lists[int(i)].val)
                    head = head.next
                    # 更新字典继续下一次循环
                    if lists[int(i)].next != None:
                        lists[int(i)] = lists[int(i)].next
                        dic[i] = lists[int(i)].val
                    else:
                        dic.pop(i)
                        break
        return final.next
```

第二版：

​	使用最小堆的方法进行求解。

​	利用元组将链表的值和链表进行打包，放入堆结构中。

​	思路参照下列[博客](https://blog.csdn.net/iyuanshuo/article/details/79600011)

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
		# 利用队列存储链表信息
        heap = []
        for ln in lists:
            if ln:
                heap.append((ln.val, ln))
        dummy = ListNode(0)
        cur = dummy
        # 这里是使用.heapify方法维护堆，其次对堆排序取决于第一个元素的大小
        # 如果是(val, list)则关于val进行排序，如果是(list,val)则关于链表的第一个值进行排序
        heapq.heapify(heap)
        while heap:
            # 使用heappop的方法取出堆顶最小的元素
            valu, ln_index = heapq.heappop(heap)
            cur.next = ln_index
            cur = cur.next
            if ln_index.next:
                # 使用heappush插入入元素并对堆进行维护
                heapq.heappush(heap, (ln_index.next.val, ln_index.next))
        return dummy.next
```

第三版：

​	使用分治法进行排序，将k个链表分成两个两个一组进行排序，

​	之后继续对已经合并好的进行排序，直到剩下最后一个链表为止。

​	这个版本参照[博客](https://blog.csdn.net/yurenguowang/article/details/78034619)

```python
class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        if lists is None or len(lists) == 0:
            return None
        while len(lists) > 1:
            tmp = []
            # 对两个链表两两排序
            for i in range(0, len(lists) - 1, 2):
                p = self.mergeTwoList(lists[i], lists[i + 1])
                tmp.append(p)
            if len(lists) % 2 == 1:
                tmp.append(lists[-1])
            lists = tmp
        return lists[0]
	# 这个是两链表排序的程序
    def mergeTwoList(self, l1, l2):
        if l1 is None:
            return l2
        if l2 is None:
            return l1
        res = ListNode(0)
        r = res
        while l1 is not None and l2 is not None:
            if l1.val <= l2.val:
                r.next = l1
                l1 = l1.next
            else:
                r.next = l2
                l2 = l2.next
            r = r.next
        if l1 is not None:
            r.next = l1
        if l2 is not None:
            r.next = l2
        return res.next

```

