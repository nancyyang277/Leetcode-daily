# Day17
## Leetcode110 Balanced Binary Tree
### Problem
Given a binary tree, determine if it is height-balanced.

Example 1:
<img width="366" alt="Screenshot 2024-05-06 at 11 14 30 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/b228c0d5-494c-41cd-ba83-d5a8beeeec76">
```
Input: root = [3,9,20,null,null,15,7]
Output: true
```
Example 2:
<img width="379" alt="Screenshot 2024-05-06 at 11 14 38 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/5e15900b-b863-4128-8c2f-67a8b9f5e397">
```
Input: root = [1,2,2,3,3,null,null,4,4]
Output: false
```
Example 3:
```
Input: root = []
Output: true
```

### Solution
```
final class TreeInfo {
    public final int height;
    public final boolean balanced;

    public TreeInfo(int height, boolean balanced) {
        this.height = height;
        this.balanced = balanced;
    }
}
class Solution {
    // Return whether or not the tree at root is balanced while also storing
    // the tree's height in a reference variable.
    private TreeInfo isBalancedTreeHelper(TreeNode root) {
        // An empty tree is balanced and has height = -1
        if (root == null) {
            return new TreeInfo(-1, true);
        }

        // Check subtrees to see if they are balanced.
        TreeInfo left = isBalancedTreeHelper(root.left);
        if (!left.balanced) {
            return new TreeInfo(-1, false);
        }
        TreeInfo right = isBalancedTreeHelper(root.right);
        if (!right.balanced) {
            return new TreeInfo(-1, false);
        }

        // Use the height obtained from the recursive calls to
        // determine if the current node is also balanced.
        if (Math.abs(left.height - right.height) < 2) {
            return new TreeInfo(Math.max(left.height, right.height) + 1, true);
        }
        return new TreeInfo(-1, false);
    }

    public boolean isBalanced(TreeNode root) {
        return isBalancedTreeHelper(root).balanced;
    }
}
```

### Summary
- we would have created a object called TreeInfo that contains two pieces of info since each time wewant to return
- a boolean (whether a balanced left/right subtree and the height of left/right subtree), if a emptry tree, return -1 as its height
- Time complexity : O(n)
- Space complexity: O(n)

## Leetcode 257 Binary Tree Paths
### Problem
Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

Example 1:
<img width="263" alt="Screenshot 2024-05-06 at 11 18 15 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/d8c2269f-c877-4114-b27d-b215181fa7de">

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```
Example 2:
```
Input: root = [1]
Output: ["1"]
```

### Solution
```
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> binaryTreePaths(TreeNode root) {
        helper(root, "");
        return res;
    }
    private void helper(TreeNode root, String path) {
        if (root.left == null && root.right == null) {
            path += root.val;
            res.add(path);
            return;
        }
        if (root.left != null) {
            helper(root.left, path + root.val + "->");
        }
        if (root.right != null) {
            helper(root.right, path + root.val + "->");
        }
    }
}
```

### Summary
- Base case: If the node is a leaf node, we add the node to the path and add to the result list
- if left sub node is not null, we go to left and find more path
- if right sub node is not null, we go to right and find more path
- Time complexity : O(n)
- Space complexity: O(n)

## Leetcode 404 Sum of Left Leaves
### Problem
Given the root of a binary tree, return the sum of all left leaves.

A leaf is a node with no children. A left leaf is a leaf that is the left child of another node.

Example 1:
<img width="309" alt="Screenshot 2024-05-06 at 11 21 24 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/e8502e42-712e-4e88-a0d8-951f845b4220">

```
Input: root = [3,9,20,null,null,15,7]
Output: 24
```
Explanation: There are two left leaves in the binary tree, with values 9 and 15 respectively.
Example 2:
```
Input: root = [1]
Output: 0
```

### Solution
```
class Solution {
    int sum = 0;
    public int sumOfLeftLeaves(TreeNode root) {
        helper(root, "start");
        return sum;
    }

    private void helper(TreeNode root, String path) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            if (path.equals("left")) {
                sum += root.val;
            }
            return;
        }
        helper(root.left, "left");
        helper(root.right, "right");
    }
}
```

### Summary
- if the parameter path is left (the node is a left sub node), we add the val to sum
- Base case: if left and right all null means a leaf node and coming from left, we add the val to sum. 
- Time complexity : O(n)
- Space complexity: O(n)
