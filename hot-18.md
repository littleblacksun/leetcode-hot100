<img width="1049" height="1093" alt="image" src="https://github.com/user-attachments/assets/41898eb5-bada-4abd-97a0-5320aac77424" />


# LeetCode 234. 回文链表 题解笔记
## 题目描述
给你单链表的头节点 `head` ，判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false`。
- 进阶：用 O(n) 时间复杂度、O(1) 空间复杂度解决

## 解题思路
核心思路：**快慢指针找中点 + 反转后半段链表 + 双指针比较**
1. **找中点**：使用快慢指针（慢指针走1步，快指针走2步），找到链表的中间节点，将链表分为前后两段
2. **反转后半段**：从中间节点开始，反转后半段链表
3. **逐一比较**：用两个指针分别遍历前半段和反转后的后半段，节点值全部相等则为回文链表

该方案满足**O(n)时间复杂度**、**O(1)空间复杂度**的进阶要求，是最优解法。

---

## 完整带注释代码
```java
/**
 * Definition for singly-linked list.
 * 单链表节点定义
 * public class ListNode {
 *     int val;        // 节点存储的值
 *     ListNode next;  // 指向下一个节点的引用
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    /**
     * 判断链表是否为回文链表
     * @param head 链表头节点
     * @return 是回文返回true，否则返回false
     */
    public boolean isPalindrome(ListNode head) {
        // 1. 找到链表的中间节点
        ListNode mid = middleNode(head);
        // 2. 反转从中间节点开始的后半段链表
        ListNode head2 = reverseList(mid);

        // 3. 双指针遍历：前半段(head) 和 反转后的后半段(head2) 逐一比较
        while(head2 != null){
            // 只要有一个节点值不相等，就不是回文链表
            if(head.val != head2.val){
                return false;
            }
            // 两个指针同时向后移动
            head = head.next;
            head2 = head2.next;
        }
        // 所有节点都匹配完成，是回文链表
        return true;
    }

    /**
     * 快慢指针法：查找链表的中间节点
     * 偶数长度：返回后半段第一个节点；奇数长度：返回正中间节点
     * @param head 链表头节点
     * @return 中间节点
     */
    private ListNode middleNode(ListNode head){
        ListNode slow = head;  // 慢指针：每次走1步
        ListNode fast = head;  // 快指针：每次走2步

        // 快指针走到链表末尾时，慢指针恰好指向中点
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    /**
     * 反转以head为头节点的链表
     * @param head 待反转链表的头节点
     * @return 反转后链表的新头节点
     */
    private ListNode reverseList(ListNode head){
        ListNode pre = null;   // 前驱节点：初始为null
        ListNode cur = head;   // 当前节点：初始为头节点

        // 遍历整个链表，逐个反转节点指向
        while(cur != null){
            ListNode nxt = cur.next;  // 保存当前节点的下一个节点（防止断链）
            cur.next = pre;          // 核心：反转当前节点的指向，指向前驱节点
            pre = cur;               // 前驱节点后移
            cur = nxt;               // 当前节点后移
        }
        // 遍历结束后，pre就是反转后的新头节点
        return pre;
    }
}
```

---

## 代码细节解析
### 1. 核心方法调用逻辑
```java
ListNode mid = middleNode(head);   // 定位后半段起点
ListNode head2 = reverseList(mid); // 反转后半段
```
- 这两行是核心：先切分链表，再反转后半段，为后续比较做准备

### 2. 快慢指针找中点原理
- 快指针速度是慢指针的2倍，遍历结束时，慢指针精准指向链表中点
- 偶数链表：`[1,2,2,1]` → 中点指向**第二个2**
- 奇数链表：`[1,2,3,2,1]` → 中点指向**3**

### 3. 链表反转原理
- 迭代反转，不使用额外空间，仅修改节点指向
- 输入：`2 → 1` → 输出：`1 → 2`
- 输入：`3 → 2 → 1` → 输出：`1 → 2 → 3`

### 4. 双指针比较逻辑
- 用原始头节点遍历前半段，用反转后的头节点遍历后半段
- 后半段长度 ≤ 前半段，因此以 `head2 != null` 为循环条件
- 任意节点不相等直接返回`false`，遍历完成则返回`true`

---

## 复杂度分析
- **时间复杂度**：O(n)
  找中点、反转、比较均只遍历一次链表，总操作数与链表长度成正比
- **空间复杂度**：O(1)
  仅使用常数个临时变量，无额外数据结构开销
