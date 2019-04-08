#### Leetcode题库04-寻找两个有序数组的中位数

给定两个大小为 m 和 n 的有序数组 `nums1` 和 `nums2`。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 `nums1` 和 `nums2` 不会同时为空。

**示例 1:**

```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```

**示例 2:**

```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

​	首先要理解怎么叫两个有序数组的中位数，最简单的意思，将两个数组按照顺序排列成一个数组，数组若为单数，则为[min, i-1]，i，[i+1, max]，这里i为中位数的位置，不是大小！中位数为位置为i的数值

​	数组若为双数，则为[min, i - 1]，[i, max]，中位数为i和i-1位置数值的和的平均数。

思路：

​	1.首先规定了两个有序数组，和一个复杂度，网上对这个复杂度的评价是肯定是使用两个数组分别寻找的方法，所以我在这方面下功夫

​	2.由于他们是有序数组，只需要将两个数组的数由头至尾进行比较并排列，可以组成一个有序的数组，长度是两个数组之和。再获得它们的中位数就简单了。

​	3.为了减少运行时间，可以在进行排列的时候进行长度计数，如果到达中位数的长度的时候即可停止排列，并输出目标值。

代码如下：

```
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        '''
        :type nums1:list[int]
        :type nums2:list[int]
        :rtype :float
        '''
        num = int((len(nums1)+len(nums2))/2)		# 获得中位数的位置（num），类型(case),新数组的总长度(all)
        cas = (len(nums1)+len(nums2))%2
        al = len(nums1)+len(nums2)
        dic = {}								  	# 用字典存储位置和数值信息 
        position = 0								
        for i in range(al):
            if (len(nums1) != 0) & (len(nums2) != 0):	# 排除nums1,nums2没有元素的情况
                a, b = nums1[0], nums2[0]
            elif len(nums1) == 0:
                a = nums2[0] + 1
                b = nums2[0]
            else:
                b = nums1[0] + 1
                a = nums1[0]

            if a <= b:								# 比较大小
                dic[position] = a
                position += 1
                nums1.pop(0)						# 剔除比较过的元素
            else:
                dic[position] = b
                position += 1
                nums2.pop(0)
            if position - 1 == num:					# 到达规定长度即停止
                break
        if cas == 1:								# 判断列表长度奇偶性
            return float(dic[num]) 
        else:
            return float(((dic[num-1] + dic[num])/2))
```

