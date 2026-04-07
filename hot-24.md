<img width="1027" height="700" alt="image" src="https://github.com/user-attachments/assets/3ed2a6ad-1343-4956-b5e3-34efe2bf84bb" />


# LeetCode 24. 两两交换链表中的节点 详细解析与笔记
我会先**逐行逐逻辑解析你提供的代码**，再按照 GitHub Markdown 格式整理成完整笔记（包含解题思路、图解、代码、运行实例）。

---

## 一、代码逐行详细解析
### 1. 前提：链表节点定义
```java
public class ListNode {
    int val;          // 节点存储的值
    ListNode next;    // 指向下一个节点的指针
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```
这是**单链表的标准节点结构**，每个节点包含一个值和一个指向下一节点的引用。

---

### 2. 核心解法代码逐行解析
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 1. 创建虚拟头节点 dummy，next 指向原链表头节点
        ListNode dummy = new ListNode(0,head);
        // 2. 定义两个指针：node0 是前驱节点，node1 是当前要交换的第一个节点
        ListNode node0 = dummy;
        ListNode node1 = head;

        // 3. 循环条件：必须有两个节点才能交换（node1 存在，且下一个节点也存在）
        while(node1!=null&&node1.next!=null){
            // 4. 暂存两个关键节点：
            // node2 = 要交换的第二个节点
            // node3 = 下一组待交换的第一个节点
            ListNode node2 = node1.next;
            ListNode node3 = node2.next;

            // 5. 核心：三步交换指针（最关键逻辑）
            node0.next = node2;   // 前驱节点指向第二个节点
            node2.next = node1;   // 第二个节点指向第一个节点（完成反转）
            node1.next = node3;   // 原第一个节点指向后续节点

            // 6. 指针后移，准备下一轮交换
            node0 = node1;        // 前驱节点更新为当前交换后的最后一个节点
            node1 = node3;        // 当前节点更新为下一组的第一个节点
        }

        // 7. 返回新链表的头节点（dummy.next 就是交换后的头）
        return dummy.next;
    }
}
```

---


## 二、GitHub Markdown 格式笔记
# LeetCode 24. 两两交换链表中的节点
## 题目描述
给定一个链表，**两两交换**其中相邻的节点，并返回交换后的链表。
你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

**示例 1：**
```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**
```
输入：head = []
输出：[]
```

**示例 3：**
```
输入：head = [1]
输出：[1]
```

---

## 解题思路
### 核心方法：迭代法 + 虚拟头节点
1. **使用虚拟头节点**：统一头节点和普通节点的操作逻辑，避免边界判断。
2. **四指针迭代**：每次处理两个节点，通过指针修改引用完成交换。
3. **循环交换**：每次交换完成后，指针后移，处理下一组节点，直到不足两个节点。

### 交换逻辑图解
原始节点关系：
`node0 -> node1 -> node2 -> node3`

交换后：
`node0 -> node2 -> node1 -> node3`

---

## 完整代码
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
    public ListNode swapPairs(ListNode head) {
        // 创建虚拟头节点，指向原链表头部
        ListNode dummy = new ListNode(0, head);
        // 前驱指针
        ListNode prev = dummy;
        // 当前遍历指针
        ListNode curr = head;

        // 必须存在两个节点才能交换
        while (curr != null && curr.next != null) {
            // 暂存第二个节点和后继节点
            ListNode second = curr.next;
            ListNode nextGroup = second.next;

            // 核心三步交换指针
            prev.next = second;
            second.next = curr;
            curr.next = nextGroup;

            // 指针后移，准备下一轮交换
            prev = curr;
            curr = nextGroup;
        }

        // 返回新链表的头节点
        return dummy.next;
    }
}
```

---

## 运行实例演示
### 输入链表：`1 -> 2 -> 3 -> 4`
**初始状态**：
`dummy -> 1 -> 2 -> 3 -> 4`
`prev = dummy, curr = 1`

**第一轮交换（1、2）**：
1. `second = 2`，`nextGroup = 3`
2. `dummy.next = 2`
3. `2.next = 1`
4. `1.next = 3`
结果：`dummy -> 2 -> 1 -> 3 -> 4`
指针更新：`prev=1，curr=3`

**第二轮交换（3、4）**：
1. `second = 4`，`nextGroup = null`
2. `1.next = 4`
3. `4.next = 3`
4. `3.next = null`
结果：`dummy -> 2 -> 1 -> 4 -> 3`
指针更新：`prev=3，curr=null`，循环终止

**最终输出**：`2 -> 1 -> 4 -> 3`

---

## 复杂度分析
- **时间复杂度**：O(n)，遍历链表一次
- **空间复杂度**：O(1)，仅使用常数级额外空间

---

## 总结
1. 虚拟头节点是链表操作的**常用技巧**，能极大简化边界处理。
2. 交换链表节点核心是**修改指针引用**，而非修改节点值。
3. 必须暂存后继节点，防止链表断裂。

---

### 总结
1. 代码核心是**迭代+虚拟头节点+四指针交换**，完美处理链表两两交换；
2. 虚拟头节点解决了头节点交换的边界问题，让代码逻辑统一；
3. 笔记可直接复制到 GitHub 使用，包含了题目、思路、代码、实例、复杂度全维度内容。****




# LeetCode 24 两两交换链表中的节点：递归解法 超详细解析
我先**逐行逐句给你讲懂这段递归代码**，再直接给你**可复制到 GitHub 的完整 Markdown 笔记**（含思路 + 图解 + 实例 + 完整代码）。

---

# 一、递归代码逐行详细解析
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 递归终止条件：没有节点 或 只剩一个节点，无法交换，直接返回
        if(head == null || head.next == null){
            return head;
        }

        // 定义当前要交换的两个节点 + 下一组的起点
        ListNode node1 = head;      // 第一个节点
        ListNode node2 = head.next; // 第二个节点
        ListNode node3 = node2.next;// 下一组待交换的起点（后面的链表）

        // 递归：先把后面的链表全部交换好，返回交换后的头节点
        // 让 node1 指向 后面已经交换好的链表
        node1.next = swapPairs(node3);

        // 让 node2 指向 node1，完成本次两个节点的交换
        node2.next = node1;

        // 本次交换后，新的头节点是 node2，返回给上一层
        return node2;
    }
}
```

## 核心逻辑一句话概括
**从后往前交换**：先递归把后面所有节点两两交换好，再回头交换当前这两个节点，最后返回当前组的新头节点。

## 关键部分拆解
1. **递归终止条件**
   - 没有节点 `head==null`
   - 只有一个节点 `head.next==null`
   → 都无法交换，直接返回原节点。

2. **三个节点定义**
   - `node1`：当前组**第一个节点**
   - `node2`：当前组**第二个节点**
   - `node3`：**下一组的第一个节点**（后面的链表）

3. **最核心的两行代码**
   ```java
   node1.next = swapPairs(node3); // 后面先交换好
   node2.next = node1;            // 再交换当前两个节点
   ```
   执行顺序：**先递归后面 → 再处理当前**。

4. **返回值**
   每组交换后，**新头都是 node2**，所以 return node2。

---

# 二、GitHub Markdown 格式笔记（可直接复制使用）
```markdown
# LeetCode 24. 两两交换链表中的节点（递归法）

## 题目描述
给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
**你不能只是单纯地改变节点内部的值，而是需要实际地进行节点交换。**

## 解题思路：递归法
### 核心思想
**把大问题拆成重复的小问题：**
每次只处理**两个节点**，剩下的链表交给递归函数去处理。

### 递归三要素
1. **终止条件**：链表为空 或 只剩一个节点，无法交换，直接返回。
2. **递归处理**：递归处理当前两个节点后面的所有链表，得到后面交换好的头节点。
3. **本次操作**：交换当前两个节点，连接后面交换好的链表。

### 交换逻辑
```
原始：node1 → node2 → (后面递归处理好的链表)
交换后：node2 → node1 → (后面递归处理好的链表)
```

## 完整代码
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
    public ListNode swapPairs(ListNode head) {
        // 递归终止条件
        if(head == null || head.next == null){
            return head;
        }

        // 定义当前要交换的两个节点 + 下一组起点
        ListNode node1 = head;
        ListNode node2 = head.next;
        ListNode node3 = node2.next;

        // 递归：先交换后面的链表
        node1.next = swapPairs(node3);
        // 交换当前两个节点
        node2.next = node1;

        // 返回当前组交换后的新头节点
        return node2;
    }
}
```

## 实例演示（最直观理解）
输入链表：`1 → 2 → 3 → 4`

### 执行过程（从后往前交换）
1. **最底层递归**：处理 `3 → 4`
   - node1=3，node2=4，node3=null
   - 递归 swapPairs(null) 返回 null
   - 3.next = null
   - 4.next = 3
   - 返回 4
   - 结果：`4 → 3`

2. **回到上一层**：处理 `1 → 2`
   - node1=1，node2=2，node3=3
   - 1.next = 上一层返回的 4
   - 2.next = 1
   - 返回 2
   - 结果：`2 → 1 → 4 → 3`

### 最终结果
`2 → 1 → 4 → 3`

## 更多测试案例
- 输入：`[]`，输出：`[]`
- 输入：`[1]`，输出：`[1]`
- 输入：`[1,2,3]`，输出：`[2,1,3]`
- 输入：`[1,2,3,4,5]`，输出：`[2,1,4,3,5]`

## 复杂度分析
- **时间复杂度**：O(n)
  遍历链表每个节点一次。
- **空间复杂度**：O(n)
  递归调用栈深度 = 链表长度。

## 总结
递归法代码**极其简洁**，思路是：
**先交换后面 → 再交换当前 → 返回新头**
非常适合理解递归思想，是链表类题目经典解法。
```

---

# 三、最关键的一张图（帮你秒懂递归）
输入：`1 → 2 → 3 → 4`

执行顺序：
1. 交换 `3、4` → 得到 `4→3`
2. 交换 `1、2` → 连接 `4→3`
3. 最终：`2→1→4→3`

---

# 四、极简总结
1. 递归法**代码短、思路清晰**，是面试高频写法；
2. 核心：**先处理后面，再处理当前**；
3. 终止条件：**空 或 单个节点**直接返回；
4. 每组交换后返回 **node2** 作为新头节点。

需要我给你做**递归 vs 迭代两种写法对比**吗？一眼就能分清什么时候用哪种！
