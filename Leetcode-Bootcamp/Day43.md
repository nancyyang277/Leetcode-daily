# Day43
## Leetcode 377
### Problem
Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.

The test cases are generated so that the answer can fit in a 32-bit integer.

 

Example 1:
```
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```
Example 2:
```
Input: nums = [9], target = 3
Output: 0
```

### Solution
```
class Solution {

    public int combinationSum4(int[] nums, int target) {
        // minor optimization
        // Arrays.sort(nums);
        int[] dp = new int[target + 1];
        dp[0] = 1;

        for (int combSum = 1; combSum < target + 1; ++combSum) {
            for (int num : nums) {
                if (combSum - num >= 0)
                    dp[combSum] += dp[combSum - num];
                // minor optimizaton, early stopping
                // else
                //     break;
            }
        }
        return dp[target];
    }
}
```

### Summary
- dp[i] = number of combinations to sum up to sum i
- As one can see, initially all the values are set to be zero, except the first element (dp[0]) which is set to be one.
Although literally it indicates that there is one combination that sum up to zero, which does not make sense, the value is set artificially to facilitate the calculation later as one will see.
- the inner loop explains that if we want to place nums[i] as the last number of the combination, what is the number of combinations for sum index - nums[i]
- return dp[target]

## Leetcode518 Coin Change2
### Problem
You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

 

Example 1:
```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```
Example 2:
```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```
Example 3:
```
Input: amount = 10, coins = [10]
Output: 1
```

### Solution
```
class Solution {
    public int change(int amount, int[] coins) {
        int[][] dp = new int[coins.length][amount + 1];
        for (int i = 0; i < coins.length; i++) {
            for (int j = 0; j <= amount; j++) {
                if (j == 0) {
                    dp[i][j] = 1;
                    continue;
                }
                if (i == 0) {
                    if (coins[i] <= j) {
                        dp[i][j] = dp[i][j - coins[i]];
                        continue;
                    } else {
                        dp[i][j] = 0;
                        continue;
                    }  
                }
            
                if (coins[i] > j) {
                    dp[i][j] = dp[i-1][j];
                } else {
                
                    dp[i][j] = dp[i-1][j] + dp[i][j - coins[i]];
                }
            }
        }
        return dp[coins.length - 1][amount];
    }
}
```

### Summary
- edge cases are important to consider, and the columns will be all the possible amounts and rows will be all the possible coins
- when amount = 0, set dp[i][amount] = 1
