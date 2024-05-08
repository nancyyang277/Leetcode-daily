# Day32
## Leetcode 122 BestTime to Buy and Sell Stock2
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

### Summary
- Look for the bestBuy (Math.max) by comparing if we buy the item now (-1 * prices[i]), or previous maxSell - prices[i] if we choose to buy the item today.
- Update bestBuy by comparing previous bestBuy and if we choose to buy the item today
- Look for the bestSell (Math.max) if we sell the item now (prices[i] + bestBuy), we could only sell if we have an item in hand, so no (1 * prices[i])
- Update bestSell by comparing previous bestSell and if we choose to buy the item today
- Update maxProfit by comparing the previous maxProfit and newest bestSell
- Runtime: O(n)
- Space: O(1)

## Leetcode 55 Jump Game
### Problem
You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.


Example 1:
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
Example 2:
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

### Solution
```
public class Solution {
    public boolean canJump(int[] nums) {
        int lastPos = nums.length - 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (i + nums[i] >= lastPos) {
                lastPos = i;
            }
        }
        return lastPos == 0;
    }
}
```

### Summary
- Start from the end of the array, if (i + nums[i]) >= lastPos where lastPos is the end of array, it means if we start at position i and we can make nums[i] jumps
and if i + nums[i] >= the last position that we can land on that could help us reach the last index, then we know if we start at i, we can land on the last index as well
- update lastPos accordingly, if lastPos is not 0 after the loop which means we can't start at position 0 and reach the last index
- Runtime: O(n)
- Space: O(1)


### Leetcode 45 Jump Game II
### Problem
You are given a 0-indexed array of integers nums of length n. You are initially positioned at nums[0].

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at nums[i], you can jump to any nums[i + j] where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach nums[n - 1]. The test cases are generated such that you can reach nums[n - 1].

 

Example 1:
```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
Example 2:
```
Input: nums = [2,3,0,1,4]
Output: 2
```

### Solution
```
class Solution {
    public int jump(int[] nums) {
        int newStart = 0;
        int newEnd = 0;
        int ans = 0;
        for (int i = 0;i < nums.length - 1; i++) {
            newEnd = Math.max(newEnd, i + nums[i]);
            if (i == newStart) {
                ans++;
                newStart = newEnd;
            }
        }
        return ans;
    }
}
```

### Summary
- 







