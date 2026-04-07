<img width="1236" height="1122" alt="image" src="https://github.com/user-attachments/assets/3180d8bc-6520-4c61-9630-6ec314d46b69" />

# LeetCode 138. 随机链表的复制（原地复制法）GitHub 笔记
我先给你**逐行详细解析代码**，再直接给你**可直接复制到 GitHub 的完整 Markdown 笔记**（含思路 + 图解 + 实例 + 代码）。

---

## 一、代码逐行详细解析
```java
class Solution {
    public Node copyRandomList(Node head) {
        // 第一步：复制每个节点，插入到原节点后面
        for(Node cur = head; cur!=null;cur=cur.next.next){
            cur.next = new Node(cur.val,cur.next);
        }

        // 第二步：给新节点设置 random 指针
        for(Node cur = head;  cur != null;cur = cur.next.next){
            if(cur.random!=null){
                cur.next.random = cur.random.next;
            }
        }

        // 第三步：拆分新旧链表，恢复原链表 + 得到新链表
        Node dummy = new Node(0);
        Node tail = dummy;
        for (Node cur = head; cur!=null;cur = cur.next,tail=tail.next){
            Node copy = cur.next; // 取出当前复制节点
            tail.next = copy;     // 拼接新链表
            cur.next = copy.next; // 恢复原链表指针
        }
        return dummy.next;
    }
}
```

### 核心三步骤（必背！）
1. **复制插入**：每个新节点直接插在原节点后面
2. **复制随机指针**：利用位置关系，直接赋值 `cur.next.random = cur.random.next`
3. **拆分链表**：把新旧链表分开，返回新链表

---

## 二、GitHub Markdown 完整笔记
```markdown
# LeetCode 138. 随机链表的复制（原地复制法）

## 题目描述
给你一个长度为 `n` 的链表，每个节点包含一个额外增加的随机指针 `random`，该指针可以指向链表中的任何节点或空节点。

构造这个链表的**深拷贝**。深拷贝应该正好由 `n` 个**全新节点**组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 `next` 指针和 `random` 指针也都应指向复制链表中的新节点。

## 解题思路：原地复制 + 三步法
### 核心思想
不使用哈希表，**原地复制节点**，利用节点位置关系完成 `random` 指针复制，最后拆分出新旧链表。

### 三大核心步骤
1. **复制节点并插入**：遍历原链表，为每个节点创建副本，插入到原节点后面。
2. **复制随机指针**：利用 `原节点.next = 新节点`，直接给新节点赋值 `random`。
3. **拆分链表**：将交错的新旧链表分离，恢复原链表并得到新链表。

## 完整代码
```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        // 第一步：复制每个节点，插入到原节点的后面
        for(Node cur = head; cur != null; cur = cur.next.next){
            cur.next = new Node(cur.val, cur.next);
        }

        // 第二步：处理新节点的 random 指针
        for(Node cur = head; cur != null; cur = cur.next.next){
            if(cur.random != null){
                // 新节点.random = 原节点.random对应的新节点
                cur.next.random = cur.random.next;
            }
        }

        // 第三步：拆分新旧链表
        Node dummy = new Node(0);
        Node tail = dummy;
        for(Node cur = head; cur != null; cur = cur.next){
            Node copy = cur.next; // 取出复制节点
            tail.next = copy;     // 拼接新链表
            tail = tail.next;     // 新链表尾指针后移
            cur.next = copy.next; // 恢复原链表的 next 指针
        }

        return dummy.next;
    }
}
```

## 图解执行过程
### 原链表
`1 → 2 → 3`

### 第一步：复制插入
`1 → 1新 → 2 → 2新 → 3 → 3新`

### 第二步：复制 random
`1新.random = 1.random.next`

### 第三步：拆分
- 原链表：`1 → 2 → 3`
- 新链表：`1新 → 2新 → 3新`

## 实例演示
输入：
```
head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
```
输出：
```
[[7,null],[13,0],[11,4],[10,2],[1,0]]
```
（返回深拷贝后的链表）

## 复杂度分析
- **时间复杂度**：O(n)
  遍历链表 3 次，均为线性时间。
- **空间复杂度**：O(1)
  仅使用常数额外空间，**最优解法**。

## 总结
1. **原地复制**是随机链表复制的最优解法，空间复杂度 O(1)。
2. 核心三步骤：**插入复制 → 复制随机 → 拆分链表**。
3. 利用 `cur.next` 直接定位新节点，无需哈希表映射。
```

---

