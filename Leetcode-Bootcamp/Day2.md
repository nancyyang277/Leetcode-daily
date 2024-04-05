# Day 1 4/3/2024
## Leetcode 977 Squares of a Sorted Array (Easy)
### Problem
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

#### constraints:
nums is sorted in non-decreasing order.

### Solution:
```
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] result = new int[nums.length];
        int mid = nums.length / 2;
        int lowest = -1;
        // find the smallest number's index
        for (int i = 0; i < nums.length - 1; i++) {
            if (Math.abs(nums[i]) < Math.abs(nums[i+1])){
                lowest = i;
                break;
            }
        }
        if (lowest == - 1) {
            lowest = nums.length - 1;
        }
        result[0] = nums[lowest] * nums[lowest];
        int left = lowest - 1;
        int right = lowest + 1;
        int index = 0;
        while (!(left < 0 && right >= nums.length) && index < nums.length - 1) {
            index++;
            if (left < 0) {
                result[index] = nums[right] * nums[right];
                right++;
                continue;
            } 
            if (right >= nums.length) {
                result[index] = nums[left] * nums[left];
                left--;
                continue;
            }
            if (Math.abs(nums[left]) <= Math.abs(nums[right])) {
                result[index] = nums[left] * nums[left];
                left--;
            } else {
                result[index] = nums[right] * nums[right];
                right++;
            }
            
        }
        return result;
    }
}
```
### Summary

Runtime: O(n)

Space: O(n)


## Leetcode 209 Minimum Size Subarray Sum
### Problem
Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

### Solution
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;
        int slow = 0;
        int fast = 0;
        int temp= 0;
        while (fast <= nums.length) {
            if (temp >= target) {
                result = Math.min(result, fast - slow);
                if (slow == fast) {
                    return result;
                }
                temp -= nums[slow];
                slow++;
            } else {
                if (fast < nums.length) {
                    temp += nums[fast];
                }
                fast++;
            }
            
        }
        if (result == Integer.MAX_VALUE) {
            return 0;
        }
        return result;
    }
}
```
### Summary

Runtime: O(n)

Space: O(n)

## Leetcode 59
### Problem




