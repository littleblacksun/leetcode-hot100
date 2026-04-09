<img width="765" height="688" alt="image" src="https://github.com/user-attachments/assets/ac5d3710-50d6-41b7-b7cf-15a9d5d5ac3e" />


# LeetCode 94. 二叉树的中序遍历 | Morris 遍历最优解法笔记
## 题目描述
给定一个二叉树的根节点 `root`，返回**中序遍历**它的节点值。

**中序遍历规则**：左子树 → 根节点 → 右子树

**进阶要求**：能够使用**常数空间**、按照递归解法实现遍历？

常规解法：递归（O(n) 栈空间）、迭代栈（O(n) 空间）
最优解法：**Morris 遍历**（O(1) 额外空间，O(n) 时间）

---

## 解法：Morris 中序遍历（常数空间）
### 核心思路
Morris 遍历是一种**无需栈/递归、仅使用常数额外空间**的二叉树遍历算法，核心原理：
1. 利用二叉树的**空闲指针（叶子节点的 right）** 建立临时线索，记录返回父节点的路径
2. 遍历完成后**恢复树的原始结构**，不破坏原树
3. 严格遵循中序遍历：左 → 根 → 右

### 算法步骤
1. 初始化当前节点 `root`，结果列表 `ans`
2. 当当前节点不为空：
   - 如果当前节点有**左子树**：
     1. 找到当前节点的**中序前驱节点**（左子树的最右节点）
     2. 若前驱节点的右指针为空：建立线索（`pre.right = root`），向左遍历
     3. 若前驱节点的右指针指向当前节点：说明左子树遍历完成，**记录当前节点值**，断开线索，向右遍历
   - 如果当前节点**无左子树**：直接记录当前节点值，向右遍历

---

## 完整代码
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();

        while (root != null) {
            // 情况1：当前节点存在左子树
            if (root.left != null) {
                // 寻找当前节点的中序前驱节点：左子树的最右节点
                TreeNode pre = root.left;
                while (pre.right != null && pre.right != root) {
                    pre = pre.right;
                }

                // 子情况1：前驱节点右指针为空 → 建立线索，向左遍历
                if (pre.right == null) {
                    pre.right = root;  // 建立临时返回路径
                    root = root.left;
                    continue;
                }

                // 子情况2：前驱节点右指针指向当前节点 → 左子树遍历完成
                pre.right = null;  // 断开线索，恢复树结构
            }
            
            // 记录根节点值（左子树已遍历完成）
            ans.add(root.val);
            // 向右遍历（右子树 / 线索返回父节点）
            root = root.right;
        }

        return ans;
    }
}
```

---

## 实例演示
### 示例二叉树
```
        1
         \
          2
         /
        3
```
**中序遍历结果**：`[1, 3, 2]`

### 遍历过程详解
1. 初始 `root = 1`，无左子树 → 记录 `1`，`root` 指向右节点 `2`
2. `root = 2`，有左子树 `3`：
   - 找到前驱节点 `3`，`3.right = null` → 建立线索 `3.right = 2`，`root` 指向 `3`
3. `root = 3`，无左子树 → 记录 `3`，`root` 指向右节点 `2`（线索）
4. `root = 2`，再次遍历：
   - 前驱节点 `3.right = 2` → 断开线索，记录 `2`，`root` 指向 `null`
5. 遍历结束，结果：`[1,3,2]`

---

## 复杂度分析
- **时间复杂度**：O(n)
  每个节点最多被访问 2 次（建立线索+遍历），整体线性时间
- **空间复杂度**：O(1)
  仅使用常数个临时变量，无递归栈/迭代栈空间开销

---

## 关键点总结
1. **前驱节点**：左子树的最右节点，是中序遍历中当前节点的上一个节点
2. **线索作用**：替代栈记录返回路径，实现常数空间遍历
3. **恢复树结构**：遍历完成后必须断开临时线索，避免破坏原二叉树
4. **适用场景**：需要严格控制空间开销的二叉树遍历场景

---

## 对比其他解法
| 解法         | 空间复杂度 | 优点                     |
|--------------|------------|--------------------------|
| 递归遍历     | O(n)       | 代码简洁，易理解         |
| 迭代栈遍历   | O(n)       | 非递归，可控遍历流程     |
| **Morris 遍历** | **O(1)** | **最优空间，无额外栈开销** |


<img width="1165" height="873" alt="image" src="https://github.com/user-attachments/assets/2c10ef1b-ed6a-497d-baf4-c3f4133fa238" />

对于每一个根节点，先找到他的前驱节点，然后建立线索，也就是如上图所示

然后继续遍历左子树，一旦左子树为空，也就是root.left==null,这个时候走root.right，因为基本上每一个叶子的right都有线索，指向他们的上一个root，这样子完成了中序遍历

