#### Leetcode133场周赛1029. 两地调度

公司计划面试 `2N` 人。第 `i` 人飞往 `A` 市的费用为 `costs[i][0]`，飞往 `B` 市的费用为 `costs[i][1]`。

返回将每个人都飞到某座城市的最低费用，要求每个城市都有 `N` 人抵达**。**

 

**示例：**

```
输入：[[10,20],[30,200],[400,50],[30,20]]
输出：110
解释：
第一个人去 A 市，费用为 10。
第二个人去 A 市，费用为 30。
第三个人去 B 市，费用为 50。
第四个人去 B 市，费用为 20。

最低总费用为 10 + 30 + 50 + 20 = 110，每个城市都有一半的人在面试。
```

 

**提示：**

1. `1 <= costs.length <= 100`
2. `costs.length` 为偶数
3. `1 <= costs[i][0], costs[i][1] <= 1000`

思路：

​	这题是第一次遇到这种类型的题目，求最小值的问题。

​	一开始是想到使用贪心算法来求解，但是没有找到关键的点，所以查找了网络上的思路。

​	贪心算法就是我每一步都选择当前情况下最好的选择。

​	按照网络上的思路，量化最好的选择时通过决定两地费用绝对值最大的人去哪里的费用，因为选择这种情况我们是最好的减少费用的方法，到最后剩下的一些费用相差不多的选择哪里就没有关系了。

第一版：

```python
class Solution(object):
    def twoCitySchedCost(self, costs):
        """
        :type costs: List[List[int]]
        :rtype: int
        """
        length = len(costs)
        temp, a_times, b_times = 0, 0, 0
        # 按照绝对值排序，使用贪心算法
        costs = sorted(costs, key= lambda x : abs(x[0] - x[1]))
        # 使列表倒序，由大到小排列
        costs = costs[::-1]
        # 当a或b出现满人的情况下跳出循环
        while b_times < length/2 and a_times < length/2:
            # 提取出绝对值最大的一组进行判断
            cost_a, cost_b = costs.pop(0)
            if cost_a > cost_b and a_times < length/2:
                temp += cost_b
                b_times += 1
            elif cost_a <= cost_b and b_times < length/2:
                temp += cost_a
                a_times += 1
        # 当满人的话则将剩下的全部分配到另外的城。
        if a_times == length/2:
            while costs:
                temp += costs.pop()[1]
        if b_times == length/2:
            while costs:
                temp += costs.pop()[0]

        return temp
```

