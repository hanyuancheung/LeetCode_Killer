# LeetCode 第 2 号问题：两数相加

> From：BrantShi


题目来源于 LeetCode 上第 2号问题：两数相加。题目难度为 中等，目前通过率为 37.0% 。

### 题目描述

给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例:**

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

### 题目解析

本题考察链表的基本应用——表示大整数及大整数间的加和，需对链表的构造，遍历等有着清晰明确的认识。

由于是逆序存储，因而操作起来更加简单（如果是顺序存储应该如何处理？），可像手算加法时一样，按位进行计算，将两数的每一位和上一位的进位相加和，并将结果存储到新的链表中。

计算时尤其注意考虑容易出错的边界情况（两链表不等长，或最高位相加仍有进位的处理）。

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
        int val_1,val_2,sum,carry = 0;
        ListNode sumListHead = new ListNode(0);//初始化存储计算结果的链表的头节点
        ListNode curr = sumListHead;//创建指针curr用于依次构建该链表
        while(l1!=null || l2!= null){
            val_1 = l1==null?0:l1.val;//三元运算符，若当前节点为空则补0
            val_2 = l2==null?0:l2.val;//否则取其该位整数
            sum = val_1 + val_2+carry;

            carry = sum / 10;
            curr.next = new ListNode(sum%10);
            curr = curr.next;
             
            if(l1!=null) l1 = l1.next;
            if(l2!=null) l2 = l2.next;
        }
        if(carry == 1)				//如果最高位存在进位
            curr.next = new ListNode(carry);
        return sumListHead.next; 	//头节点在初始化时存有无意义值0，故返回时需去除
    }
}
```

