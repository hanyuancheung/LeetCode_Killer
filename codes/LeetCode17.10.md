# LeetCode 第 17.10 号面试问题：主要元素

**from zhanghanyuan**

题目来源于 LeetCode 上第 17.10 号面试问题：主要元素。题目难度为 easy。

### 题目描述

数组中占比超过一半的元素称之为主要元素。给定一个整数数组，找到它的主要元素。若没有，返回-1。

**示例1:**

```
输入：[1,2,5,9,5,9,5,5,5]
输出：5
```

**示例1:**

```
输入：[3,2]
输出：-1
```

### 说明

你有办法在时间复杂度为 O(N)，空间复杂度为 O(1) 内完成吗？

### 题目解析

1. 新建 Hashmap

2. 遍历nums，将nums中每个元素出现的次数进行统计

3. 遍历map，若某个数字的出现次数大于nums.length / 2，

4. 返回该元素，否则返回-1


### 代码实现

```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int i:nums) {
            int count  = map.getOrDefault(i, 0) + 1;            //getOrDefault，若map中有该元素则返回个数，否则设置default值为0
            map.put(i, count);
        }
        for (Map.Entry<Integer, Integer> entry:map.entrySet()){ //entry是按照map中的键值对进行返回
            if (entry.getValue() > nums.length / 2){
                return entry.getKey();
            }
        }
        return -1;
    }
}
```

### 结果描述

执行用时：14 ms, 在所有 Java 提交中击败了27.78%的用户

内存消耗：44 MB, 在所有 Java 提交中击败了14.32%的用户