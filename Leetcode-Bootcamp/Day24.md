# Day24
## Leetcode 669 Trim a Binary Search Tree
### Problem
Given the root of a binary search tree and the lowest and highest boundaries as low and high, trim the tree so that all its elements lies in [low, high]. Trimming the tree should not change the relative structure of the elements that will remain in the tree (i.e., any node's descendant should remain a descendant). It can be proven that there is a unique answer.

Return the root of the trimmed binary search tree. Note that the root may change depending on the given bounds.

 

Example 1:
<img width="376" alt="Screenshot 2024-05-04 at 1 42 03 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/f08767e6-84f2-4a66-8dff-d02606b3a735">
```
Input: root = [1,0,2], low = 1, high = 2
Output: [1,null,2]
```
Example 2:
<img width="382" alt="Screenshot 2024-05-04 at 1 42 15 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/20f7825f-6839-4065-8dae-a7c26645c781">
```
Input: root = [3,0,4,null,2,null,null,1], low = 1, high = 3
Output: [3,2,null,1]
```

### Solution
```
class Solution {
    public TreeNode trimBST(TreeNode root, int low, int high) {
        return helper(root, low, high);
    }

    private TreeNode helper(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }
        root.left = helper(root.left, low, high);
        root.right = helper(root.right, low, high);
        if (root.val < low) {
            return root.right;
        } else if (root.val > high) {
            return root.left;
        }
        return root;
    }
}
```

### Summary
Use post-order traversal to loop through all the elements in the tree (bottom to up) that 
- if the root is smaller than low (needed to be trimed), we know that all its left side will needed to be trimed,
thus return its right tree
- if the root is larger than high (needed to be trimed), we know that all its right side will needed to be trimed,
thus return its left tree
- otherwise, we just return the root

## Leetcode 108 Convert Sorted Array to Binary Search Tree
### Problem
Given an integer array nums where the elements are sorted in ascending order, convert it to a 
height-balanced
 binary search tree.

 

Example 1:

<img width="334" alt="Screenshot 2024-05-04 at 1 48 10 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/c7606f86-0caf-470d-ab45-742becb7b022">
```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
<img width="336" alt="Screenshot 2024-05-04 at 1 48 19 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/b84bbf01-d76c-429f-b8e8-0c14ae9b4547">
```
Example 2:
<img width="370" alt="Screenshot 2024-05-04 at 1 48 29 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/06ad7f0e-2868-42de-a2d8-3d13543fbaff">

```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```

### Solution
```
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }

    private TreeNode helper(int[] nums, int low, int high) {
        if (low > high) {
            return null;
        }
        int mid = low + (high - low) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = helper(nums, low, mid - 1);
        root.right = helper(nums, mid + 1, high);
        return root;
    }
}
```

### Summary
let low > high be the base case, otherwise if low == high, then we know the current node is the leaf node.

## Leetcode 538 Convert BST to Greater Tree
### Problem
Given the root of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:
<img width="373" alt="Screenshot 2024-05-04 at 1 50 24 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/2b22911a-ec1b-49c9-ad65-0dcdfd300bf4">

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```
Example 2:
```
Input: root = [0,null,1]
Output: [1,null,1]
```

### Solution
```
class Solution {
    int addedVal = 0;
    public TreeNode convertBST(TreeNode root) {
        helper(root);
        return root;
    }

    private void helper(TreeNode root) {
        if (root == null) {
            return;
        }
        helper(root.right);
        root.val += addedVal;
        addedVal = root.val;
        helper(root.left);
    }
}
```

### Summary
Loop through the right-most node and create a global variable to store the amount that needed to be added (traversal order: right->root->left)

