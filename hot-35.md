# LeetCode 199 二叉树的右视图 BFS层序遍历思路解析
## 一、你的思路判断 ✅ **完全正确，没有任何逻辑问题！**
你的方案：
1.  BFS层序遍历二叉树，用二维数组 `tmp` 存储**每一层所有节点的值**
2.  遍历二维数组，**取出每一层最后一个元素**
3.  拼接成结果数组，就是二叉树右视图

这个思路**100%贴合题目定义、逻辑严谨、无边界漏洞**，是面试最稳妥、最容易写对、不容易出错的标准写法。

---
## 二、思路原理
二叉树右视图本质：**每一层，站在右边只能看到当前层最靠右的那个节点**
- 层序遍历刚好按「从上到下、逐层遍历」
- 每一层数组末尾元素 = 该层最右侧节点 = 右视图可见节点
- 完美匹配题目「从上到下顺序返回」的要求

---
## 三、Java 完整可提交代码（严格按照你的思路实现）
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
    public List<Integer> rightSideView(TreeNode root) {
        List<List<Integer>> tmp = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root == null) return new ArrayList<>();

        queue.offer(root);
        // BFS层序遍历，记录每一层所有节点
        while(!queue.isEmpty()){
            int levelSize = queue.size();
            List<Integer> levelList = new ArrayList<>();

            for(int i=0; i<levelSize; i++){
                TreeNode curr = queue.poll();
                levelList.add(curr.val);
                // 先左后右入队，保证层内顺序从左到右
                if(curr.left != null) queue.offer(curr.left);
                if(curr.right != null) queue.offer(curr.right);
            }
            tmp.add(levelList);
        }

        // 取每一层最后一个元素，拼接答案
        List<Integer> ans = new ArrayList<>();
        for(List<Integer> level : tmp){
            ans.add(level.get(level.size()-1));
        }
        return ans;
    }
}
```

---
## 四、你的思路优缺点
### ✅ 优点
1.  **逻辑极度直观，和题目描述一一对应，零理解成本**
2.  不会写错遍历顺序、不会漏边界，新手友好，面试不容易翻车
3.  同时顺便掌握了二叉树层序遍历模板，可复用在大量树题目
4.  空树、左斜树、右斜树所有边界用例全部兼容

### ⚠️ 可以优化的小点
你用二维数组存储了**整棵树所有节点**，额外空间开销稍大
- 优化写法：不需要存整层数组，每一层只记录**最后一个节点**，遍历到层尾直接加入答案，空间复杂度更低
- 但你的写法**逻辑完全正确，面试写这个绝对满分**，优化只是锦上添花

---
## 五、复杂度分析
- 时间复杂度：$O(n)$，每个节点仅入队、出队各1次
- 空间复杂度：$O(n)$，最坏满二叉树，最后一层节点数接近 $n/2$

---
## 六、补充极简优化版（同BFS思路，不存二维数组）
```java
public List<Integer> rightSideView(TreeNode root) {
    List<Integer> ans = new ArrayList<>();
    Queue<TreeNode> q = new LinkedList<>();
    if(root == null) return ans;
    q.add(root);

    while(!q.isEmpty()){
        int n = q.size();
        for(int i=0; i<n; i++){
            TreeNode curr = q.poll();
            // 每一层最后一个节点，直接加入答案
            if(i == n-1) ans.add(curr.val);
            if(curr.left!=null) q.add(curr.left);
            if(curr.right!=null) q.add(curr.right);
        }
    }
    return ans;
}
```

---
要不要我顺便给你补一份**DFS递归写法**的右视图笔记，和你BFS思路做对比？
