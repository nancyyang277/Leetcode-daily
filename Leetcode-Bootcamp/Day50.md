# Day 50
## Leetcode309 Best time to Buy and Sell Stock with Cooldown
### Problem
You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

Example 1:
```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```
Example 2:
```
Input: prices = [1]
Output: 0
```

### Solution
```
class Solution {
    public int maxProfit(int[] prices) {
        int[][] record = new int[prices.length][3];
        // [0]->cooldown, [1] -> buy, [2] -> sell
        record[0][0] = 0;
        record[0][1] = (-1) * prices[0];
        record[0][2] = 0;
        for (int i = 1; i < prices.length; i++) {
            record[i][0] = Math.max(record[i-1][2], record[i-1][0]);
            record[i][1] = Math.max(record[i-1][0] - prices[i], record[i-1][1]);
            record[i][2] = record[i-1][1] + prices[i];
            
        }
        int profit = Math.max(record[prices.length-1][2], record[prices.length-1][0]);
        return profit > 0 ? profit : 0;
      
    }
}
```
### Summary
- define a state machine to model
  - state sell: in this state, the agent has just sold a stock right before entering this state. And the agent holds no stock at hand.
  - state buy: in this state, the agent holds a stock that it bought at some point before.
  - state cooldown: first of all, one can consider this state as the starting point, where the agent holds no stock and did not sell a stock before.
- For updating each cell:
  - If we choose to sell at state i, then state i - 1 could be buy, so record[i][sell] = record[i-1][buy] + prices[i]
  - If we choose to buy at state i, then state i - 1 could be buy which means we inherit from state i - 1 buy, or state i - 1 could be cooldown according to the rule, we take the maximum between these two
  - If we choose to cooldown at state i, then state i - 1 could also be cooldown or state i - 1 could be sell, take maximum among two
  - return the maximum of cooldown and sell at record[prices.length-1], if profit is smaller than zero, return 0

## Leetcode 714 Best Time to Buy and Sell Stock with Transaction fee
### Problem
You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

Note:

You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
The transaction fee is only charged once for each stock purchase and sale.
 

Example 1:
```
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```
Example 2:
```
Input: prices = [1,3,7,5,10,3], fee = 3
Output: 6
```

### Solution
```
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int[][] dp = new int[prices.length][2];
        // buy
        dp[0][0] = -1 * prices[0];
        //sell
        dp[0][1] = 0;

        for (int i = 1; i < prices.length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], Math.max(-1 * prices[i], dp[i - 1][1] - prices[i]));
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }
        return dp[prices.length - 1][1];
    }
}
```

### summary
- same thought process as before but to add transaction fee




