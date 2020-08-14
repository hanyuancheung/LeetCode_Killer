# LeetCode 第 4 号问题：寻找两个有序数组的中位数

**from zhanghanyuan**

题目来源于 LeetCode 上第 4 号问题：寻找两个有序数组的中位数。题目难度为 Hard，想起来容易、实现起来不简单，建议亲手写一遍。

### 题目描述

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。


**示例:**

```
输入: nums1 = [1, 3], nums2 = [2]
输出: 则中位数是 2.0

输入: nums1 = [1, 2], nums2 = [3, 4]
输出: 则中位数是 (2 + 3)/2 = 2.5
```

### 题目解析1

1. 最简单的解法：**暴力求解**，遍历全部数组 (m+n)，空间复杂度：开辟了一个数组，保存合并后的两个数组 O(m+n)

2. 先把这两个有序数组按照元素大小整合到一个新的数组内，这里两个有序数组的合并也是归并排序中的一部分，因此时间复杂度为 O(log(m + n)) ；

3. 然后再根据这个新的数组是奇数还是偶数，返回对应的中位数。


### 代码实现

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int [] num = new int[m + n];    //新数组的长度等于两个有序数组之和
        int count = 0;                  //count用作标记指针来指向新数组的当前当前元素
        if (m == 0){
            if (n % 2 == 0){
                return ((nums2[n / 2 - 1] + nums2[n / 2]) / 2.0);
            }else{
                return (nums2[n / 2]);
            }
        }
        if (n == 0){
            if (m % 2 == 0){
                return ((nums1[m / 2 - 1] + nums1[m / 2]) / 2.0);
            }else{
                return (nums1[m / 2]);
            }
        }

        int i = 0, j = 0;               //分别用i和j来指向nums1和nums2两个数组的当前元素，便于比较每个元素的大小
        
        while(count != (m + n)){        //这一部分循环是数组的合并操作
            if (i == m){
                while(j != n){
                    num[count++] = nums2[j++];
                }
                break;                  
            }
            if (j == n){
                while(i != m){
                    num[count++] = nums1[i++];
                }
                break;
            }
            if (nums1[i] < nums2[j]){
                num[count++] = nums1[i++];

            }else{
                num[count++] = nums2[j++];
            }
        }

        if (count % 2 == 0){            //这部分是根据新数组的奇数偶数来确定返回值
            return ((num[count / 2 - 1] + num[count / 2]) / 2.0);
        }else{
            return (num[count / 2]);
        } 
    }
}

```

