# LeetCode 第 15 号问题：三数之和

**from zhanghanyuan**

题目来源于 LeetCode 上第 15 号问题：三数之和。题目难度为 Mediun，目前通过率为 29.07% 。

### 题目描述

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。


**示例1:**

给定数组  `nums = [-1, 0, 1, 2, -1, -4]`，

满足要求的三元组集合为：
```
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

### 题目解析

**排序 + 双指针**

- 看到这道题，首先想到的就是LeetCode开篇的'两数之和'，我们当时是用了一遍哈希的方法，每一步将数据对应的值存进一个hashmap中，然后每次遍历`target - a`，查看这个结果值是否在哈希表中；

- 这里我们最原始的思路应该也是类似的，如果使用暴力求解法，直接使用三重循环枚举三元组，时间复杂度将达到惊人的O(n^3)，这显然是不行的；因此我们必须使用类似的思路进行解题；

- 同时，与两数之和不同的一点是，我们要保证最后结果数据没有重复，比如(a, b, c)和(b, c, a)本质上是一致的，所以我们在最开始要对数组中的数据进行排序，避免数据的重复；

- 如果我们固定了前两重循环枚举到的元素 a 和 b，那么只有唯一的 c 满足 a+b+c=0。当第二重循环往后枚举一个元素 b′时，由于 b' > b，那么满足 a+b'+c'=0 的c'一定有 c' < c，即 c'：在数组中一定出现在 c 的左侧。也就是说，我们可以从小到大枚举 b，同时从大到小枚举 c，即第二重循环和第三重循环实际上是并列的关系。

- 这就是我们说的双指针的方法，来看一下时间复杂度，我们每次移动 b，c的位置都会移动1个或数个，b和c总共移动的次数是n，因此我们整体的时间复杂度就是O(n^2)

时间复杂度：O(n^2)

空间复杂度：O(n)

### 代码实现

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
        for (int first = 0;first < n;++first){
            if (first > 0 && nums[first] == nums[first - 1]){
                continue;
            }
            int third = n - 1;
            int target = -nums[first];
            for (int second = first + 1;second < n ;++second){
                if (second > first + 1 && nums[second] == nums[second - 1]){
                    continue;
                }
                while(second < third && nums[second] + nums[third] > target){
                    third--;
                }
                if (second == third){
                    break;
                }
                if (nums[second] + nums[third] == target){
                    List<Integer> ret = new ArrayList<Integer>();
                    ret.add(nums[first]);
                    ret.add(nums[second]);
                    ret.add(nums[third]);
                    ans.add(ret);
                }
            }
        }
        return ans;
    }
}
```
**注意，每次我们必须保证不同循环中需要和上一次枚举的数不相同**

### 结果描述

执行用时：23 ms, 在所有 Java 提交中击败了83.97%的用户

内存消耗：44.1 MB, 在所有 Java 提交中击败了24.72%的用户