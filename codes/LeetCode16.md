# LeetCode 第 16 号问题：最接近的三数之和

**from zhanghanyuan**

题目来源于 LeetCode 上第 16 号问题：最接近的三数之和。题目难度为 Medium，目前通过率为 45.78% 。

### 题目描述

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

**示例1:**

```
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。
```

**提示:**

```
3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4
```

### 题目解析

本题与 15. 三数之和 非常类似，可以使用「双指针」的方法来解决。

题目要求找到与目标值 \textit{target}target 最接近的三元组，这里的「最接近」即为差值的绝对值最小。我们可以考虑直接使用三重循环枚举三元组，找出与目标值最接近的作为答案，时间复杂度为 O(N^3)。

那么如何进行优化呢？我们首先考虑枚举第一个元素 aa，对于剩下的两个元素 b 和 c，我们希望它们的和最接近 target - a。对于 b 和 c，如果它们在原数组中枚举的范围（既包括下标的范围，也包括元素值的范围）没有任何规律可言，那么我们还是只能使用两重循环来枚举所有的可能情况。

借助双指针，我们就可以对枚举的过程进行优化。我们用 p_b 和 p_c 分别表示指向 b 和 c 的指针，初始时，p_b 指向位置 i+1 ，即左边界；p_c 指向位置 n-1n−1，即右边界。在每一步枚举的过程中，我们用 a+b+ca+b+c 来更新答案，并且：

- 如果 a+b+c ≥ target，那么就将 p_c 向左移动一个位置；

- 如果 a+b+c < target，那么就将 p_b 向右移动一个位置。


### 代码实现

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int n = nums.length;
        int ans = 100000000;
        
        for (int first = 0;first < n;first ++){
            int third = n - 1;
            int second = first + 1;
            
            if (first > 0 && nums[first] == nums[first - 1]){
                continue;
            }
            while(second < third){
                int temp = nums[first] + nums[second] + nums[third];
                if (Math.abs(target - ans) > Math.abs(target - temp)){
                    ans = temp;
                }
                if (temp == target){
                    return target;
                }
                if (temp < target){
                    second++;
                    while(second < third && nums[second] == nums[second - 1]){
                        second++;
                    }
                }
                if (temp > target){
                    third--;
                    while(second < third && nums[third] == nums[third + 1]){
                        third--;
                    }
                }
                
            }
        }
        return ans;
    }
}
```

### 结果描述

执行用时：6 ms, 在所有 Java 提交中击败了85.69%的用户

内存消耗：39.3 MB, 在所有 Java 提交中击败了72.07%的用户