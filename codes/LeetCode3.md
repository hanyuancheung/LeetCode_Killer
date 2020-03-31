# LeetCode 第 3 号问题：无重复字符的最长子串

**from Chester_zhy**

题目来源于 LeetCode 上第 3 号问题：无重复字符的最长子串。题目难度为 Medium。

### 题目描述

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

**示例:**

```
输入："abcabcbb"

输出：3

原因：因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

### 题目解析1

1. 使用暴力求解法进行求解该问题，时间复杂度为O(n^3)；

2. 循环嵌套，第一次循环为定义头元素，将后面的每个元组与之比较；

3. 第二层循环为向后遍历，每找到下一个元素，都取得该元素和头元素之间的若干个数组元素，进行第三重比较，检查中间元素中是否含有相同的元素；

4. 第三重循环将中间的每个元素依次存入一个集合中，同时判断当前元素是否存在于集合set中，如果存在则返回false；否则一直遍历结束。


### 代码实现

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int ans = 0;

        for (int i = 0; i < n; i++)
            for (int j = i + 1; j <= n; j++)
                if (allUnique(s, i, j)) 
                    ans = Math.max(ans, j - i);
        return ans;
    }

    public boolean allUnique(String s, int start, int end) {
        Set<Character> set = new HashSet<>();

        for (int i = start; i < end; i++) {
            Character ch = s.charAt(i);
            if (set.contains(ch)) return false;
            set.add(ch);
        }
        return true;
    }
}

```

### 题目解析2

> 滑动窗口

1. 什么是滑动窗口？

其实就是一个队列,比如例题中的 abcabcbb，进入这个队列（窗口）为 abc 满足题目要求，当再进入 a，队列变成了 abca，这时候不满足要求。所以，我们要移动这个队列！

2. 如何移动？

始终保持队列的最大长度移动，如果下一个元素存在于队列中，则将最左端的元素移除以保持当前最大长度；如果不存在，则将下一个元素加入队列中，重新给max当前最大值。

我们只要把队列的左边的元素移出就行了，直到满足题目要求！

一直维持这样的队列，找出队列出现最长的长度时候，求出解！

3. 时间复杂度：O(n)


### 代码实现

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max = 0;    //max是队列的最大长度
        int left = 0;   //left是队列最左端的元素

        for(int i = 0; i < s.length(); i ++){
            if(map.containsKey(s.charAt(i))){
                left = Math.max(left,map.get(s.charAt(i)) + 1);
            }
        
            map.put(s.charAt(i),i);
            max = Math.max(max,i-left+1);   //始终保持队列的最大长度移动
        }
        return max;
        
    }
}

```