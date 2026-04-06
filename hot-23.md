<img width="717" height="712" alt="image" src="https://github.com/user-attachments/assets/e9fd8457-0b7c-4bc4-9fbd-201f976b4326" />

我为你**逐行关键注释**这段代码，然后用标准 **GitHub Markdown** 格式写完整解题思路、步骤、图解，和你之前两数相加的笔记格式完全一致。

# 完整带详细注释代码
```java
/**
 * Definition for singly-linked list.
 * 单向链表节点定义（LeetCode 自带）
 */
public class ListNode {
    int val;        // 节点存储的值
    ListNode next;  // 指向下一个节点
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

class Solution {
    /**
     * 删除链表的倒数第 N 个节点
     * 方法：快慢指针（双指针）+ 哨兵节点
     */
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 1. 创建【哨兵节点（虚拟头节点）】
        // 作用：统一处理「删除头节点」的边界情况（不用单独判断）
        ListNode dummy = new ListNode(0, head);

        // 2. 定义快慢指针，初始都指向哨兵节点
        ListNode left = dummy;  // 慢指针：最终指向【待删除节点的前驱节点】
        ListNode right = dummy; // 快指针：负责拉开距离

        // 3. 快指针先走 n 步
        // 执行后：right 领先 left 共 n 个节点
        while (n-- > 0) {
            right = right.next;
        }

        // 4. 快慢指针同时向后走
        // 直到快指针到达链表【最后一个节点】
        // 此时：left 正好停在【待删除节点的前一个节点】
        while (right.next != null) {
            left = left.next;
            right = right.next;
        }

        // 5. 核心删除操作
        // 让 left 的 next 直接指向 下下个节点 → 跳过待删除节点
        left.next = left.next.next;

        // 6. 返回真正的头节点（哨兵.next）
        return dummy.next;
    }
}
```

---

# GitHub Markdown 格式：解题思路 & 解答方法

# LeetCode 19. 删除链表的倒数第 N 个节点
## 题目描述
给你一个链表，删除链表的**倒数第 n 个节点**，并且返回链表的头节点。

**要求**：使用**一次遍历**实现。

---

## 解题思路（快慢指针法 / 双指针法）
这道题最优雅、面试最推荐的方法是 **快慢指针 + 哨兵节点**。

### 核心思想
1. **倒数变正数**
   不用先遍历求长度，用**两个指针的间距**直接定位倒数第 N 个节点。

2. **快指针先走 n 步**
   让快指针领先慢指针 `n` 个节点。

3. **一起走到末尾**
   当快指针走到链表最后一个节点时，
   **慢指针正好停在「待删除节点的前一个节点」**。

4. **哨兵节点**
   解决**删除头节点**的边界问题（例如 `[1]` 删除倒数第1个）。

---

## 图解步骤（超清晰）
以链表 `1 → 2 → 3 → 4 → 5`，删除倒数第 2 个节点（4）为例：

### 步骤 1：创建哨兵节点
`dummy → 1 → 2 → 3 → 4 → 5`
`left、right` 都指向 `dummy`

### 步骤 2：快指针先走 n 步
`right` 走 2 步 → 指向 `2`
此时间距 = 2

### 步骤 3：快慢指针一起走，直到 right 到尾节点
`right` 走到 `5`（最后一个节点）
`left` 走到 `3`（待删除节点 4 的前驱）

### 步骤 4：执行删除
`left.next = left.next.next`
`3 → 5`，跳过 4

### 结果
`1 → 2 → 3 → 5`

---

## 代码执行步骤总结
1. **创建哨兵节点**，统一边界处理。
2. **快指针先走 n 步**，拉开间距。
3. **双指针同时移动**，直到快指针到末尾。
4. **慢指针指向待删除节点的前驱**。
5. **修改指针跳过待删除节点**，完成删除。
6. **返回哨兵节点的 next** 作为新头节点。

---

## 关键代码解释
### 1. 哨兵节点 `dummy`
```java
ListNode dummy = new ListNode(0, head);
```
- 解决**删除头节点**的问题（如 `[1]` 删除 1）
- 让所有节点的删除逻辑完全统一

### 2. 快指针先走 n 步
```java
while(n-- >0){
    right = right.next;
}
```
- 让 `right` 领先 `left`  exactly `n` 步

### 3. 找到待删除节点的前驱
```java
while(right.next!=null){
    left=left.next;
    right=right.next;
}
```
- 循环结束时，`left` 就是**待删除节点的前一个节点**

### 4. 删除节点
```java
left.next = left.next.next;
```
- 链表删除的标准操作：跳过待删除节点

---

## 复杂度分析
- **时间复杂度**：$O(L)$
  只遍历链表一次，L 为链表长度。
- **空间复杂度**：$O(1)$
  只使用了常数个指针变量。

---

## 优点
1. **一次遍历**（最优解法）
2. **无需计算链表长度**
3. **哨兵节点完美处理边界**
4. **代码简洁、面试高分写法**

---

### 总结
1. 这道题的**最优解就是快慢指针**。
2. 核心：**快指针先走 n 步，再一起走到末尾**。
3. 哨兵节点让代码不用判断头节点删除，逻辑更干净。
4. 面试遇到这道题，直接写这个版本即可。

如果你需要，我还能帮你做**递归版 + 快慢指针版**的对比笔记！
