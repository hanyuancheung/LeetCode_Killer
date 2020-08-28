# LeetCode 第 33 号问题：搜索旋转排序数组

**from Wilfred**

题目来源于 LeetCode 上第 33 号问题：搜索旋转排序数组。题目难度为 Medium，目前通过率为 38.7%。

### 题目描述

假设按照升序排序的数组在预先未知的某个点上进行了旋转。

( 例如，数组 `[0,1,2,4,5,6,7]` 可能变为 `[4,5,6,7,0,1,2]` )。

搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。

你可以假设数组中不存在重复的元素。

你的算法时间复杂度必须是 `O(log n)` 级别。

**示例1:**

```
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
```

**示例2:**

```
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1
```

### 题目解析

首先理解题目的定义，“旋转排序数组”其实就是经过**平移**的有序数组，即起点索引不一定为 `0`。所以我们将算法分为两部分：第一部分是找起点，根据起点将数组分为两个有序数组，这是对数组的第一次二分；第二部分是找元素，在上一步的基础上使用最简单的二分算法即可。注意题目中要求总的时间复杂度为 `O(log n)`，找元素的算法非常简单，使用二分查找就可以，这题的关键在于设计一个时间复杂度不超过 `O(log n)` 的算法来查找起点，这里我们依旧使用二分的思想：

1. 如果起点索引不为 `0`，则数组中第一个元素一定大于最后一个元素（不重复）
2. 如果中点大于第一个元素，则起点在中点右侧，反之在左侧
3. 如果一个元素小于左侧的元素，则该元素为起点

根据 1 可以发现没有旋转的情况，根据 2 确定了二分的规则，根据 3 确定如何找到起点。


### 代码实现

```java
class Solution {
    public int search(int[] nums, int target) {
        int start = searchStart(nums);
        if (start == 0)
            return searchHelp(nums, target, 0, nums.length - 1);
        else if (nums[0] <= target)
            return searchHelp(nums, target, 0, start - 1);
        else
            return searchHelp(nums, target, start, nums.length - 1);
    }

    public int searchStart(int[] nums) {
        int len = nums.length;
        int l = 0, r = len - 1;
        if (nums[l] <= nums[r]) // 如果起点索引不为 0，则数组中第一个元素一定大于最后一个元素
            return 0
        while (l <= r) {
            int mid = (l + r) / 2;
            if (mid == 0) { // 中点为 0 会越界，直接跳过（该情况下中点不可能为起点，否则已经在循环外筛选出来）
                l = mid + 1;
                continue;
            }
            if (nums[mid - 1] > nums[mid]) // 如果一个元素小于左侧的元素，则该元素为起点
                return mid;
            if (nums[0] > nums[mid]) // 如果中点大于第一个元素，则起点在中点右侧，反之在左侧
                r = mid - 1;
            else
                l = mid + 1;
        }
        return -1; // 合格的旋转排序数组不可能运行到这里
    }

    public int searchHelp(int[] nums, int target, int left, int right) {
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else
                right = mid - 1;
        }
        return -1;
    }
}
```
