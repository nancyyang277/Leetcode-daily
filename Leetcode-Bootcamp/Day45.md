# Day45 Knapsack problems
## Leetcode 322 Coin Change
### Problem
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```
Example 2:
```
Input: coins = [2], amount = 3
Output: -1
```
Example 3:
```
Input: coins = [1], amount = 0
Output: 0
```

### Solution
```
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount+1];
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            dp[i] = -1;
            for (int j = 0; j < coins.length; j++) {
                if (i - coins[j] < 0) {
                    continue;
                } else if (dp[i-coins[j]] == -1) {
                    continue;
                } else if (dp[i] == -1) {
                    dp[i] = dp[i - coins[j]] + 1;
                    continue;
                }
                dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
            }
            // System.out.println(dp[i]);
        }
        return dp[amount];
    }
}
```

### Summary
- outer loop is to loop through all the amounts
- inner loop is to loop though all the coin candidates
- check if we can put this candidate into our knapsack
  - First if the coin value is greater than current amount
  - if we choose the coin, is it possible to make the amount (i - coin value) using the coin values we have, if not, we cannot select this coin
  - if it is our first time update the coin, directly update it without using Math.min since -1 is always smaller than any number of coins
  - update the minimum number of coins using dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1)


## Leetcode 279 Perfect Squares
### Problem
Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

 

Example 1:
```
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
```
Example 2:
```
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

### Solution
```
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;
        for (int i = 1; i <= n; i ++) {
            int j = 1;
            int product = j * j;
            dp[i] = -1;
            while (product <= i) {
                if (dp[i - product] == -1) {
                    continue;
                }
                if (dp[i] == -1) {
                    dp[i] = 1 + dp[i - product];
                } else {
                    dp[i] = Math.min(dp[i], 1 + dp[i - product]);
                }
                j++;
                product = j * j;
            }
        }
        return dp[n];
    }
}
```

### Summary
- Same thought process compared to coin change problem, candidate will be all the squares less than the current integer n (outer loop)
  
