<img width="806" height="1066" alt="image" src="https://github.com/user-attachments/assets/450e0fe8-c0b4-4c5c-badc-5093cab0ecce" />


# LeetCode 108. 将有序数组转换为二叉搜索树 极简递归题解笔记

## 题目描述
给你一个整数数组 `nums`，其中元素已经按**升序排列**，请你将其转换为一棵**高度平衡**的二叉搜索树。

**高度平衡二叉树**：一棵满足每个节点的左右两个子树的高度差的绝对值不超过 1 的二叉树。

## 解题思路
核心算法：**二分查找 + 递归构建**
1. **分治法**：有序数组的**中间元素**就是 BST 的根节点；
2. **递归构建**：
   - 中间元素左边的子数组 → 构建**左子树**
   - 中间元素右边的子数组 → 构建**右子树**
3. 这种天然二分的方式，保证了构建出的树一定是**高度平衡**的。

## 完整代码（Java）
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        // 入口：调用DFS，覆盖整个数组 [0, nums.length)
        return dfs(nums, 0, nums.length);
    }

    /**
     * 递归深度优先搜索
     * @param nums 有序数组
     * @param left 左边界（包含）
     * @param right 右边界（不包含）
     * @return 构建好的子树根节点
     */
    private TreeNode dfs(int[] nums, int left, int right) {
        // 递归终止条件：区间为空，返回 null
        if (left == right) {
            return null;
        }

        // 找中间节点：无符号右移1位，等价于 (left + right) / 2，且防止整型溢出
        int m = (left + right) >>> 1;
        
        // 递归创建节点：中间值为根，左区间建左树，右区间建右树
        return new TreeNode(nums[m], 
                           dfs(nums, left, m), 
                           dfs(nums, m + 1, right));
    }
}
```

## 代码核心解析
### 1. 递归区间定义（左闭右开）
```java
dfs(nums, 0, nums.length);
```
- 采用 **`[left, right)` 左闭右开** 区间：
  - 包含 `left`，不包含 `right`
  - 终止条件简单：`left == right` 就代表区间为空
  - 代码逻辑非常简洁，是算法竞赛常用写法

### 2. 递归终止条件
```java
if(left == right) return null;
```
- 当左右边界重合，说明没有元素可以构建节点，返回空。

### 3. 中间值计算（关键技巧）
```java
int m = (left + right) >>> 1;
```
- **`>>>` 无符号右移**：
  - 效果 = 整数除法 `/2`
  - **优点**：避免 `left + right` 超出 int 范围导致**溢出**
  - 比 `(left + right) / 2` 更安全、更优雅

### 4. 递归建树（一行代码搞定）
```java
return new TreeNode(nums[m], dfs(左), dfs(右));
```
- 利用 Java 构造方法，**一行代码**完成：
  1. 创建当前根节点
  2. 递归构建左子树
  3. 递归构建右子树

## 复杂度分析
- **时间复杂度**：$O(n)$
  数组中每个元素只被访问一次，用于创建节点。
- **空间复杂度**：$O(\log n)$
  递归调用栈的深度为树的高度，平衡二叉树高度为 $\log n$。

## 示例演示
输入：`nums = [-10,-3,0,5,9]`
1. 中间值 `0` 作为根节点
2. 左区间 `[-10,-3]` → 根 `-3`
3. 右区间 `[5,9]` → 根 `9`
4. 最终生成平衡二叉搜索树：
```
       0
     /   \
   -3     9
   /     /
 -10    5
```

---

### 总结
1. 本题是**二分法**在二叉树上的经典应用；
2. **左闭右开区间** + **无符号右移** 让代码极简且安全；
3. 递归一行建树，面试手写无压力，完美满足平衡要求。
