#### Leetcode题库30. 串联所有单词的子串

给定一个字符串 **s** 和一些长度相同的单词 **words。**找出 **s** 中恰好可以由 **words** 中所有单词串联形成的子串的起始位置。

注意子串要与 **words** 中的单词完全匹配，中间不能有其他字符，但不需要考虑 **words** 中单词串联的顺序。

**示例 1：**

```
输入：
  s = "barfoothefoobarman",
  words = ["foo","bar"]
输出：[0,9]
解释：
从索引 0 和 9 开始的子串分别是 "barfoor" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。
```

**示例 2：**

```
输入：
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
输出：[]
```

思路：

​	题目有一个关键的点，单词的长度是相同的，所以遍历列表，判断每个单词长度的单词是否在words里，若存在则继续，若不存在则跳过。按照这种思路，写出了第一版，这一版需求时间太长了，所以在网络上继续找了更好的版本。

第一版：

```python
class Solution(object):
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        if len(words) == 0: return []
        word_num = len(words)
        word_length = len(words[0])
        # 利用一个临时列表，存储words的单词，方便后续的比较操作
        temp = words[:]
        result = []
        # 对字符串中每个字符开始遍历，其中对于剩余小于words的长度的部分直接舍弃
        for i in range(len(s) - word_num * word_length + 1):
            k = i
            # 这个while好像有点多余
            while True:
                j = 0
                # 当出现下列情况的时候才跳出循环，j = temp的长度说明temp里没有符合的值，temp的长度为0是避免temp[j]的错误
                while j != len(temp) and len(temp) != 0:
                    if temp[j] == s[k:k + word_length]:
                        temp.pop(j)
                        j = 0
                        k += word_length
                    else:
                        j += 1
				# 等于0则说明符合，将当前位置加入
                if len(temp) == 0:
                    result.append(i)
				# 重置temp
                temp = words[:]
                break
        return result
```

第二版：

​	参照[博客](https://blog.csdn.net/weixin_41958153/article/details/80813042)的思路，对字符串进行划分并排序的方法进行比较

​	将字符串按照width放入列表中，并通过排序判断两个列表相同的方法

​	当然时间比第一版要短，但是还是很长

```python

class Solution:
    #将一个字符串string按着width的宽度切开放在一个列表中，返回这个列表。
    def split(self,string,width):
        result = []
        i = 0
        length = len(string)
        while i<=length-width:
            result.append(string[i:i+width])
            i = i+width
        return result
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        result = []
        words_count = len(words)
        #判断输入的s和words是否为空，如果不为空，将words中的单词的宽度放在length_word中
        if words_count>0:
            length_word = len(words[0])
        else:
            length_word = 0
        i= 0
        length_s = len(s)
        if length_s == 0 or words_count == 0:#如果s为空或者words为空，返回空的列表
            return []
        #利用while循环，实现对s遍历
        while i <= length_s-length_word*words_count:
            #将s从i开始切分出一个长度和words中所有单词加在一起长度相同的一个子串，并将这个子串切开，放在string_list中
            string_list = self.split(s[i:i+length_word*words_count],length_word)
            #由于words中的单词并不是排好序的，所以这里需要调用两个sort函数，将这两个列表排序，这样才能够判断他们是否相等。
            string_list.sort()
            words.sort()
            #如果不是排好序的列表，即使里面的元素都相等，但是顺序不等的话，也是不会相等的。
            if  string_list == words:
                result.append(i)
            i = i + 1
```

第三版

​	第三版是参照LeetCode上耗时最短的版本，链接无法引用。

```python
class Solution(object):
    def findSubstring(self, s, words):
        """
        :type s: str
        :type words: List[str]
        :rtype: List[int]
        """
        if not s or not words:
            return []
        lens, len_word, len_subs, times = len(s), len(words[0]), 0, {}
        times = {}
        # 通过字典存储字符在words出现的次数
        for word in words:
            len_subs += len_word
            times[word] = times.get(word) + 1 if times.get(word) else 1
        res = []
        # 只需要遍历word长度的情况
        for i in range(len_word):
            start = i
            cur = {}
            # 当超过字符串长度时，则跳出循环
            while i + len_subs <= lens:
                # 将当前的Word提取
                word = s[start:start + len_word]
                start += len_word
                # 若word不存在，则对当前位置加上一个word长度(0, len(word), 2 * len(word)等。
                if word not in times:
                    i = start
                    cur.clear()
                else:
                    if word in cur:
                        cur[word] += 1
                    else:
                        cur[word] = 1
                    # 如果出现当前cur节点词大于tiem的情况，说明当前节点的是错误的， i+=len_word
                    # 这个while用的很精髓，配合i和start两个指针分开使用，无敌。
                    while cur[word] > times[word]:
                        cur[s[i:i + len_word]] -= 1
                        i += len_word
                    # 若words里的word都存在，则将i加入
                    if start - i == len_subs:
                        res.append(i)
        return res
```

