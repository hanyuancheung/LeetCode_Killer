# LeetCode 第 1512 号问题：好数对的数目

**from zhanghanyuan**

题目来源于 LeetCode 上第 1512 号问题：好数对的数目。题目难度为 easy。

### 题目描述

给你一个整数数组 nums 。

如果一组数字 (i,j) 满足 nums[i] == nums[j] 且 i < j ，就可以认为这是一组 好数对 。

返回好数对的数目。


**示例1:**

```
输入：nums = [1,2,3,1,1,3]
输出：4
解释：有 4 组好数对，分别是 (0,3), (0,4), (3,4), (2,5) ，下标从 0 开始
```

**示例1:**

```
输入：nums = [1,1,1,1]
输出：6
解释：数组中的每组数字都是好数对
```

### 题目解析1：暴力求解

看到这道题，最简单的方法就是直接进行暴力求解，O(n^2)循环遍历所有 i < j 的情况，再对 i 和 j 位置的nums进行比较，最后即可得出结果；

时间复杂度：O(n^2)

空间复杂度：O(1)

### 代码实现

```java
class Solution {
    public int numIdenticalPairs(int[] nums) {
        int ret = 0;
        for (int i = 0;i < nums.length;i++){
            for (int j = i + 1;j < nums.length;j++){
                if (nums[i] == nums[j]){
                    ret++;
                }
            }
        }
        return ret;
    }
}
```

### 结果描述

执行用时：2 ms, 在所有 Java 提交中击败了5.97%的用户

内存消耗：36.9 MB, 在所有 Java 提交中击败了85.27%的用户


### 题目解析2：组合计数

