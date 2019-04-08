#### Leetcode题库11. 盛最多水的容器

给定 *n* 个非负整数 *a*1，*a*2，...，*a*n，每个数代表坐标中的一个点 (*i*, *ai*) 。在坐标内画 *n* 条垂直线，垂直线 *i* 的两个端点分别为 (*i*, *ai*) 和 (*i*, 0)。找出其中的两条线，使得它们与 *x* 轴共同构成的容器可以容纳最多的水。

**说明：**你不能倾斜容器，且 *n* 的值至少为 2。

![1554702811827](G:\typora图片\1554702811827.png)



- 图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

   

  **示例:**

  ```
  输入: [1,8,6,2,5,4,8,3,7]
  输出: 49
  ```

思路：

​	第一种：暴力解法

​		直接将数组遍历，采用两个循环的方式进行计算，计算复杂度高，用Python会超时

​	第二种：贪心算法

​		由于取决于数值低的元素，可以使用头，尾两个指针，每次将两指针小的数值往中间移，知道两指		针位置相同为止，计算复杂度低于第一种方法		

程序如下

方法一：暴力解法

```
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        final = 0
        for position, item in enumerate(height):
            for p1, item1 in enumerate(height):
                if p1 <= position: continue
                low = item if item < item1 else item1
                area = (p1 - position) * low
                final = final if final > area else area
        return final
```

方法二：

```
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        final = 0
        p1 = 0
        p2 = len(height) - 1
        while p2 >= p1:
            low = height[p1] if height[p1] < height[p2] else height[p2]
            area = low * (p2 - p1)
            final = final if final > area else area
            if height[p1] < height[p2] :
                p1 += 1
            else:
                p2 -= 1
        return final
```

