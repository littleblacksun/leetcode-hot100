<img width="821" height="817" alt="image" src="https://github.com/user-attachments/assets/809d34f8-7073-49a5-8810-f32ef4224239" />

# LeetCode 206. 反转链表 递归法 超详细逐行解析

这是**反转链表**的递归解法，核心思路：**先递归走到链表末尾，把末尾节点作为新头节点，再回溯反转每一个节点的指向**。

时间复杂度 O(N)，空间复杂度 O(N)（递归调用栈）。

---

## 完整代码 + 逐行中文注释

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
    /**
     * 递归法反转单链表
     * @param head 当前递归层的头节点
     * @return 反转后新链表的头节点（原链表末尾节点）
     */
    public ListNode reverseList(ListNode head) {
        // ===================== 递归终止条件 =====================
        // 情况1：head == null → 链表本身为空，直接返回 null
        // 情况2：head.next == null → 已经递归到链表最后一个节点，这是新链表的头
        if(head == null || head.next == null)
        {
            return head; // 返回最后一个节点，作为整个反转链表的新头节点 revHead
        }

        // ===================== 递归「递」的过程 =====================
        // 递归调用：一直往链表深处走，直到最后一个节点
        // revHead 永远保存的是【原链表最后一个节点】，也就是反转后的新头节点
        ListNode revHead = reverseList(head.next);

        // ===================== 递归「归」的过程（核心反转逻辑） =====================
        // head.next 是当前节点的下一个节点，也就是当前新链表的最后一个节点（尾巴）
        ListNode tail = head.next;
        
        // 核心反转：让当前尾巴节点 指向 自己（head）
        // 例：原 1→2，反转后变成 2→1
        tail.next = head;
        
        // 必须把当前节点的 next 置空，否则会形成环！
        // 不写这行：1↔2 互相指向，链表死循环
        head.next = null;

        // ===================== 统一返回新头节点 =====================
        // 每层递归都返回 revHead，保证最上层拿到的就是新头
        return revHead;
    }
}
```

---

## 核心原理图解（超级好懂）

假设链表：`1 → 2 → 3 → 4 → 5 → null`

### 1. 递归「递」下去（一直往后走）
- `reverseList(1)` → 调用 `reverseList(2)`
- `reverseList(2)` → 调用 `reverseList(3)`
- `reverseList(3)` → 调用 `reverseList(4)`
- `reverseList(4)` → 调用 `reverseList(5)`
- 到 `5` 时：`5.next == null` → **触发终止条件，返回 5 作为新头 `revHead`**

### 2. 递归「归」回来（从后往前反转）
回到 `4` 这一层：
- `tail = 4.next = 5`
- `5.next = 4` → 指向反转：`5 → 4`
- `4.next = null` → 斩断旧指向

回到 `3` 这一层：
- `tail = 3.next = 4`
- `4.next = 3` → 指向反转：`5 → 4 → 3`
- `3.next = null`

依次回溯，最终得到：`5 → 4 → 3 → 2 → 1 → null`

---

## 每一步核心作用总结
1. **终止条件**：找到链表最后一个节点，作为**新头节点**
2. **递归深入**：一直走到链表末尾，拿到新头
3. **回溯反转**：让后一个节点指向前一个节点
4. **置空旧指向**：防止链表成环
5. **返回新头**：全程保持新头节点不变，最终返回

---

## 必须注意的坑
- **不写 `head.next = null`**：链表最后两个节点会**互相指向形成环**，导致死循环
- **`revHead` 全程不变**：永远是原链表最后一个节点，是反转后的正确头节点

---

### 总结
1. 递归分为**递（深入到末尾）**和**归（回溯反转指向）**两步；
2. 终止条件返回**末尾节点作为新头**；
3. 回溯时**反转节点指向 + 置空旧指向**；
4. 全程返回新头节点，最终得到完整反转链表。


# LeetCode 206. 反转链表 迭代法（头插法/双指针）超详细笔记
这是**反转链表最优解**，**迭代（双指针）法**，也叫头插法反转。
时间复杂度 **O(N)**，空间复杂度 **O(1)**（只用了几个指针，无额外空间）。

---

## 完整代码 + 逐行超详细注释
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
    /**
     * 迭代法（双指针/头插法）反转单链表
     * @param head 原链表的头节点
     * @return 反转后新链表的头节点
     */
    public ListNode reverseList(ListNode head) {
        // 1. 定义前驱指针 pre：初始指向 null（反转后，原头节点变成尾节点，指向 null）
        ListNode pre = null;
        
        // 2. 定义当前指针 cur：初始指向 原链表头节点 head
        ListNode cur = head;

        // 3. 循环遍历整个链表：cur 不为空就继续反转
        while(cur != null){
            // 【关键步骤1】保存 cur 的下一个节点 nxt
            // 因为马上要修改 cur.next，如果不保存，会丢失后面的链表
            ListNode nxt = cur.next;

            // 【关键步骤2】核心反转：让当前节点 cur 指向 前驱节点 pre
            // 例：1→2  →  1←2
            cur.next = pre;

            // 【关键步骤3】pre 指针向后移动：pre 走到 cur 的位置
            pre = cur ;

            // 【关键步骤4】cur 指针向后移动：cur 走到刚才保存的 nxt 位置
            cur = nxt;
        }

        // 4. 循环结束时，cur = null，pre 指向反转后的【新链表头节点】
        // 所以返回 pre
        return pre;
    }
}
```

---

## 核心逻辑图解（超级好懂）
假设原链表：`1 → 2 → 3 → null`

### 初始状态
`pre = null`
`cur = 1`

### 第1轮循环
- 保存 `nxt = 2`
- `cur.next = pre` → `1 → null`
- `pre = 1`
- `cur = 2`

链表：`null ← 1    2 → 3 → null`

### 第2轮循环
- 保存 `nxt = 3`
- `cur.next = pre` → `2 → 1`
- `pre = 2`
- `cur = 3`

链表：`null ← 1 ← 2    3 → null`

### 第3轮循环
- 保存 `nxt = null`
- `cur.next = pre` → `3 → 2`
- `pre = 3`
- `cur = null`

链表：`null ← 1 ← 2 ← 3`

### 循环结束
`cur = null`
`pre = 3`（新头节点）
返回 `pre`

---

## 四步口诀（背会就不会忘）
1. **保存下一节点**
2. **反转当前指向**
3. **前驱指针后移**
4. **当前指针后移**

---

## 为什么要写 ListNode nxt = cur.next？
- 如果不保存，执行 `cur.next = pre` 后，**后面的链表会直接丢失**
- 必须先存下家，再改指向

---

## 递归 vs 迭代 对比
| 方式 | 空间复杂度 | 理解难度 | 推荐 |
| --- | --- | --- | --- |
| 递归 | O(N) | 中等 | 理解思路 |
| **迭代** | **O(1)** | **简单** | **面试/刷题首选** |

---

### 总结
1. 迭代法用**双指针**，从前往后逐个反转指向；
2. 必须**保存下一节点**，防止链表断裂；
3. 循环结束后 **pre 就是新链表头**；
4. 最优解法，面试必背。
