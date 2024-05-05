# Day15
## Leetcode 226 Invert Binary Tree
### Problem
Given the root of a binary tree, invert the tree, and return its root. 

Example 1:
<img width="381" alt="Screenshot 2024-05-04 at 11 17 50 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/36fc8bef-ac19-4b41-9165-b141660488a9">

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```
Example 2:
<img width="372" alt="Screenshot 2024-05-04 at 11 18 00 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/59d5ed26-63be-49b3-aac1-5713f60e990c">

```
Input: root = [2,1,3]
Output: [2,3,1]
```
Example 3:
```
Input: root = []
Output: []
```

### Solution
```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        return helper(root);
    }

    private TreeNode helper(TreeNode root) {
        if (root == null) {
            return null;
        }
        TreeNode node = invertTree(root.left);
        root.left = invertTree(root.right);
        root.right = node;
        return root;
    }
}
```

### Summary
- Remember to use a temp node to store the left/right value before inverting
- time complexity is O(n)
- Space complexity is O(n)
  
## Leetcode 101 Symmetric Tree
### Problem
Given the root of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

Example 1:
<img width="369" alt="Screenshot 2024-05-04 at 11 20 04 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/64444c69-4744-4737-89f0-7e6a1ced4ef1">

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```
Example 2:
<img width="333" alt="Screenshot 2024-05-04 at 11 20 12 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/b862ce7c-3354-41d8-a4f7-7e10ecd3b2e8">
```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

### Solution
```
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return helper(root.left, root.right);
    }

    private boolean helper(TreeNode n1, TreeNode n2) {
        if (n1 == null && n2 == null) {
            return true;
        }
        if ((n1 == null && n2 != null) || (n1 != null && n2 == null)) {
            return false;
        }
        if (n1.val != n2.val) {
            return false;
        }
        return helper(n1.left, n2.right) && helper(n1.right, n2.left);
    }
}
```

### Summary
- Time complexity: O(n)
- Space complexity: O(n)
