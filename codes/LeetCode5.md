# LeetCode 5.最长回文子串

From Wilfred.

题目源自 LeetCode 上第 5 号问题，难度为 Medium，目前通过率 29.1%。

### 题目描述

给定一个字符串 `s`，找到 `s` 中最长的回文子串。你可以假设 `s` 的最大长度为 `1000`。

**示例1:**

```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

**示例1:**

```
输入: "cbbd"
输出: "bb"
```

### 题目解析

回文子串首先具有的特性就是**对称性**，那么我们判断回文子串就可以从中心开始向两边延伸，比较是否对称。

为了找出最长回文子串，我们需要遍历所有情况，直接暴力求解。

### Java 实现

```java
class Solution {
    public String longestPalindrome(String s) {
        int len = s.length();
        if (len == 0) return "";
        int max = 1, maxl = 0, maxr = 0;// 记录最长回文子串的长度及其始末位置
        int left = 0, right = 0;// 回文子串的始末位置
        while(right+1 < len) {
            left = right;
            // 贪心策略，连续的重复字符视为一个整体，作为回文子串的中心
            while (right+1 < len && s.charAt(left) == s.charAt(right+1)) ++right;
            // 记录下一个中心
            // 以连续字符中任何一点开始搜索回文子串，其都不可能长于连续字符的总长度
            // 所以下一个中心直接跳过所有连续字符
            int nextRadius = right+1;
            // 尽可能多地增加长度
            while (peek(s, left, right)) {
                --left;
                ++right;
            }
            if (max < right-left+1) {
                max = right-left+1;
                maxl = left;
                maxr = right;
            }
            right = nextRadius;
            if (left == right) ++right;
        }
        return s.substring(maxl, maxr+1);
    }

    /**
     * 用于判断是否对称
     */
    private boolean peek(String s, int left, int right) {
        if (left-1 < 0 || right+1 >= s.length()) return false;  // 下标越界
        return s.charAt(left-1) == s.charAt(right+1);
    }
}
```

#### 复杂度分析

时间复杂度：$ O(n^{2}) $
空间复杂度：$ O(1) $
