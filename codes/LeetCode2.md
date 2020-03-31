# LeetCode 第 1 号问题：两数相加

**from Chester_zhy**

题目来源于 LeetCode 上第 2 号问题：两数相加。题目难度为 Medium，目前通过率为 37.0% 。

### 题目描述

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照**逆序**的方式存储的，并且它们的每个节点只能存储**一位**数字。

如果我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字**0**之外，这两个数都不会以**0**开头。


**示例:**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)

输出：7 -> 0 -> 8

原因：342 + 465 = 807
```

### 题目解析

我们使用变量来跟踪进位，并从包含最低有效位的表头开始模拟逐位相加的过程。

![LeetCode2](https://github.com/zhyChesterCheung/LeetCode_Killer/blob/master/images/LeetCode2.svg)

1. 这个加法计算和我们在纸上的计算方法是相似的，都是从最低位开始相加，如果同位数相加之和>10，就需要进行进位操作，即向更高位进位1，比如：**4+8=12 > 10**，就必须往更高位数上进1.因此，我们在这里定义一个进位变量carry，carry的值必为0或1；同时将carry变量放入循环中进行迭代。

2. 在开始之前，考虑到可能l1和l2全部为空的情况，此时只需把当前l1和l2的节点赋为0即可。

3. 同时，每次进行遍历的时，还要考虑到可能会有节点为空的现象，此时我们需要将当前节点的值值赋为0即可。

**注意：**

小心关于节点的定义，在迭代赋值的时候注意创建新的ListNode节点，否则会出现空指针错误！

### 伪代码

+ 将当前结点初始化为返回列表的哑结点。
+ 将进位 carry 初始化为 0。
+ 将 p 和 q 分别初始化为列表 l1 和 l2 的头部。
+ 遍历列表 l1 和 l2 直至到达它们的尾端。
+ 将 xx 设为结点 p 的值。如果 p 已经到达 l1 的末尾，则将其值设置为 0。
+ 将 yy 设为结点 q 的值。如果 q 已经到达 l2 的末尾，则将其值设置为 0。
+ 设定 sum = x + y + carry。
+ 更新进位的值，carry = sum / 10。
+ 创建一个数值为 (summod10) 的新结点，并将其设置为当前结点的下一个结点，然后将当前结点前进到下一个结点。
+ 同时，将 p 和 q 前进到下一个结点。
+ 检查 carry = 1 是否成立，如果成立，则向返回列表追加一个含有数字 1 的新结点。
+ 返回哑结点的下一个结点。

### 代码实现

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode curr = new ListNode(0);
        ListNode ret = curr;

        int carry = 0;
        int x = 0;
        int y = 0;

        while(l1 != null || l2 != null){
            if (l1 != null){
                x = l1.val;
            }else{
                x = 0;
            }
            if (l2 != null){
                y = l2.val;
            }else{
                y = 0;
            }

            int a = x + y + carry;
            ret.next = new ListNode(a % 10);
            carry = a / 10;

            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
            ret = ret.next;
        }
        if (carry != 0){
            ret.next = new ListNode(carry);
        }
        return curr.next;
    }
}
```
