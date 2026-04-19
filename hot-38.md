<img width="942" height="1034" alt="image" src="https://github.com/user-attachments/assets/355dd57d-28ab-401b-abfe-92c8a9b79f96" />


# LeetCode 437 路径总和 III · 全网最通俗讲解
我保证用**大白话+例子+逐行翻译**，让你彻底看懂这个**前缀和 + 回溯**解法！

先记住题目要求：
**统计二叉树中，路径和 = targetSum 的路径总数。**
路径规则：**必须从上往下走，但可以从任意节点开始、任意节点结束**

---

# 一、先懂核心概念：前缀和（超级简单）
举个例子：
路径：`1 → 2 → 3`
前缀和就是：
- 到1：`1`
- 到2：`1+2=3`
- 到3：`1+2+3=6`

**公式：**
`某一段路径和 = 后面的前缀和 - 前面的前缀和`

比如：
`2→3` 的和 = `6 - 1 = 5`

---

# 二、这道题的核心逻辑（一句话）
我们在遍历树时，记录**走到当前节点的总前缀和 s**
如果 **s - targetSum** 这个值**之前出现过**
说明**中间某一段路径和正好 = targetSum**
出现几次，就加几条路径！

---

# 三、用超级小例子演示
树：
```
1
 \
  2
   \
    3
```
targetSum = 5

遍历过程：
1. 走到1，s=1
   找 s-target = 1-5=-4 → 没出现过
2. 走到2，s=3
   找 3-5=-2 → 没出现过
3. 走到3，s=6
   找 6-5=1 → **1 之前出现过！**
   ✅ 找到一条路径：`2→3`

---

# 四、代码逐行翻译（你那版代码）
我一行一行给你讲人话，保证听懂！

```java
class Solution {
    private int ans; // 最终答案：统计有多少条路径

    public int pathSum(TreeNode root, int targetSum) {
        Map<Long,Integer> cnt = new HashMap<>();
        cnt.put(0L,1); // 关键：前缀和0，出现1次（为了刚好等于target的情况）
        dfs(root,0,targetSum,cnt);
        return ans;
    }

    // node：当前节点
    // s：从根走到这里的【前缀和】
    // cnt：记录【每个前缀和出现了几次】
    private void dfs(TreeNode node, long s, int targetSum,Map<Long,Integer> cnt){
        if(node == null){
            return ; // 空节点直接返回
        }

        // 1. 加上当前节点的值，更新前缀和
        s += node.val;

        // 2. 核心！
        // 看看 s - targetSum 出现过几次 = 有几条满足条件的路径
        ans += cnt.getOrDefault(s-targetSum,0);

        // 3. 把当前前缀和 s 记录到 map 里
        cnt.merge(s,1,Integer::sum);

        // 4. 递归左右子树
        dfs(node.left , s , targetSum , cnt);
        dfs(node.right , s , targetSum , cnt );

        // 5. 回溯！
        // 离开这个节点，要把刚才加的前缀和删掉（不影响其他分支）
        cnt.merge(s,-1,Integer::sum);
    }
}
```

---

# 五、4个关键步骤，背下来就会写
1. **算当前前缀和 s**
2. **查 s - target 出现过几次 → 加到答案**
3. **把 s 放进 map**
4. **递归完左右 → 把 s 从 map 删掉（回溯）**

---

# 六、为什么要回溯（删掉s）？
因为二叉树有**左右两个分支**，它们是**互不干扰**的！
- 走左分支时用的前缀和
- 不能影响右分支
所以递归回来必须**撤销操作**

---

# 七、为什么要放 cnt.put(0L,1)？
举个例子：
路径 `1 → 4`，target=5
走到4时 s=5
`s-target = 0`
如果没有 `0:1`，就**找不到这条路径**！
它是为了处理**从根节点开始的路径**。

---

# 八、终极口诀（超好记）
```
前缀和，走一遍
s减目标查几遍
查到几次加几条
记完递归要还原
```

---

### 总结
1. 这是**前缀和 + DFS + 回溯**
2. 核心公式：`路径和 = s - 前面的s`
3. `cnt` 存前缀和出现次数
4. 递归完必须**回溯删除**，避免分支干扰

你现在是不是完全看懂了？这个解法是这道题**最优解 O(n)**，面试必考！
