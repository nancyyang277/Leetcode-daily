# Day16
## Leetcode 111 Minimum Depth of Binary Tree
### Problem
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

Example 1:
<img width="389" alt="Screenshot 2024-05-05 at 1 14 44 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/7c39a1ba-e98c-4f28-87d5-4d66123a5391">

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```
Example 2:
```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
```

### Solution
class Solution {
    private int dfs(TreeNode root) {
        if (root == null) {
            return 0;
        }

        // If only one of child is non-null, then go into that recursion.
        if (root.left == null) {
            return 1 + dfs(root.right);
        } else if (root.right == null) {
            return 1 + dfs(root.left);
        }

        // Both children are non-null, hence call for both children.
        return 1 + Math.min(dfs(root.left), dfs(root.right));
    }

    public int minDepth(TreeNode root) {
        return dfs(root);
    }
}

### Summary
- If one of the children of a node is not null, the node is not a leaf node, we need to traverse more
- for maxDepth, simply change Math.min to Math.max
- Time complexity: O(N)
- Space complexity: O(N)

## Leetcode 222 Count Complete Tree Nodes
### Problem
Given the root of a complete binary tree, return the number of the nodes in the tree.

According to Wikipedia, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.

Design an algorithm that runs in less than O(n) time complexity.

 

Example 1:
<img width="391" alt="Screenshot 2024-05-05 at 1 17 21 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/b3d762c0-e406-49fd-9d24-34f92c0789df">

```
Input: root = [1,2,3,4,5,6]
Output: 6
```
Example 2:
```
Input: root = []
Output: 0
```
Example 3:
```
Input: root = [1]
Output: 1
```

### Solution
```
class Solution {
    public int countNodes(TreeNode root) {
        
        if(root==null)
            return 0;
        int lef=0,rig=0;
        TreeNode tmp=root;
        while(tmp.left!=null) {
		
            tmp=tmp.left;
            lef++;
        }
        tmp=root;
        while(tmp.right!=null){
		
            tmp=tmp.right;
            rig++;
        }
        if(lef==rig)
            return 2*((int)Math.pow(2,lef)-1)+1;
        return countNodes(root.left)+countNodes(root.right)+1;
    }
}
```
### Summary
- in the first case, compare the left sub-tree height with the right sub-tree height. If they are equal it is a full tree, then the answer will be 2^height – 1.
- Otherwise, If they aren’t equal, recursively call for the left sub-tree and the right sub-tree to count the number of nodes.
- Time Complexity: O((log N)2)

- Calculating the height of a tree with x nodes takes (log x) time.
  Here, we are traversing through the height of the tree.
  For each node, height calculation takes logarithmic time.
  As the number of nodes is N, we are traversing log(N) nodes and calculating the height for each of them.
  So the total complexity is (log N * log N) = (log N)^2
-  O(log N)  because of Recursion Stack Space (which takes elements upto the maximum depth of a node in the tree)
