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
    int maxStreak = 0;
    int currStreak = 0;
    int currNum = 0;
    List<Integer> ans = new ArrayList();
    
    public int[] findMode(TreeNode root) {
        dfs(root);
        
        int[] result = new int[ans.size()];
        for (int i = 0; i < ans.size(); i++) {
            result[i] = ans.get(i);
        }
        
        return result;
    }
    
    public void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        
        dfs(node.left);
        int num = node.val;
        if (num == currNum) {
            currStreak++;
        } else {
            currStreak = 1;
            currNum = num;
        }

        if (currStreak > maxStreak) {
            ans = new ArrayList();
            maxStreak = currStreak;
        }

        if (currStreak == maxStreak) {
            ans.add(num);
        }
        
        dfs(node.right);
    }
}
```
### Summary
Same idea here, using in order traversal. 


