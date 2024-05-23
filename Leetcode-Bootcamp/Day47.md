# Day47
## Leetcode 213 House Robber 2
### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

 

Example 1:
```
Input: nums = [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```
Example 2:
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
Example 3:
```
Input: nums = [1,2,3]
Output: 3
```

### Solution
```
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) return 0;

        if (nums.length == 1) return nums[0];
        // if we choose 
        int max1 = rob_simple(nums, 0, nums.length - 2);
        int max2 = rob_simple(nums, 1, nums.length - 1);

        return Math.max(max1, max2);
    }

    public int rob_simple(int[] nums, int start, int end) {
        int t1 = 0;
        int t2 = 0;

        for (int i = start; i <= end; i++) {
            int temp = t1;
            int current = nums[i];
            t1 = Math.max(current + t2, t1);
            t2 = temp;
        }

        return t1;
    }
}
```

### Summary
- Since the start house and last house are adjacent to each other, if the thief decides to rob the start house 7, they cannot rob the last house 5. Similarly, if they select last house 5, they have to start from a house with value 4. Therefore, the final solution that we are looking for is to take the maximum value the thief can rob between houses of list [7,4,1,9,3,8,6] and [4,1,9,3,8,6,5]. For each of the lists, all we need to do is to figure the maximum value the thief can get using the approach in the original House Robber Problem.
- Trivial cases:
  - If there is one house, the answer is the value of that house.
  - If there are two houses, the answer is max(house1, house2).
  - If there are three houses, you can either pick the middle house or the sum of the first and the last house. Therefore, it boils down to max(house3 + house1, house2)
 
- t1 is the maximum value reaped until house i - 1
- t2 is the maximum value reaped until house i - 2
- current is the current value for house i
- t1 and t2 initialized at 0
- If there are four houses, t1 will leave a note of the maximum value reaped until this point and move to the next house. Then t1 will compare the value of the sum of the current house and the house which t2 is in with the value of the house t1 was in. The maximum value between those two will be chosen and t2 will move into the house next to it.

- This procedure is done over and over again as long as there are houses in nums. If t1 has reached to the end of nums, t1 should have reaped the maximum amount obtainable from houses in nums.

## Leetcode 337 House Robber 3
### Problem
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

 

Example 1:
<img width="336" alt="Screenshot 2024-05-23 at 3 57 27 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/21f56870-c977-4d82-a80c-af01e7246ef1">

```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```
Example 2:
<img width="406" alt="Screenshot 2024-05-23 at 3 57 33 PM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/4487470f-3f08-4066-a7b4-64dc5d296f68">

```
Input: root = [3,4,5,1,3,null,1]
Output: 9
Explanation: Maximum amount of money the thief can rob = 4 + 5 = 9.
```

### Solution


