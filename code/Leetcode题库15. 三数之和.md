#### Leetcode题库15. 三数之和

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

```
例如, 给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

思路：

​	这题是参照网络上的方法写出来的，一开始并没有什么想法。

​	1.根据题目要求，需要判断不重复的三元组，**不重复的通用解决方法就是采用排序的方法解决**，通过对数组排序，避免重复情况出现。

​	根据网络上的方法，首先是构建字典，存储列表中每个数字出现的次数，并依据字典的键值生成不含有重复数字的列表。

​	然后对列表进行排序，并从中选取三数之和。

​	2.选取三数之和a + b + c = 0 可以转换成，通过a + b = -c 的问题，通过选取前两个数即可判断第三个数。

​	选取规则如下：

​	1.首先若出现元素0至少3次以上，直接添加[0,0,0]

​	2.其次进行第一次遍历，从小到大选取列表的值

​	3.由于数组无重复数字并且顺序排列的，所以为了避免重复，直接将小于第一层循环中的值剔除，组成新的列表进行第二层循环。

​	4.首先获得两个值后，判断是否存在组成[1,1,2]，[1,2,2]的可能性，若有则加入final数组。

​	5.继续判断3数不相同的情况[1,2,3]，基于第二层循环的值选择。

```python
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums_time = {}
        final = list()
        for num in nums:
            # 建立字典，存储每个值出现的次数
            nums_time[num] = nums_time.get(num, 0) + 1
        if 0 in nums_time and nums_time[0] >= 3:
            final.append([0, 0, 0])
        # 将字典键值拆分组成列表并排列
        nums = sorted(list(nums_time.keys()))
        for pst_0,item_0 in enumerate(nums):
            # item_1 要比 item_0大
            for item_1 in nums[pst_0 + 1:]:
                if item_0 * 2 + item_1 == 0 and nums_time[item_0] >= 2:
                    final.append([item_0,item_0,item_1])
                if item_1 * 2 + item_0 == 0 and nums_time[item_1] >= 2:
                    final.append([item_0, item_1, item_1])
                item_2 = 0 - item_0 - item_1
                # item_2 要比 item_1大
                # list和字典都有in,但是使用字典快，使用list会超时
                if item_2 > item_1 and item_2 in nums_time:
                    final.append([item_0, item_1, item_2])

        return final
```

2019.4.12更新

​	由于考虑到通用性，如n数相加的情况，上述的方法就会失效(如果愿意把所有的情况都考虑到的话则另说)，所以要使用一种更加通用的方法。

​	考虑直接通过去重的方式进行三数之和的判断(参照网络上的方法)。

​	1.对数组进行排列，可得到一个从小到大的数组。

​	2.第一层遍历，从数组中顺序抽取一个元素，由于列表经过排序，所以若该元素是第一个出现的话，它的前一位必定不同，如果前一位相同，说明这种情况考虑过了，直接跳过进行下一个元素判断。

​	3.当第一层循环抽取元素之后，可以对第二层循环进行抽取。由于需要不重复的数组，所以将位于第一层循环中抽取的元素前的元素舍弃(不考虑它们)，并考虑接下来的两数之和的问题。

​	4.两数和问题就考虑两个指针，一个在头，一个在尾，向中间靠拢。由于需要考虑不重复的情况，我们应该先判断两数之和是否符合，若小于或者大于，则分别移动首尾指针，若相等的话，需要做以下的一些处理

​	(1)将相等的情况存储到最终数组(这是第一次出现，所以可能不重复)

​	(2)若头指针指向的元素和下一个元素相同，则将指针指向下一个元素，并继续判断(去重复的步骤)

​	(3)尾指针也是做相同的处理

​	(4)去重处理完之后，则将头指针指向下一个元素，尾指针指向前一个元素，继续寻找另一个数组。

程序如下

```python
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        target = 0
        nums = sorted(nums)
        final = []
        for pst_0, item_0 in enumerate(nums):
            # 考虑多种情况，当处理到倒数第二个数，则直接跳出循环
            if pst_0 == len(nums) - 2:break
            # 考虑pst_0 == 0的情况
            if pst_0 > 0 and item_0 == nums[pst_0 - 1]: continue 
            # 转换为两数和，设置首尾指针
            twoSum = target - item_0
            les = pst_0 + 1
            big = len(nums) - 1
            while les < big:
				# 判断数组的情况，小于两数和，则首指针相加
                if nums[big] + nums[les] < twoSum:  les += 1
                elif nums[big] + nums[les] > twoSum:  big -= 1
                else:
                    # 对应于4的处理，注意去重的时候注意判断首尾指针的位置，否则会出现错误
                    final.append([item_0, nums[big], nums[les]])
                    while les < big and nums[les] == nums[les + 1]: les += 1
                    while big > les and nums[big] == nums[big - 1]: big -= 1
                    les += 1
                    big -= 1
        return final
```

