# LeetCode 第 902 号问题：最大为 N 的数字组合

**from Wilfred**

题目来源于 LeetCode 上第 902 号问题：最大为 N 的数字组合。题目难度为 Hard，目前通过率为 30.1%。

### 题目描述

我们有一组排序的数字 `D`，它是  `{'1','2','3','4','5','6','7','8','9'}` 的非空子集。（请注意，`'0'` 不包括在内。）

现在，我们用这些数字进行组合写数字，想用多少次就用多少次。例如 `D = {'1','3','5'}`，我们可以写出像 `'13', '551', '1351315'` 这样的数字。

返回可以用 `D` 中的数字写出的小于或等于 `N` 的正整数的数目。

**示例1:**

```
输入：D = ["1","3","5","7"], N = 100
输出：20
解释：
可写出的 20 个数字是：
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77.
```

**示例2:**

```
输入：D = ["1","4","9"], N = 1000000000
输出：29523
解释：
我们可以写 3 个一位数字，9 个两位数字，27 个三位数字，
81 个四位数字，243 个五位数字，729 个六位数字，
2187 个七位数字，6561 个八位数字和 19683 个九位数字。
总共，可以使用D中的数字写出 29523 个整数。
```

### 题目解析

这是第一个我不用学习任何新的知识就能做出来的“困难”难度的题目，可喜可贺。

其实这是一道数学题，设 `|D|` 为 `D` 的元素个数，我们可以发现能组合成的长为 `i` 的正整数 `M` 共有 `|D| ^ i` 个，而 `M` 位数小于 `N` 时，`M < N` 恒成立，所以我们只需要考虑 `M, N` 位数相等的情况。另外，当 `M, N` 位数相等时，如果 `M` 的高位小于 `N` 的高位，则低位可以任意排列，如果相等，则 `M` 的低位要小于 `N` 的低位（问题规模缩小了！想必看到这里已经懂了）。设整型数组 `nums`，其中 `nums[i] = |D| ^ i`；设函数 `int search(char c)`，返回 `D` 中小于 `c` 的项的个数，那么我们可以得到一个等式：

`count[i] = nums[i] × search(N[i]), i ∈ [0, |N|)`

这个等式从高位到低位计算，直到 `M` 的高位小于 `N` 的高位就停止计算（后面的 `count` 都为 `0`），然后把所有的 `count` 都加起来，再加上位数小于 `N` 的所有情况，就是最终答案。


### 代码实现

```java
class Solution {
    public int atMostNGivenDigitSet(String[] D, int N) {
        String top = String.valueOf(N);
        int len = top.length(), d = D.length, count = 0;
        int[] nums = new int[len + 1];
        char[] set = new char[d];
        for (int i = 0; i < d; ++i)
            set[i] = D[i].charAt(0);
        nums[0] = 1;
        for (int i = 1; i <= len; ++i)
            nums[i] = nums[i - 1] * d;
        for (int i = len - 1; i >= 0; --i) {
            int tmp = search(set, top.charAt(len - i - 1));
            count += nums[i] * tmp;
            if (tmp < d && set[tmp] == top.charAt(len - i - 1)) { // 高位相等，继续计算 nums[i] × search(N[i])
                count += nums[i]; // 加上位数为 i，且 i < |N| 的情况
                continue;
            }
            // 计算结束，统计剩余的情况
            for (int j = i; j >= 1; --j)
                count += nums[j]; // 加上位数为 i，且 i < |N| 的情况
            break;
        }
        return count;
    }

    public int search(char[] set, char target) {
        for (int i = 0; i < set.length; ++i)
            if (set[i] >= target)
                return i;
        return set.length;
    }
}
```
