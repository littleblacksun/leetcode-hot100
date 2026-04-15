<img width="688" height="786" alt="image" src="https://github.com/user-attachments/assets/41c9c5f4-ca30-4cde-9e08-4c7b9fb87bc6" />


<img width="1276" height="1141" alt="image" src="https://github.com/user-attachments/assets/9eb6b5f3-6278-49af-91de-3660489faca2" />


# LeetCode 102. 二叉树的层序遍历 最优简洁版题解笔记
## 题目描述
给你二叉树的根节点 `root`，返回其节点值的**层序遍历**（即逐层地，从左到右访问所有节点）。

## 解题思路
采用**迭代式 BFS（广度优先搜索）**，核心思路：
1. 用列表保存**当前层**的所有节点，从根节点开始；
2. 遍历当前层所有节点：收集节点值、收集下一层节点；
3. 逐层迭代，直到没有下一层节点为止；
4. 最终返回按层存储的结果列表。

这种写法**无队列、代码极简、可读性极强**，是面试/刷题优选版本。

## 完整代码（Java）
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // 空树直接返回空列表
        if(root == null) {
            return List.of();
        }

        // 最终结果：存储每一层的节点值
        List<List<Integer>> ans = new ArrayList<>();
        // 当前层节点列表：初始化为根节点
        List<TreeNode> cur = List.of(root);

        // 迭代遍历每一层
        while(!cur.isEmpty()){
            // 存储下一层的所有节点
            List<TreeNode> nxt = new ArrayList<>();
            // 存储当前层的所有节点值
            List<Integer> vals = new ArrayList<>(cur.size());

            // 遍历当前层所有节点
            for(TreeNode node : cur){
                // 收集当前节点值
                vals.add(node.val);
                // 把左孩子加入下一层
                if(node.left != null) {
                    nxt.add(node.left);
                }
                // 把右孩子加入下一层
                if(node.right != null) {
                    nxt.add(node.right);
                }
            }

            // 更新当前层为下一层，继续循环
            cur = nxt;
            // 把当前层结果加入总答案
            ans.add(vals);
        }
        return ans;
    }
}
```

## 逐行核心代码解析
### 1. 空节点边界处理
```java
if(root == null) {return List.of();}
```
- 二叉树为空时，直接返回空列表，避免空指针异常。

### 2. 结果容器初始化
```java
List<List<Integer>> ans = new ArrayList<>();
```
- **大列表 `ans`**：存储最终层序遍历结果，内部每个小列表对应一层的节点值。

### 3. 当前层节点初始化
```java
List<TreeNode> cur = List.of(root);
```
- **`cur` 列表**：保存**当前正在遍历的层**的所有节点；
- 初始值为根节点，代表从二叉树第一层开始遍历。

### 4. 层序遍历主循环
```java
while(!cur.isEmpty())
```
- 循环条件：当前层有节点就继续遍历，无节点则结束。

### 5. 下一层 & 当前层值容器
```java
List<TreeNode> nxt = new ArrayList<>();
List<Integer> vals = new ArrayList<>(cur.size());
```
- `nxt`：收集当前层所有节点的子节点，作为**下一层**节点；
- `vals`：收集当前层所有节点的数值，用于存入最终结果。

### 6. 遍历当前层节点
```java
for(TreeNode node : cur){
    vals.add(node.val);
    if(node.left != null) nxt.add(node.left);
    if(node.right != null) nxt.add(node.right);
}
```
- 遍历当前层每一个节点：
  1. 把节点值加入当前层值列表；
  2. 把非空的左右子节点加入下一层列表。

### 7. 迭代更新 & 保存结果
```java
cur = nxt;
ans.add(vals);
```
- 将下一层节点赋值给当前层，进入下一轮循环；
- 将当前层的值列表加入最终结果。

## 复杂度分析
- **时间复杂度**：$O(n)$
  二叉树每个节点仅被访问一次，$n$ 为节点总数。
- **空间复杂度**：$O(n)$
  列表存储节点，最多存储一层节点，完美二叉树最后一层节点数为 $n/2$，等价于 $O(n)$。

## 示例演示
输入二叉树：
```
    3
   / \
  9  20
    /  \
   15   7
```
遍历过程：
1. 第一层 `cur=[3]` → `vals=[3]`，下一层 `nxt=[9,20]`；
2. 第二层 `cur=[9,20]` → `vals=[9,20]`，下一层 `nxt=[15,7]`；
3. 第三层 `cur=[15,7]` → `vals=[15,7]`，下一层 `nxt=[]`；
4. 循环结束，返回结果：`[[3],[9,20],[15,7]]`。

---

### 总结
1. 本解法用**列表迭代**实现 BFS，替代传统队列，代码更简洁易懂；
2. 核心逻辑：**保存当前层 → 收集值 → 生成下一层 → 迭代循环**；
3. 边界处理、空间利用率、代码简洁度均为最优，适合面试手写。
