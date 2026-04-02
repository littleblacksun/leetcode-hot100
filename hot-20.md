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

```

<img width="1520" height="670" alt="image" src="https://github.com/user-attachments/assets/7ecc871a-c4ce-4f5b-ac95-46404d5c4df8" />

### 数学原理
设：
- 头节点到环入口距离为 L
- 环入口到相遇点距离为 x
- 环的周长为 c

相遇时：
- slow 走过路程：L + x
- fast 走过路程：L + x + n·c
- 因为 fast 速度是 slow 的2倍，故 2(L+x) = L+x + n·c
- 化简得：L = n·c − x

这表示：**从头节点到环入口的距离 = 从相遇点走到环入口的距离**。
因此两指针同步走1步，必然在环入口相遇。

<img width="1545" height="669" alt="image" src="https://github.com/user-attachments/assets/d1345103-b8f4-4ffe-a1a9-66d0f75e531b" />

