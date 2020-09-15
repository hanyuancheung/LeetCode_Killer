# LeetCode 第 867 号问题：转置矩阵

**from zhanghanyuan**

题目来源于 LeetCode 上第 867 号问题：转置矩阵。题目难度为 easy。

### 题目描述

给定一个矩阵 A， 返回 A 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

**示例1:**

```
输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：[[1,4,7],[2,5,8],[3,6,9]]
```

**示例1:**

```
输入：[[1,2,3],[4,5,6]]
输出：[[1,4],[2,5],[3,6]]
```

**提示:**

1 <= A.length <= 1000
1 <= A[0].length <= 1000

### 题目解析

尺寸大小为 width * height 的矩阵 A 转置后会得到尺寸大小为 height * width 的矩阵，因此会有`array[height][width] = A[width][height]`

最简单直观的做法是直接初始化一个心矩阵，然后依次做数据的搬移

### 代码实现

```java
class Solution {
    public int[][] transpose(int[][] A) {
        int height = A.length, width = A[0].length;
        int[][] array = new int[width][height];
        for (int i = 0;i < height;++i){
            for (int j = 0;j < width;++j){
                array[j][i] = A[i][j];
            }
        }
        return array;
    }
}
```

### 结果描述

执行用时：0 ms, 在所有 Java 提交中击败了100.00%的用户

内存消耗：40.5 MB, 在所有 Java 提交中击败了83.94%的用户