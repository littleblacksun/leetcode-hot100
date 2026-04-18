<img width="763" height="1016" alt="image" src="https://github.com/user-attachments/assets/645e5bc7-6ba3-48ad-9acb-f1e637afd9fc" />


<img width="516" height="530" alt="image" src="https://github.com/user-attachments/assets/40a4ac01-373b-46e5-8d25-6967065dac3e" />

既然要求左子树都小于节点值，右子树都大于节点值，那么就可以在每一个节点的左右子树，画一个范围，满足范围就符合题意

# LeetCode 98 验证二叉搜索树 题解笔记
## 题目描述
给定一个二叉树，判断其是否是一个有效的二叉搜索树（BST）。

有效二叉搜索树定义如下：
1. 节点的左子树只包含**小于**当前节点的数
2. 节点的右子树只包含**大于**当前节点的数
3. 所有左子树和右子树自身必须也是二叉搜索树

## 解题思路：递归 + 区间判断
核心思路：**为每个节点设置合法的取值区间**，递归验证每个节点是否满足区间要求，同时更新子节点的区间边界。
- 根节点的合法区间：`(-∞, +∞)`
- 左子节点的合法区间：`(父节点左边界, 父节点值)`（必须小于父节点）
- 右子节点的合法区间：`(父节点值, 父节点右边界)`（必须大于父节点）

使用`long`类型避免节点值为`Integer.MIN_VALUE`/`Integer.MAX_VALUE`时的边界溢出问题。

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

    public boolean isValidBST(TreeNode root) {
        // 调用重载方法，初始区间：Long 最小值 ~ Long 最大值
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    /**
     * 递归验证二叉搜索树
     * @param node 当前遍历的节点
     * @param left 当前节点的最小合法值（开区间）
     * @param right 当前节点的最大合法值（开区间）
     * @return 是否满足BST条件
     */
    private boolean isValidBST(TreeNode node, long left, long right) {
        // 空节点默认为有效BST
        if (node == null) {
            return true;
        }

        long x = node.val;
        // 1. 当前节点值在合法区间内
        // 2. 左子树区间更新为 (left, x)
        // 3. 右子树区间更新为 (x, right)
        return left < x && x < right 
                && isValidBST(node.left, left, x) 
                && isValidBST(node.right, x, right);
    }
}
```

## 代码解析
### 1. 主方法 `isValidBST(TreeNode root)`
- 作用：入口方法，初始化递归的**全局区间**
- 参数：二叉树根节点
- 返回：重载递归方法的结果，初始区间为`Long.MIN_VALUE`（负无穷）和`Long.MAX_VALUE`（正无穷）

### 2. 递归方法 `isValidBST(TreeNode node, long left, long right)`
- **终止条件**：节点为`null`，返回`true`（空树是合法BST）
- **核心判断**：
  1. 当前节点值必须严格大于左边界、严格小于右边界
  2. 递归验证左子树：左边界不变，右边界更新为**当前节点值**
  3. 递归验证右子树：左边界更新为**当前节点值**，右边界不变
- 所有条件同时满足才返回`true`，否则为`false`

## 关键细节
1. **为什么用 `long` 类型？**
   节点值范围是`int`，如果节点值为`Integer.MAX_VALUE`或`Integer.MIN_VALUE`，用`int`作为边界会导致区间判断失效，`long`可以避免溢出。

2. **严格大于/小于**
   二叉搜索树**不允许节点值相等**，必须使用`<`和`>`，不能用`<=`/`>=`。

3. **递归逻辑**
   自上而下传递区间约束，每个子节点都继承父节点的约束，保证整棵树满足BST规则。

## 复杂度分析
- **时间复杂度**：$O(n)$，每个节点仅遍历一次
- **空间复杂度**：$O(h)$，$h$为树的高度，递归调用栈的深度（最坏情况：树为链状，$h=n$）

---

### 总结
1. 核心：**区间约束 + 递归**，为每个节点限定合法取值范围
2. 重点：用`long`避免边界溢出，严格校验节点值区间
3. 优点：逻辑清晰、代码简洁，覆盖所有边界场景


# LeetCode 98 验证二叉搜索树（中序遍历解法）笔记
这是**二叉搜索树中序遍历性质**的经典解法：**二叉搜索树的中序遍历结果一定是严格递增序列**，利用这个特性可以极简验证BST。

## 题目回顾
判断一棵二叉树是否为有效的二叉搜索树（BST）
- 左子树所有节点 < 当前节点
- 右子树所有节点 > 当前节点
- 左右子树也必须是BST

## 核心原理
二叉搜索树 **中序遍历（左→根→右）** 得到的序列一定是**严格递增**的。
- 遍历过程中记录上一个节点值 `pre`
- 当前节点值必须 > `pre`，否则不是合法BST

## 修正后完整代码
你提供的代码存在**方法名、参数、变量名错误**，我修正为可直接提交 LeetCode 的标准版本：
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
    // 记录中序遍历的上一个节点值，初始化为 long 最小值避免边界问题
    private long pre = Long.MIN_VALUE;

    public boolean isValidBST(TreeNode root) {
        // 递归出口：空节点合法
        if (root == null) {
            return true;
        }

        // 1. 递归遍历左子树
        if (!isValidBST(root.left)) {
            return false;
        }

        // 2. 验证当前节点：必须 > 上一个节点值（严格递增）
        if (root.val <= pre) {
            return false;
        }
        // 更新上一个节点值为当前节点值
        pre = root.val;

        // 3. 递归遍历右子树
        return isValidBST(root.right);
    }
}
```

## 代码解析
### 1. 全局变量 pre
- `private long pre = Long.MIN_VALUE;`
- 作用：**记录中序遍历中上一个节点的值**
- 使用 `long`：避免节点值为 `Integer.MIN_VALUE` 时判断失效

### 2. 递归逻辑（中序遍历：左 → 根 → 右）
1. **遍历左子树**
   优先递归左子树，如果左子树不合法，直接返回 `false`
2. **验证当前节点**
   当前节点值必须 **严格大于** 上一个节点值，否则违反递增规则
3. **更新前驱值**
   把 `pre` 更新为当前节点值，给下一个节点判断使用
4. **遍历右子树**
   递归验证右子树，返回最终结果

### 3. 递归终止条件
`root == null` → 空节点是合法BST，返回 `true`

## 关键细节
1. **严格递增**
   必须用 `root.val <= pre`，BST 不允许重复值
2. **为什么用 long**
   当节点值为 `Integer.MIN_VALUE` 时，`int` 无法表示比它更小的值，会导致误判
3. **遍历顺序**
   严格遵循 **左子树 → 当前节点 → 右子树**，这是中序遍历核心

## 复杂度分析
- **时间复杂度**：$O(n)$
  每个节点仅遍历一次
- **空间复杂度**：$O(h)$
  $h$ 为树高度，递归调用栈开销（最坏链状树 $h=n$，最优平衡树 $h=logn$）

## 两种解法对比
| 解法         | 核心思想           | 优点                     |
|--------------|--------------------|--------------------------|
| 递归区间法   | 限定节点取值范围   | 直观易懂，容易理解       |
| 中序遍历法   | 利用BST递增性质    | 代码更简洁，无多余参数   |

---

### 总结
1. 核心：**BST 中序遍历 = 严格递增序列**
2. 思路：递归中序遍历 + 记录前驱节点 + 验证递增
3. 注意：使用 `long` 避免边界溢出，严格判断递增关系
