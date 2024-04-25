# Day 21
## Leetcode 450 Delete Node in a BST
### Problem
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
 

Example 1:
<img width="389" alt="Screenshot 2024-04-24 at 11 34 41 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/8e3cef01-74ff-4a4b-9f6c-05e54c6ac985">

```
Input: root = [5,3,6,2,4,null,7], key = 3
Output: [5,4,6,2,null,null,7]
Explanation: Given key to delete is 3. So we find the node with value 3 and delete it.
One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.
```
### Solution
```
class Solution {
  /*
  One step right and then always left
  */
  public int successor(TreeNode root) {
    root = root.right;
    while (root.left != null) root = root.left;
    return root.val;
  }

  /*
  One step left and then always right
  */
  public int predecessor(TreeNode root) {
    root = root.left;
    while (root.right != null) root = root.right;
    return root.val;
  }

  public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) return null;

    // delete from the right subtree
    if (key > root.val) root.right = deleteNode(root.right, key);
    // delete from the left subtree
    else if (key < root.val) root.left = deleteNode(root.left, key);
    // delete the current node
    else {
      // the node is a leaf
      if (root.left == null && root.right == null) root = null;
      // the node is not a leaf and has a right child
      else if (root.right != null) {
        root.val = successor(root);
        root.right = deleteNode(root.right, root.val);
      }
      // the node is not a leaf, has no right child, and has a left child    
      else {
        root.val = predecessor(root);
        root.left = deleteNode(root.left, root.val);
      }
    }
    return root;
  }
}
```
### Summary
if the node's val equals to the root's val, we would first go through its right subtree and find the node with the smallest val but greater than node's val;
if it does not have right subtree, then go through its left subtree and find the node with the largest val that is smaller than node's val
And this can go in the opposite way as well. 

<img width="594" alt="Screenshot 2024-04-24 at 11 38 49 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/54420ae4-6bd2-41f4-865d-abccd3a0ef71">

## Leetcode 701 Insert into a Binary tree
### Problem
You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

 

Example 1:

<img width="600" alt="Screenshot 2024-04-24 at 11 39 56 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/11dd75e7-7388-4464-94e5-d3cb7bbe05a5">
```
Input: root = [4,2,7,1,3], val = 5
Output: [4,2,7,1,3,5]
Explanation: Another accepted tree is:
```
<img width="444" alt="Screenshot 2024-04-24 at 11 40 10 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/fb7611b6-59d9-4bca-9f11-bf66db55948b">

### Solution
```
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) 
        return new TreeNode(val);
        
        if(val>root.val)
        root.right = insertIntoBST(root.right,val);

        else if(val<root.val) root.left = insertIntoBST(root.left,val);

        return root;
    }
}
```
### Summary
if the val is greater than root, insert to right, else insert to left. When reach to the leaf node, build and return the inserted node.
Don't overthink

