

<img width="1024" height="971" alt="image" src="https://github.com/user-attachments/assets/e92815c7-f4ed-4554-b228-382ebe7b9202" />



# LeetCode 21 合并两个有序链表（Java 迭代版）笔记
## 题目简介
合并两个**升序**的单链表，返回一个新的升序链表，新链表由两个原链表的节点拼接而成。

## 完整代码（带规范注释）
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
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        // 1. 创建哨兵节点（虚拟头节点），避免处理空链表的边界问题
        ListNode dummy = new ListNode();
        // 2. 工作指针：用来遍历、拼接新链表，始终指向新链表的末尾
        ListNode cur = dummy;

        // 3. 循环：两个链表都没遍历完时，比较节点值，小的优先加入新链表
        while(list1 != null && list2 != null){
            if(list1.val < list2.val){
                // 把较小的 list1 节点接到新链表末尾
                cur.next = list1;
                // list1 指针后移，继续比较下一个节点
                list1 = list1.next;
            }else{
                // 把较小的 list2 节点接到新链表末尾
                cur.next = list2;
                // list2 指针后移，继续比较下一个节点
                list2 = list2.next;
            }
            // 工作指针后移，保持指向新链表末尾
            cur = cur.next;
        }

        // 4. 拼接剩余节点：一个链表遍历完了，直接把另一个链表剩下的部分接上
        cur.next = list1 != null ? list1 : list2;

        // 5. 哨兵节点的 next 才是真正的新链表头节点
        return dummy.next;
    }
}
```

## 你的注释理解核对 ✅
**你的理解100%正确**，每一句注释都精准对应代码逻辑，没有错误：
1. `dummy` 确实是**哨兵节点**，用来简化边界处理；
2. `cur` 是用来移动拼接的指针，理解完全正确；
3. 循环内比较值、拼接节点、指针后移的逻辑注释无误；
4. 最后拼接剩余链表的逻辑理解准确。

## 超直白解题思路（新手一看就懂）
### 1. 为什么要用哨兵节点？
正常合并链表，如果两个链表都为空，或者第一个节点就很小，需要写很多`if`判断。
创建一个**虚拟的头节点**，不管原链表是不是空，都能直接往后拼接，最后直接返回`dummy.next`就行，**不用处理复杂的边界情况**。

### 2. 核心思路（像排队一样）
- 两个链表都是从小到大排好序的；
- 我们拿两个指针，分别指着两个链表的**第一个节点**；
- 每次比一比，**谁的值小，就把谁拉到新链表里**；
- 拉走之后，这个链表的指针往后挪一位，继续比；
- 直到其中一个链表全部拉完了，**剩下的另一个链表直接全部接到末尾**（因为本来就是有序的）。

### 3. 步骤拆解
1. 建哨兵节点 + 工作指针；
2. 双指针遍历两个链表，小节点优先拼接；
3. 拼接完一个节点，对应链表指针后移；
4. 工作指针永远跟着新链表末尾走；
5. 一个链表空了，直接拼接另一个链表的剩余部分；
6. 返回真正的新链表头。

## 复杂度分析
- **时间复杂度**：$O(n+m)$，`n`和`m`是两个链表的长度，每个节点只遍历一次；
- **空间复杂度**：$O(1)$，只用到了两个额外指针，没有开辟新的链表空间。

