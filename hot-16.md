<img width="1239" height="1059" alt="image" src="https://github.com/user-attachments/assets/ad9014e3-fad5-4707-a3b4-83a132dd8f55" />

# LeetCode 160. 相交链表 双指针法 超详细解析

这是**相交链表（Intersection of Two Linked Lists）**的最优解法，时间复杂度 O(N+M)，空间复杂度 O(1)，核心思路是**双指针循环遍历，消除链表长度差**。

## 完整代码 + 逐行注释

```java
/**
 * Definition for singly-linked list.
 * 链表节点定义（LeetCode 内置）
 */
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) {
        val = x;
        next = null;
    }
}

public class Solution {
    /**
     * 获取两个链表的第一个相交节点
     * @param headA 链表A的头节点
     * @param headB 链表B的头节点
     * @return 相交节点 / null(无相交)
     */
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // 定义指针p，初始指向链表A的头节点
        ListNode p = headA;
        // 定义指针q，初始指向链表B的头节点
        ListNode q = headB;

        // 核心循环：当两个指针不相遇时，持续遍历
        // 终止条件：p == q（要么指向相交节点，要么都指向null）
        while(p != q){
            // 指针p移动逻辑：
            // 如果p不为空 → 向后走一步；如果p为空 → 走到链表B的头部
            p = p!= null ? p.next : headB;
            
            // 指针q移动逻辑：
            // 如果q不为空 → 向后走一步；如果q为空 → 走到链表A的头部
            q = q!= null ? q.next : headA;
        }

        // 循环结束，返回p（和q相等）：相交节点 或 null
        return p;
    }
}
```

---

## 核心原理解析

### 1. 为什么这个算法能找到相交节点？

- 假设链表A长度：`a + c`，链表B长度：`b + c`（`c` 是公共部分长度）
- 指针 `p` 完整走一遍：`A → B`，总路程：`a + c + b`
- 指针 `q` 完整走一遍：`B → A`，总路程：`b + c + a`
- 
- **两个指针路程完全相等**，一定会在**相交节点**相遇；
- 
- 如果无相交节点，两个指针最终都会走到 `null`，循环依然终止。
  

### 2. 执行流程举例

1. 初始：`p → A头`，`q → B头`
2. 
3. 两个指针同时向后移动
4. 
5. 一个指针走到链表末尾（null），立刻切换到另一个链表头部
6. 
7. 最终**必然相遇**

### 3. 边界情况处理

- 两个链表**完全相同**：直接在头节点相遇
- 
- 两个链表**无相交**：最终都走到 `null`，返回 `null`
- 
- 一个链表是另一个链表的子链：在相交点正常相遇

---

## 代码关键点总结

1. **不修改原链表**：纯遍历，无任何节点改动
2. 
3. **O(1) 空间**：只用了两个临时指针
4. 
5. **自动消除长度差**：不需要提前计算链表长度
6. 
7. **终止条件严谨**：`p == q` 覆盖所有场景

### 一句话记忆

> 你走我的路，我走你的路，路程相同，我们终会相遇。
> 
</think>### 总结

1. 核心逻辑：双指针**交替遍历两个链表**，利用路程相等消除长度差，最终相遇在相交节点；
2. 
3. 逐行代码都是指针移动和循环判断，无复杂操作，空间复杂度O(1)；
4. 
5. 终止条件 `p == q` 完美处理**有相交**和**无相交**两种场景。
6. 
