# LeetCode 第 1431 号问题：拥有最多糖果的孩子

**from zhanghanyuan**

题目来源于 LeetCode 上第 1431 号问题：拥有最多糖果的孩子。题目难度为 easy

### 题目描述

给你一个数组 candies 和一个整数 extraCandies ，其中 candies[i] 代表第 i 个孩子拥有的糖果数目。

对每一个孩子，检查是否存在一种方案，将额外的 extraCandies 个糖果分配给孩子们之后，此孩子有 最多 的糖果。注意，允许有多个孩子同时拥有 最多 的糖果数目。


**示例1:**

```
输入：candies = [2,3,5,1,3], extraCandies = 3
输出：[true,true,true,false,true] 
解释：
孩子 1 有 2 个糖果，如果他得到所有额外的糖果（3个），那么他总共有 5 个糖果，他将成为拥有最多糖果的孩子。
孩子 2 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
孩子 3 有 5 个糖果，他已经是拥有最多糖果的孩子。
孩子 4 有 1 个糖果，即使他得到所有额外的糖果，他也只有 4 个糖果，无法成为拥有糖果最多的孩子。
孩子 5 有 3 个糖果，如果他得到至少 2 个额外糖果，那么他将成为拥有最多糖果的孩子。
```

**示例2:**

```
输入：candies = [4,2,1,1,2], extraCandies = 1
输出：[true,false,false,false,false] 
解释：只有 1 个额外糖果，所以不管额外糖果给谁，只有孩子 1 可以成为拥有糖果最多的孩子。
```

### 题目解析1

1. 按照题目要求，我们希望某一个小朋友获得的糖果最多、或者是最多之一，那么最优的方案是：将所有额外的糖果全部分给这个小朋友；

2. 因此，我们需要将所有的额外糖果分配给这个小朋友，然后用O(n)时间复杂度来遍历所有小朋友，比较他们之间的差值是否小于等于额外糖果的数量。

### 代码实现

暴力法枚举：
```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        int n = candies.length;
        int maxsize = 0;
        for (int i = 0;i < n;i++){
            maxsize = Math.max(maxsize, candies[i]);
        }
        List<Boolean> ret = new ArrayList<Boolean>(n);
        for (int i = 0;i < n;i++){
            ret.add(candies[i] + extraCandies >= maxsize);
        }
        return ret;
    }
}
```

### 结果描述

执行用时：1 ms, 在所有 Java 提交中击败了99.94%的用户

内存消耗：39.6 MB, 在所有 Java 提交中击败了90.19%的用户

