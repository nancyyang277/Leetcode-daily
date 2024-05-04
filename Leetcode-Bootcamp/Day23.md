# Day23
## Leetcode 530 minimum absolute difference in BST
### Problem
Given the root of a Binary Search Tree (BST), return the minimum absolute difference between the values of any two different nodes in the tree.

 

Example 1:
<img width="348" alt="Screenshot 2024-05-02 at 11 47 47 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/511cf065-5ee6-4397-ab2a-c392e11c4e3c">
```
Input: root = [4,2,6,1,3]
Output: 1
```
Example 2:
<img width="335" alt="Screenshot 2024-05-02 at 11 47 53 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/0d64ca9b-9a94-4088-9342-4f5c5b6845ab">
```
Input: root = [1,0,48,null,null,12,49]
Output: 1
```

### Solution
```
class Solution {
    int minDifference = Integer.MAX_VALUE;
    // Initially, it will be null.
    TreeNode prevNode;

    void inorderTraversal(TreeNode node) {
        if (node == null) {
            return;
        }

        inorderTraversal(node.left);
        // Find the difference with the previous value if it is there.
        if (prevNode != null) {
            minDifference = Math.min(minDifference, node.val - prevNode.val);
        }
        prevNode = node;
        inorderTraversal(node.right);
    }

    int getMinimumDifference(TreeNode root) {
        inorderTraversal(root);
        return minDifference;
    }
};
```
### Summary
It is always good to use in-order traversal for Binary Search Tree Problem (left->root->right) this will give the Inorder traversal gives nodes in non-decreasing order.
- Recursively perform the in-order traversal for node.left.
- We handle node now. We check its difference with the previously visited node prevNode. If prevNode != null, it means we have visited a node previously and hence, we try to update minDifference using minDifference = min(minDifference, node.val - prevNode.val).
- Recursively perform the in-order traversal for node.right.

## Leetcode 501. Find Mode in Binary Search Tree
### Problem
Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it.

If the tree has more than one mode, return them in any order.

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than or equal to the node's key.
The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:
<img width="218" alt="Screenshot 2024-05-03 at 12 58 24 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/ba6009b1-3fd0-413e-94d4-ede882bfa891">

```
Input: root = [1,null,2,2]
Output: [2]
```
Example 2:
```
Input: root = [0]
Output: [0]
```
### Solution
```
class Solution {
    TreeNode prevNode;
    int maxCount = 1;
    int count = 1;
    List<Integer> list = new ArrayList<>();
    public int[] findMode(TreeNode root) {
        helper(root);
        return list.stream().mapToInt(i -> i).toArray();
    }

    private void helper(TreeNode root) {
        if (root == null) {
            return;
        }
        helper(root.left);
        if (prevNode != null) {
            if (root.val == prevNode.val) {
                count++;
            } else {
                prevNode = root;
                count = 1;
            }
        } else {
            prevNode = root;
        }
        if (count > maxCount) {
            list.clear();
            list.add(root.val);
            maxCount = count;
        } else if (count == maxCount) {
            list.add(root.val);
        }
        helper(root.right);
    }
}
```
### Summary
Same idea here, using in order traversal. 

## Leetcode 236 Lowest Common Ancestor of a Binary Tree
### Problem
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

 

Example 1:
<img width="221" alt="Screenshot 2024-05-04 at 1 53 52 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/739a7053-68a2-422d-a2d1-0ee313f4b512">

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```
Example 2:
<img width="224" alt="Screenshot 2024-05-04 at 1 54 05 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/326fca44-691a-45b9-87d3-43350d193ded">

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

### Solution
```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        return helper(root, p, q);
    }

    private TreeNode helper(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null) {
            return null;
        }
        TreeNode left = helper(root.left, p, q);
        TreeNode right = helper(root.right, p, q);
        if (left != null && right != null) {
            return root;
        } 
    
        if (root.val == p.val || root.val == q.val) {
            return root;
        }

        if (left != null) {
            return left;
        }
        if (right != null) {
            return right;
        }
        return null;
    }
}
```

### Summary
- Traversal order (left->right->root) from bottom to up
- if root is one of the val, we return root, if its left/right subtree contains another value, we would still return the root since it is the common root
- We want to check if the left subtree contains any value, if yes, return left subtree
- if right subtree contains any value, if yes, return right subtree
- if both does not contains val, return null
- if both contains val (left/right subtree return non-null values), return the current root





