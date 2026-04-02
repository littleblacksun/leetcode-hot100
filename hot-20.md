# LeetCode 142. 环形链表 II

## 题目描述
给定一个链表，返回链表开始入环的第一个节点。如果链表无环，则返回 null。

## 解法：快慢指针法（龟兔赛跑算法）

### 算法思路
1. 定义慢指针 slow（每次走1步）、快指针 fast（每次走2步）
2. 若两指针相遇 → 链表存在环
3. 相遇后，将一个指针指向头节点，两指针**同步每次走1步**
4. 再次相遇的位置，就是**环的入口节点**

### 复杂度分析
- 时间复杂度：O(n)
- 空间复杂度：O(1)（仅使用两个指针，无额外空间）

### 解题代码
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        // 寻找快慢指针相遇点
        while(fast!=null && fast.next!=null)
        {
            fast = fast.next.next;
            slow = slow.next;
            
            // 找到相遇点，开始找环入口
            if(slow == fast){
                while(slow!= head)
                {
                    slow = slow.next;
                    head = head.next;
                }
                return slow;
            }
        }
        // 无环返回 null
        return null;
    }
}
