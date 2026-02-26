+++
title = "Leetcode 热题 100：哈希"
[cover]
	image = "/images/hash.png"
	relative = false
date = "2026-02-26T00:35:34+08:00"
draft = false

categories = ["算法学习"]
tags = ["哈希表", "Leetcode热题 100"]

+++

开始刷Leetcode热题100了，希望自己能够坚持刷算法题这个习惯，来保证自己的代码能力。目前就只考虑刷算法题吧，如果后面有空的话，还考虑刷一些机器学习的手撕。

## 哈希

先来讲讲关于哈希在编程算法中的概念：

- 抽象的说，哈希指的是将输入转化成唯一输出的一种方法，不同的输入一定会输出不同的内容，相同的输入一定会输出相同的内容
- 具体到算法中，则是对于数组那种O(n)级别的查找和插入的优化，可以将**查找、插入和删除优化到O(1)级别**

其中，有两个重要的概念：

1. **哈希函数**。指的是将输入参数转化成唯一输出的规则，常用的有MD5编码等。在实际的算法题中，往往是需要自己根据题目背景设计一个哈希函数出来。
2. **哈希表**。指的是可供查询和插入的数据结构，在python中使用字典dict数据结构进行使用即可。在使用的过程中如我前面所说，相比于数组，哈希表可以将具体的查找和插入优化到O(1)，在需要优化查找、插入和删除的时候可以优先考虑一下。

## Leetcode热题 100 哈希

关于这一块的内容，热题100中提供了3道题目：

### 1. 两数之和

![twonum](/images/twonum.png)

#### 1.1 示例和数据范围

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i] <= 10^9`
- `-10^9 <= target <= 10^9`
- **只会存在一个有效答案**

#### 1.2 解题思路

- 纯暴力+边界优化。初步看到这道题目的时候，想法就是从头到尾查一遍，对于每个数，查它后面的每个数的和是否满足条件，这样时间复杂度就是O(n^2)，符合题目条件。然后又想到，还能针对边界进行优化，找到数组的最大值和最小值，然后在每次遍历的时候都检查`num + min > target or num + max < target`，可以减少一些情况，发现效果不错，和后续的O(n)算法速度一致（应该是数据原因），代码和效果如下：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        res: List[int] = []
        length = len(nums)
        min_num = min(nums)
        max_num = max(nums)
        for i in range(length):
            if nums[i] + min_num > target or nums[i] + max_num < target:
                continue
            for j in range(i + 1, length):
                if nums[i] + nums[j] == target:
                    res.append(i)
                    res.append(j)
                    return res
```

- 哈希表。进一步思考，发现暴力的整个过程中，如果能将查询数组中是否有相匹配的数这一O(n)的步骤优化到O(1)，则可实现O(n)级别的效果，于是想到了哈希表。逐个遍历整个数组，如果哈希表中没有`target - num`这一数据，则将`<num, i>`元组添加到哈希表中，直到遍历到某一数据在哈希表中找到`target - num`数据，实现O(n)级别的效果，代码如下：

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashtable: dict = {}
        for i, num in enumerate(nums):
            if target - num in hashtable:
                return [hashtable[target - num], i]
            hashtable[num] = i
        return []
```

### 2. 字母异位词分组

![groupAnagrams](/images/groupAnagrams.png)

#### 2.1 示例和数据范围

**示例 1:**

**输入:** strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

**输出:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**解释：**

- 在 strs 中没有字符串可以通过重新排列来形成 `"bat"`。
- 字符串 `"nat"` 和 `"tan"` 是字母异位词，因为它们可以重新排列以形成彼此。
- 字符串 `"ate"` ，`"eat"` 和 `"tea"` 是字母异位词，因为它们可以重新排列以形成彼此。

**示例 2:**

**输入:** strs = [""]

**输出:** [[""]]

**示例 3:**

**输入:** strs = ["a"]

**输出:** [["a"]]

**提示：**

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

#### 2.2 解题思路

这道题和两数之和很类似，但是看数据范围，因为是字符串，所以O(n^2)级别的算法的复杂度还要乘以字符串长度才是结果，所以时间复杂度为`10^4 * 10^4 * 100 = 10^{10} > 10^8`。因此，同样考虑使用哈希表优化查找，但是这里和之前不同，这里需要进行匹配的字符串会有多种顺序，所以这里有两种想法进行优化：

- 重新设计一个哈希函数。通过对a-z每个字符赋予一个随机数，这个随机数的范围设计到（1, 10000），然后一个单词的哈希值由单词中每个字符相加或者相乘得到，这样就可以把字母异位词进行统一查找，时间复杂度为O(Nk)，代码如下:

```python
class Solution:
    def hashfunction(self, s: str, letters_random_dict: dict) -> int:
        hash_value: int = 1
        for letter in s:
            hash_value = hash_value * letters_random_dict[letter]
        return hash_value

    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        letters: List[str] = list(string.ascii_lowercase)
        letters_random_dict: dict = {letter: random.randint(1, 10000) for letter in letters}
        word_dict: dict[List] = {}
        ans: List[List[str]] = []
        for s in strs:
            hash_value = self.hashfunction(s, letters_random_dict)
            if hash_value in word_dict:
                word_dict[hash_value].append(s)
            else:
                word_dict[hash_value] = [s]
        for key in word_dict:
            ans.append(word_dict[key])
        return ans
```

- 统一顺序哈希。通过对单词中字符进行排序，把字母异位词的顺序进行统一，然后再使用哈希表进行查找，时间复杂度为O(Nklogk)，这个方法相对于第一种方法而言一定会通过，第一种方法在部分情况下通过不了，代码如下：

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        word_dict: dict = collections.defaultdict(list)
        for word in strs:
            sorted_word = ''.join(sorted(word))
            word_dict[sorted_word].append(word)
        return list(word_dict.values())
```

### 3. 最长连续序列

![longestConsecutive](/images/longestConsecutive.png)

#### 3.1 示例和数据范围

 **示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

**示例 3：**

```
输入：nums = [1,0,1,2]
输出：3
```

**提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

#### 3.2 解题思路

- 纯暴力。这道题看数据范围，发现可以使用O(nlogn)的方法，于是就直接排序，然后从头到尾遍历整个数组，找到最大连续子序列，时间复杂度为O(NlogN)，代码如下：

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if nums == []:
            return 0
        sorted_nums = sorted(nums)
        longest_streak = 1
        max_streak = 1
        for i in range(1, len(sorted_nums)):
            if sorted_nums[i] == sorted_nums[i - 1] + 1:
                longest_streak += 1
            elif sorted_nums[i] != sorted_nums[i - 1]:
                max_streak = max(max_streak, longest_streak)
                longest_streak = 1
        max_streak = max(max_streak, longest_streak)
        return max_streak
```

- 使用set数据结构进行哈希。同样可以发现，这道题的瓶颈出在查找上，在没有统一顺序的情况下，每次查找都要遍历大部分数据，即O(n)级别，所以也能用哈希来进行优化。同时，考虑到存在重复数据的情况且这道题不需要知道顺序，所以直接使用set数据结构，在去重的基础上进行哈希。然后，查找的时候为了保证一定从最小的数字进行查找，还要添加一个边界就是，小于这个数1的数不在数组中，这样才能避免重复查找。最终代码时间复杂度O(n)，代码如下：

```python
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        longest_streak = 0
        max_streak = 0
        unique_nums = set(nums)
        for num in unique_nums:
            if num - 1 not in unique_nums:
                longest_streak = 1
                while num + 1 in unique_nums:
                    longest_streak += 1
                    num += 1
                max_streak = max(max_streak, longest_streak)
        return max_streak
```

## 总结

最后总结一下，关键还是得知道，哈希函数和哈希表主要都是为了解决数组查找和插入时间复杂度过高的问题，所以当遇到这一类瓶颈的时候可以优先考虑这一内容。同时，也要活用python中的数据结构，像dict、去重的set、默认生成的defaultdict这三类。

