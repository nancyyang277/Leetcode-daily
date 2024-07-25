# Leetcode 189 Rotate Array (https://leetcode.com/problems/rotate-array/description/)
## Problem
Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.
<img width="369" alt="Screenshot 2024-07-24 at 11 40 33 PM" src="https://github.com/user-attachments/assets/7b9496e1-e152-4f7c-bc8f-ec42c717f20d">

## Solution
```

class Solution {
    public void rotate(int[] nums, int k) {
        //Do not return anything, modify nums in-place instead
        k = k % nums.length;
        int l = 0, r = nums.length - 1;
        while(l < r) {
            int tmp = nums[l];
            nums[l] = nums[r];
            nums[r] = tmp;
            l += 1;
            r -= 1;
        }
        l = 0;
        r = k - 1;
        while(l < r) {
            int tmp = nums[l];
            nums[l] = nums[r];
            nums[r] = tmp;
            l += 1;
            r -= 1;
        }
        l = k;
        r = nums.length - 1;
        while(l < r) {
            int tmp = nums[l];
            nums[l] = nums[r];
            nums[r] = tmp;
            l += 1;
            r -= 1;
        }
    }
}
```

## Summary
- first reverse the whole array, then reverse the first k elements of the reversed array, and then reverse the rest elements
- 
