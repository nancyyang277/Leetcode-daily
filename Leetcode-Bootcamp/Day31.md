# Day31
## Leetcode 455 Assign Cookies (Greedy)
### Problem
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

 

Example 1:
```
Input: g = [1,2,3], s = [1,1]
Output: 1
```
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
Example 2:
```
Input: g = [1,2], s = [1,2,3]
Output: 2
```
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.

### Solution
```
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int res = 0;
        for (int i = s.length - 1, j = g.length - 1; i >= 0 && j >= 0; ) {
            if (s[i] >= g[j]) {
                res++;
                i--;
                j--;
            } else {
                j--;
            }
        }
        return res;
    }
}
```
### Summary
- - Sort two arrays, and do a for-loop on both arrays and if s[i] >= g[j], then we could decrement both i and j and update our result, if not,
- it means that the cookie can't reach the student's greedy factor, then we decrement i to see if it could
- match a student with lower greedy factor
- - Runtime: O(n)
  - Space: O(n)
    
## Leetcode 376 Wiggle Subsequence
### Problem
A wiggle sequence is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

For example, [1, 7, 4, 9, 2, 5] is a wiggle sequence because the differences (6, -3, 5, -7, 3) alternate between positive and negative.
In contrast, [1, 4, 7, 2, 5] and [1, 7, 4, 5, 5] are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.
A subsequence is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array nums, return the length of the longest wiggle subsequence of nums.

Example 1:
```
Input: nums = [1,7,4,9,2,5]
Output: 6
```
Explanation: The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).
Example 2:
```
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
```
Explanation: There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).
Example 3:
```
Input: nums = [1,2,3,4,5,6,7,8,9]
Output: 2
```

### Solution
```
class Solution {
    public int wiggleMaxLength(int[] nums) {
        if (nums.length == 1) {
            return 1;
        }
        ArrayList<Integer> diff = new ArrayList<>();
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] - nums[i-1] != 0) {
                diff.add(nums[i] - nums[i-1]);
            }
        }
        // 0 positive, 1 negative
        
        int count = 1;
        if (diff.size() == 0) {
            return count;
        }
        int prev = diff.get(0) < 0 ? 1 : 0;
        for (Integer d : diff) {
            if (d > 0) {
                if (prev != 0) {
                    count++;
                }
                prev = 0;
            } else if (d < 0) {
                if (prev != 1) {
                    count++;
                }
                prev = 1;
            }
        }
        return count + 1;
    }
}
```

### Summary
- we find a diff arraylist that calculates the difference between the current number and its previous number
- if the previous diff is negative and the current difference is positive add to count
- if the previous diff is positive and the current difference is negative add to count
- update prev diff to current diff
- Time Complexity: O(n)
- Space Complexity: O(n)

## Leetcode 53 Maximum Subarray
### Problem
Given an integer array nums, find the 
subarray
 with the largest sum, and return its sum.

 

Example 1:
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
```
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
Example 2:
```
Input: nums = [1]
Output: 1
```
Explanation: The subarray [1] has the largest sum 1.
Example 3:
```
Input: nums = [5,4,-1,7,8]
Output: 23
```
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.

### Solution
```
class Solution {
    public int maxSubArray(int[] nums) {
        int[] result = new int[nums.length];
        // Arrays.fill(result, Integer.MIN_VALUE);
        result[0] = nums[0];
        int max = nums[0];
        for (int i = 1; i < result.length; i++) {
            if (nums[i] + result[i-1] > nums[i]) {
                result[i] = nums[i] + result[i-1];
            } else {
                result[i] = nums[i];
            }
            max = Math.max(result[i], max);
        
        }
        return max;
    }
}
```

### Summary
- If we want to include the current number, it either would be the start of a new subarray or the new number to the previous subarray that includes the number before it
- compare the sum of adding the current number to the previous sum (sum[i-1]) and its value (start of new subarray), and store the value in sum[i]
- find the max value in sum array
- Time Complexity: O(n)
- Space Complexity: O(n)
