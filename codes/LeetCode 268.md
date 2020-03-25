# LeetCode 268.缺失数字

From Brant

题目源自LeetCode上286号问题，难度为Easy，通过率为54.7%。

笔者近日在腾讯一面时遇到此题，因当时未能想出最简解法而表现丢分。

### 题目描述

给定一个包含 `0, 1, 2, ..., n` 中 *n* 个数的序列，找出 0 .. *n* 中没有出现在序列中的那个数。

**示例 1:**

```
输入: [3,0,1]
输出: 2
```

**示例 2:**

```
输入: [9,6,4,2,3,5,7,0,1]
输出: 8
```

### 题目解析

在面试中遇到此题时一定不能急于做题，要在仔细思考后向面试官询问清问题的一些具体细节如边界条件和对时间空间代价的要求。

可能要求如下：

1. 你的算法应具有线性时间复杂度。你能否仅使用额外常数空间来实现?
2. 边界条件: n <=50;

此外，需注意到此题隐含的信息：序列无序，序列的上限已给出（n)，且缺失数字有且仅有一个。(0~n范围大小为n+1,而共n个数)**

综合以上要求，可采取的题解有以下四种：

#### 排序

如果数组有序，若缺失数字位于序列的首位和末位，则首位或末位必不等于0或n，易判断；此外，如缺失数组位于(0~n)，则缺失数字两侧的数字绝对值相差为2，通过遍历数组找到满足以上条件的情况，即得到缺失数字。

```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);

        // 判断 n 是否出现在末位
        if (nums[nums.length-1] != nums.length) {
            return nums.length;
        }
        // 判断 0 是否出现在首位
        else if (nums[0] != 0) {
            return 0;
        }
        // 此时缺失的数字一定在 (0, n) 中
        for (int i = 1; i < nums.length; i++) {
            int expectedNum = nums[i-1] + 1;
            if (nums[i] != expectedNum) {
                return expectedNum;
            }
        }

        // 未缺失任何数字（保证函数有返回值）
        return -1;
    }
}
```

##### 复杂度分析

* 时间复杂度：至少为O(nlogn)
* 空间复杂度：O(1)或O(n)，取决于排序算法是否申请新空间

#### 哈希表

如果n较小如要求2，则可直接查询每个数是否在数组中出现过来找出缺失的数字。

```python
class Solution:
    def missingNumber(self, nums):
        num_set = set(nums)
        n = len(nums) + 1
        for number in range(n):
            if number not in num_set:
                return number
```

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int a[nums.size()+1];	//哈希表，以
        for(auto &it:a)	//初始化为0
            it=0;
        for(auto &it : nums)//出现过即自增1
            a[it]++;
        for(int i=0;i<len+1;i++)
            if(a[i]==0) return i;//哈希表对应位为0则未出现过
        return 0;

    }
};
```

##### 复杂度分析

* 时间复杂度：O(n)，集合插入，查询操作均为O(1)，执行最多n+1次查询；
* 空间复杂度：O(n)，集合或哈希表的大小

#### 数学公式法

使用高斯求和公式求出0-n的和，减去数组中所有数的和，就得到了缺失的数字。此方法较巧，需要灵活的思路。

必要适用条件：n要求不能过大，若n的边界值较大时采用该方法可能会出现数组越界

```python
class solution:
    def missingNumber(self,nums):
        n = len(nums)
        expectSum = n*(n+1)/2
        actualSum = sum(nums)
        return expectSum - actualSum
```

##### 复杂度分析

* 时间复杂度：O(n)，对序列进行求和操作O(n)，其余均为O(1)
* 空间复杂度：O(1),额外申请O(1)的空间用来存储答案

#### 位运算

位运算：由于异或运算（XOR）满足结合律，并且对一个数进行两次完全相同的异或运算会得到原来的数，因此我们可以通过异或运算找到缺失的数字。

无缺失序列比缺失序列多一个元素，其余所有元素都能在缺失序列中找到相同元素，所以两个序列所有元素按位异或，除了多出来的那个元素，其他所有元素都能找到相同的另一个，两个相同的元素按位异或得到0。整个过程下来，按位异或得到的结果就是缺失元素。

```python
class Solution:
    def missingNumber(self, nums):
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing

```

##### 复杂度分析

* 时间复杂度：O(n)
* 空间复杂度：O(1)