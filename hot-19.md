<img width="1266" height="993" alt="image" src="https://github.com/user-attachments/assets/76022cbf-7f70-49de-aa05-79ca45ef8abd" />


# LeetCode 141. 环形链表
## 题目
给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。

## 解法：快慢指针法（龟兔赛跑）
### 算法思路
- 定义两个指针：慢指针slow（每次走1步）、快指针fast（每次走2步）
- 有环：快指针一定会在环内追上慢指针（相遇）
- 无环：快指针会走到链表末尾，退出循环

### 复杂度分析
- 时间复杂度：O(n)，n为链表节点数
- 空间复杂度：O(1)，仅使用两个指针，无额外空间

### 解题代码
```java
class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        // 防止空指针异常
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            // 指针相遇，存在环
            if (fast == slow) {
                return true;
            }
        }
        // 遍历到链表末尾，无环
        return false;
    }
}
