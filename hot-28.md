
<img width="775" height="814" alt="image" src="https://github.com/user-attachments/assets/dfd264a2-3cdc-48c9-998a-d1b851d92712" />

题目很简单，直接上题解了

class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null){
            return root ;
        }
        
        invertTree(root.left);
        invertTree(root.right);
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        return root;
    }
}
