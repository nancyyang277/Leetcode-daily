# Day41
## Leetcode 416. Partition Equal Subset Sum
### Problem
Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.

 

Example 1:
```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```
Example 2:
```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

### Solution
```
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
        }
        if (sum % 2 == 1) {
            return false;
        }
        int half = sum / 2;
        Arrays.sort(nums);
        if (nums[nums.length - 1] > half) {
            return false;
        }
        if (nums[nums.length - 1] == half) {
            return true;
        }
        boolean[][] result = new boolean[nums.length][half + 1];
        for (int i = 0; i < nums.length; i++) {
            if (i == 0) {
                result[i][nums[i]] = true;
                continue;
            }
            if (i != 0 && result[i-1][half - nums[i]]) {
                return true;
            }
            result[i][nums[i]] = true;
            for (int j = 1; j < half - nums[i]; j++) {
                if (result[i-1][j]) {
                    result[i][j] = true;
                }
                result[i][j + nums[i]] = result[i - 1][j + nums[i]];
                if (result[i-1][j]) {
                    result[i][j + nums[i]] = true;
                } 
            }
        }
        return false;
    }
}
```

### Summary
- sort the array and construct an 2-d array called result to store that result(i, j) = if we include the current number nums[i] in our list, we could either reach a sum of j by adding all the element in our list
- we loop through every possible j for each round of i that sum = nums[i] + j, but if sum > half, we stop the loop, so our condition for for-loop is j + nums[i] < half -> j < half - nums[i]
- if result[i-1][j] is true, then we copy over for result[i][j] is still going to be true since we could achieve j if we include nums[i]; we could also set result[i][j + nums[i]] to be true since we now have num[i] in our list and we can achieve a sum of j + nums[i]
- if result[i-1][j] is false, result[i][j] is still false
- if result[i-1][half - nums[i] for any i, we return true, else return false
- time: O(m*n)
- space: O(n^2)
