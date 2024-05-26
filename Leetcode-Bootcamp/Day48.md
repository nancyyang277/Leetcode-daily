# Day48
## Leetcode121 Best Time to Buy and Sell Stock
### Problem
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

 

Example 1:
```
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```
Example 2:
```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.
```

### Solution
```
class Solution {
    public int maxProfit(int[] prices) {
        int max = 0;
        int buy = prices[0];
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > buy) {
                max = Math.max(max, prices[i] - buy);
            } else {
                buy = prices[i];
            }
        }
        return max;
    }
}
```

### Summary
- Decide whether to buy/sell today
  - If my current price is smaller than previous buy, better buy today
  - Otherwise it means I can sell today, compare the profit with max profit


## Leetcode 122 Best Time to Buy and Sell Stock 2
### Problem
You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

 

Example 1:
```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```
Example 2:
```
Input: prices = [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Total profit is 4.
```
Example 3:
```
Input: prices = [7,6,4,3,1]
Output: 0
Explanation: There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
```

### Solution
```
class Solution {
    public int maxProfit(int[] prices) {
        int bestSell = 0;
        int bestBuy = -1 * prices[0];
        int maxProfit = 0;
        for (int i = 1; i < prices.length; i++) {
            int currentBuy = Math.max(-1 * prices[i], bestSell - prices[i]);
            bestBuy = Math.max(bestBuy, currentBuy);
            int currentSell = bestBuy + prices[i];
            bestSell = Math.max(currentSell, bestSell);
            maxProfit = Math.max(maxProfit, bestSell);
        }
        return maxProfit;
    }
}
```

## Leetcode 337 House Robber III
### Problem
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

 

Example 1:
<img width="366" alt="Screenshot 2024-05-26 at 1 34 17 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/96c036e3-ba51-4709-a3af-a7f155e19019">

```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
Example 2:
<img width="379" alt="Screenshot 2024-05-26 at 1 34 23 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/3502f7e5-28a0-487e-961e-2241c0f64ea3">

```
Input: root = [3,4,5,1,3,null,1]
Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

### Solution
```
class Solution {
    public int rob(TreeNode root) {
        Node result = helper(root);
        return Math.max(result.include, result.exclude);
    }

    public Node helper(TreeNode root) {
        if (root.left == null && root.right == null) {
            return new Node(root.val, 0);
        }
        Node left = new Node(0, 0);
        Node right = new Node(0, 0);
        if (root.left != null) {
            left = helper(root.left);
        }
        if (root.right != null) {
            right = helper(root.right);
        }
        int compare = Math.max(left.include + right.exclude, left.exclude + right.include);
        Node temp = new Node(root.val + left.exclude + right.exclude, Math.max(left.include + right.include, Math.max(left.exclude + right.exclude, compare)));
        return temp;
    }
}

class Node {
    int include = 0;
    int exclude = 0;
    public Node(int include, int exclude) {
        this.include = include;
        this.exclude = exclude;
    }
}
```

### Summary
- Create a new class contains two fields include and exclude, include is the maximum value if we include the current node, exclude is the maximum value if we don't include this value
- for exclude, we have 4 combinations and we take the maximum among 4 combos
  - Include left + include right
  - Exclude left + exclude left
  - Include left + exclude right
  - Exclude left + include right
 - return Math.max(include, exclude) for root node
