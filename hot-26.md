<img width="967" height="1036" alt="image" src="https://github.com/user-attachments/assets/3846697f-c010-43d1-b089-9b7c4afa3f28" />

# LeetCode 148. 排序链表（归并排序/分治法）笔记
## 题目描述
给你链表的头结点 `head`，请将其按**升序**排列并返回**排序后的链表**，要求时间复杂度 **O(n log n)**，空间复杂度 **O(1)**（递归栈空间不计入）。

## 解题思路
本题核心采用**归并排序（分治法）**，完美适配链表结构：
1. **分（Divide）**：利用快慢指针找到链表中间节点，将链表**拆分为左右两个子链表**，递归拆分直到每个子链表只有一个节点（天然有序）；
2. **治（Merge）**：递归合并两个有序子链表，最终得到完整有序链表。

整体流程：**拆分链表 → 递归排序子链表 → 合并有序链表**

## 完整代码（带注释）
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        // 递归终止条件：链表为空 或 只有一个节点，无需排序
        if (head == null || head.next == null) {
            return head;
        }
        // 1. 找到链表中间节点，并断开前后连接，拆分左右两个子链表
        ListNode head2 = middleNode(head);
        // 2. 分治：递归排序左、右两个子链表
        head = sortList(head);
        head2 = sortList(head2);
        // 3. 合并两个有序子链表，返回排序后的头节点
        return mergeTwoLists(head, head2);
    }

    /**
     * 快慢指针找链表中间节点（LeetCode 876）
     * 同时断开中间节点与前一个节点的连接，完成链表拆分
     */
    private ListNode middleNode(ListNode head) {
        ListNode pre = head;  // 记录慢指针的前一个节点（用于断链）
        ListNode slow = head; // 慢指针：每次走1步
        ListNode fast = head; // 快指针：每次走2步

        // 快指针走到末尾时，慢指针恰好指向中间节点
        while (fast != null && fast.next != null) {
            pre = slow;       // 保存慢指针前驱节点
            slow = slow.next; // 慢指针移动
            fast = fast.next.next; // 快指针移动
        }
        pre.next = null; // 关键：断开链表，完成拆分
        return slow;     // 返回右半部分链表的头节点
    }

    /**
     * 合并两个有序链表（LeetCode 21）
     * 双指针遍历，按大小拼接节点
     */
    private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(); // 哨兵节点：简化边界处理
        ListNode cur = dummy;            // 游标：指向新链表末尾

        // 同时遍历两个链表，拼接较小的节点
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                cur.next = list1;
                list1 = list1.next;
            } else {
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next; // 游标后移
        }
        // 拼接剩余未遍历完的节点
        cur.next = list1 != null ? list1 : list2;
        return dummy.next; // 哨兵节点的下一个节点即为新链表头
    }
}
```

## 核心模块解析
### 1. 主函数：sortList（分治核心）
- **终止条件**：空链表/单节点链表天然有序，直接返回；
- **拆分**：调用 `middleNode` 拆分链表为左右两部分；
- **递归**：对左右子链表分别递归排序；
- **合并**：调用 `mergeTwoLists` 合并两个有序链表。

### 2. 拆分函数：middleNode（快慢指针）
- **原理**：快指针速度是慢指针的2倍，快指针到末尾时，慢指针指向中间；
- **关键操作**：用 `pre` 记录慢指针前驱节点，执行 `pre.next = null` 断开链表，保证左右子链表完全分离；
- **示例**：链表 `[4,2,1,3]` 拆分后 → 左 `[4,2]`、右 `[1,3]`。

### 3. 合并函数：mergeTwoLists（双指针）
- **哨兵节点**：避免处理头节点为空的边界问题，简化代码；
- **双指针遍历**：同时遍历两个有序链表，每次取较小节点拼接到新链表；
- **剩余节点拼接**：一个链表遍历完后，直接拼接另一个链表的剩余部分。

## 复杂度分析
1. **时间复杂度**：O(n log n)
   - 拆分：每次拆分链表为两半，共 log n 层；
   - 合并：每层合并的总节点数都是 n，总时间为 n log n。
2. **空间复杂度**：O(log n)
   - 递归调用栈的深度为 log n（链表长度），符合题目要求。

## 核心技巧总结
1. 链表排序首选**归并排序**，无需额外空间移动节点，仅修改指针指向；
2. 快慢指针是链表**找中点、拆分链表**的万能方法；
3. 合并有序链表时用**哨兵节点**，大幅简化代码逻辑。

---

### 总结
1. 这道题用**归并排序（分治法）** 实现链表排序，满足 O(n log n) 时间复杂度；
2. 核心三步：**快慢指针拆分链表 → 递归排序子链表 → 双指针合并有序链表**；
3. 两个关键工具：快慢指针（找中点/拆分）、哨兵节点（简化合并）。
