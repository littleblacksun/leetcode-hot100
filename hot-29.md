<img width="463" height="976" alt="image" src="https://github.com/user-attachments/assets/94871285-3ba9-489a-94c5-24f6025d6f84" />

# LeetCode 101. 对称二叉树
## 题目描述
给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```

## 解题思路
判断二叉树是否对称，**本质是判断根节点的左子树和右子树是否互为镜像**。
互为镜像的两个树需满足三个条件：
1. 两个树的根节点值相等
2. 第一个树的左子树 与 第二个树的右子树 镜像
3. 第一个树的右子树 与 第二个树的左子树 镜像

基于这个逻辑，我们可以复用「判断两棵树是否相同」的递归思路，**仅调整递归对比的节点顺序**，即可实现镜像判断。

## 代码实现
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        // 空树直接对称
        if(root == null){
            return true;
        }
        // 核心：判断左子树和右子树是否互为镜像
        return isMirror(root.left, root.right);
    }

    /**
     * 辅助方法：判断两棵树是否互为镜像
     * 复用同树判断逻辑，仅修改递归节点顺序
     */
    private boolean isMirror(TreeNode p, TreeNode q){
        // 终止条件1：两个节点都为空 → 镜像
        // 终止条件2：一个为空一个不为空 → 不对称
        if(p == null || q == null){
            return p == q;
        }
        // 1. 值相等 2. 左对右 3. 右对左
        return p.val == q.val 
            && isMirror(p.left, q.right) 
            && isMirror(p.right, q.left);
    }
}
```

## 代码解析
1. **主方法 `isSymmetric`**
   - 首先判断根节点是否为空，空树天然对称
   - 调用辅助方法，传入根节点的左、右子树，判断是否镜像

2. **辅助方法 `isMirror`**
   - **终止条件**：`p == null || q == null` 时，只有两者都为空才返回`true`，否则`false`
   - **递归核心**：
     - 先判断当前节点值是否相等
     - 递归判断`p.left`和`q.right`（左对右）
     - 递归判断`p.right`和`q.left`（右对左）
   - 三个条件同时满足，才是镜像对称

## 复杂度分析
- **时间复杂度**：$O(n)$
  遍历树的所有节点，每个节点仅访问一次
- **空间复杂度**：$O(h)$
  $h$ 为树的高度，递归调用栈的深度，最坏情况（链状树）为$O(n)$，平衡树为$O(logn)$

## 解题亮点
1. **代码极简**：复用递归思想，仅调整递归节点顺序，无冗余代码
2. **逻辑清晰**：严格贴合「镜像对称」的数学定义，易理解、易记忆
3. **边界全覆盖**：完美处理空树、单节点树、不对称树等所有边界场景

---
我可以帮你把这篇笔记**优化成带图文、更适合GitHub展示的进阶版**，也能帮你补充迭代法版本，需要吗？
