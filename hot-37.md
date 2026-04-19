<img width="969" height="704" alt="image" src="https://github.com/user-attachments/assets/a389fa7e-cd71-46f5-b672-4f1d94c9dd5a" />


# LeetCode105 前序+中序重建二叉树
**大白话+画图+一步步例子，零基础秒懂**
先记住两个遍历规则：
1. **前序 preorder**：根 → 左子树 → 右子树
2. **中序 inorder**：左子树 → 根 → 右子树

## 这个代码思路：纯分治递归，超级直观
### 一句话核心逻辑
1. 前序**第一个数，一定是根节点**
2. 去中序里找到这个根
3. 中序：根左边=左子树，右边=右子树
4. 算出左右子树长度，把前序、中序**切成4段小数组**
5. 递归造左子树、递归造右子树
6. 根连上左右孩子，返回

---

# 举最简单例子一看就懂
题目数组：
- 前序：`[1,2,3]`
- 中序：`[2,1,3]`

## 步骤1：找根
前序第一个：**1 是根**

## 步骤2：去中序找根位置
中序 `[2 , 1 , 3]`
- 1左边：`[2]` → **左子树大小 = 1**
- 1右边：`[3]` → 右子树

## 步骤3：切割4个数组
1. 左子树前序：pre 从1开始，切长度1 → `[2]`
2. 右子树前序：pre 剩下后面 → `[3]`
3. 左子树中序：in 0~1 → `[2]`
4. 右子树中序：in 2往后 → `[3]`

## 步骤4：递归
- build(pre=[2],in=[2]) → 造出左节点2
- build(pre=[3],in=[3]) → 造出右节点3

## 步骤5：拼接
`new TreeNode(1, 左2 , 右3)`
得到树：
```
  1
 / \
2   3
```

---

# 逐行翻译你的代码
```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    int n = preorder.length;
    if(n==0){
        return null; // 数组空，没有节点
    }

    // 1. 前序第一个 = 根
    // 2. 去中序找根下标，左边个数就是左子树节点总数
    int leftSize = indexOf(inorder,preorder[0]);

    // ========== 切割前序数组 ==========
    // 前序：根 左 右
    // 左子树前序：第1 ~ 1+leftSize
    int[] pre1 = Arrays.copyOfRange(preorder,1,1+leftSize);
    // 右子树前序：剩下全部
    int[] pre2 = Arrays.copyOfRange(preorder,1+leftSize,n);

    // ========== 切割中序数组 ==========
    // 中序：左 根 右
    // 左子树中序：0 ~ leftSize
    int[] in1 = Arrays.copyOfRange(inorder,0,leftSize);
    // 右子树中序：根下一位到末尾
    int[] in2 = Arrays.copyOfRange(inorder,1+leftSize,n);

    // 递归造左、递归造右
    TreeNode left = buildTree(pre1,in1);
    TreeNode right = buildTree(pre2,in2);

    // 根节点 连上左右孩子
    return new TreeNode(preorder[0],left,right);
}
```

---

# 辅助函数 indexOf
```java
private int indexOf(int[] a, int x){
    for (int i=0;;i++){
        if(a[i]==x){
            return i;
        }
    }
}
```
就是：**在中序数组里，找到根节点在哪一位**
这个位置左边长度 = 整棵左子树一共有多少个节点

---

# 再来一个经典大树例子
前序：`[3,9,20,15,7]`
中序：`[9,3,15,20,7]`

1. 根 = 3
2. 中序里3下标=1 → **左子树大小=1**
3. 切分：
   - 左前：`[9]`，左中：`[9]`
   - 右前：`[20,15,7]`，右中：`[15,20,7]`
4. 递归建好左右
最终树：
```
    3
   / \
  9  20
    / \
   15  7
```

---

# 三句话终极总结
1. **前序定根**
2. **中序分左右**
3. 切数组 → 递归左右子树 → 拼回去

---

要不要我顺便给你对比
105重建树 + 114展开链表
两种递归思路区别，你二叉树递归直接通透？
