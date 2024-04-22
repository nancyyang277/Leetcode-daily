# Day20
## Leetcode 654 Maximum Binary Tree (Medium)
### Problem
You are given an integer array nums with no duplicates. A maximum binary tree can be built recursively from nums using the following algorithm:

Create a root node whose value is the maximum value in nums.
Recursively build the left subtree on the subarray prefix to the left of the maximum value.
Recursively build the right subtree on the subarray suffix to the right of the maximum value.
Return the maximum binary tree built from nums.

 

Example 1:
<img width="402" alt="Screenshot 2024-04-22 at 3 52 46 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/6deb1a3d-636b-40b0-8735-1cd5727e7b04">
```
Input: nums = [3,2,1,6,0,5]
Output: [6,3,5,null,2,0,null,null,1]
Explanation: The recursive calls are as follow:
- The largest value in [3,2,1,6,0,5] is 6. Left prefix is [3,2,1] and right suffix is [0,5].
    - The largest value in [3,2,1] is 3. Left prefix is [] and right suffix is [2,1].
        - Empty array, so no child.
        - The largest value in [2,1] is 2. Left prefix is [] and right suffix is [1].
            - Empty array, so no child.
            - Only one element, so child is a node with value 1.
    - The largest value in [0,5] is 5. Left prefix is [0] and right suffix is [].
        - Only one element, so child is a node with value 0.
        - Empty array, so no child.
```
### Solution
```
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        return helper(nums, 0, nums.length - 1);
    }

    public TreeNode helper(int[] nums, int left, int right) {
        if (left > right) {
            return null;
        }
        int index = findMax(nums, left, right);
        TreeNode root = new TreeNode(nums[index]);
        root.left = helper(nums, left, index - 1);
        root.right = helper(nums, index + 1, right);
        return root;
    }

    public int findMax(int[] nums, int left, int right) {
        int max = -1;
        int res = 0;
        for (int i = left; i <= right; i++) {
            if (nums[i] > max) {
                res = i;
                max = nums[i];
            }
        }
        return res;
    }
}
```

## Leetcode 700 Search in a Binary Search Tree
### Problem
You are given the root of a binary search tree (BST) and an integer val.

Find the node in the BST that the node's value equals val and return the subtree rooted with that node. If such a node does not exist, return null.

 

Example 1:
<img width="408" alt="Screenshot 2024-04-22 at 3 54 07 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/48d58c48-33a4-4591-84d3-0e71fb18f608">

```
Input: root = [4,2,7,1,3], val = 2
Output: [2,1,3]
```
### Solution
```
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (val == root.val) {
            return root;
        }
        if (val > root.val) {
            return searchBST(root.right, val);
        } 
        return searchBST(root.left, val);
    }
}
```

## Leetcode 617 Merge Two Binary Trees
### Problem
You are given two binary trees root1 and root2.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return the merged tree.

Note: The merging process must start from the root nodes of both trees.

 

Example 1:
<img width="384" alt="Screenshot 2024-04-22 at 3 55 04 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/621398be-e14e-4b86-9980-2fb0f1cc344a">

```
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

### Solution
```
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) {
            return null;
        }
        TreeNode temp = null;
        if (root1 != null && root2 != null) {
            temp = new TreeNode(root1.val + root2.val);
        } else if (root1 == null) {
            temp = new TreeNode(root2.val);
        } else if (root2 == null) {
            temp = new TreeNode(root1.val);
        }
        temp.left = mergeTrees(root1 == null ? null : root1.left, root2 == null ? null : root2.left);
        temp.right = mergeTrees(root1 == null ? null : root1.right, root2 == null ? null : root2.right);
        return temp;
    }
}
```

## Leetcode 98 Validate Binary Search Tree
### Problem
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left 
subtree
 of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:
<img width="362" alt="Screenshot 2024-04-22 at 3 56 09 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/4fa9404d-2741-45bf-858d-6d3e4b6e5998">

```
Input: root = [2,1,3]
Output: true
```

### Solution
```
class Solution {
    public boolean isValidBST(TreeNode root) {
        return helper(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean helper(TreeNode root, long leftBound, long rightBound) {
        if (root == null) {
            return true;
        }
        if (root.val <= leftBound || root.val >= rightBound) {
            return false;
        }
        return helper(root.left, leftBound, root.val) && helper(root.right, root.val, rightBound);
    }
}
```

### Summary
Remember to use ```Long``` instead of ```int``` since some of the input may cause overflow
