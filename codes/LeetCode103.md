# LeetCode 第 103 号问题：二叉树的锯齿形层次遍历

**from Wilfred**

题目来源于 LeetCode 上第 103 号问题：二叉树的锯齿形层次遍历。题目难度为 Medium，目前通过率为 54.9%。

### 题目描述

给定一个二叉树，返回其节点值的锯齿形层次遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

**示例1:**

给定二叉树 `[3,9,20,null,null,15,7]`：

```
    3
   / \
  9  20
    /  \
   15   7
```

返回锯齿形层次遍历如下：

```
[
  [3],
  [20,9],
  [15,7]
]
```

### 题目解析

这题难度并不高，但是很有意思。从直觉上来讲，它其实就是一个树的层序遍历（BFS），只不过在偶数层倒一下，而且题目要求返回 List，没有限制具体实现，所以我们的解决方案有很多：

1. 标准 BFS 遍历，最后把结果数组部分翻转一下
2. BFS 使用优先队列辅助遍历，插入 LinkedList，奇数层插尾，偶数层插头
3. BFS 使用优先队列辅助遍历，每层使用一个双端队列辅助翻转，最后从一侧取出
4. 使用 DFS 实现（这是官方给的一个方法，太过花哨，没啥意义）

最后是我个人实现的一种解法，一开始是企图使用双端队列，但是插入判断一时没捋清楚，突然发现**先遍历**的节点的子节点在下一层会**后遍历**到，典型的 LIFO，就采用了栈来实现，而且符合直觉，不用去纠结双端队列插入时的各种情况（我思考的双端队列没有使用优先队列辅助遍历，较为复杂）。


### 代码实现

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.Stack;
import java.util.ArrayList;

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        int level = 0;
        Stack<TreeNode> st1 = new Stack<>(), st2 = new Stack<>();
        List<List<Integer>> result = new ArrayList<>();
        if (root == null)
            return result;
        st1.push(root);
        while (!st1.empty() || !st2.empty()) {
            result.add(new ArrayList<>());
            if (st1.empty()) {
                while (!st2.empty()) {
                    TreeNode tmpNode = st2.pop();
                    if (tmpNode.right != null)
                        st1.push(tmpNode.right);
                    if (tmpNode.left != null)
                        st1.push(tmpNode.left);
                    result.get(level).add(tmpNode.val);
                }
            } else {
                while (!st1.empty()) {
                    TreeNode tmpNode = st1.pop();
                    if (tmpNode.left != null)
                        st2.push(tmpNode.left);
                    if (tmpNode.right != null)
                        st2.push(tmpNode.right);
                    result.get(level).add(tmpNode.val);
                }
            }
            ++level;
        }
        return result;
    }
}
```
