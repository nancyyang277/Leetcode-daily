# Day53
## Leetcode 1143 Longest Common Subsequence
### Problem
Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

 

Example 1:
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```
Example 2:
```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```
Example 3:
```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

### Solution
```
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length()][text2.length()];
        for (int i = 0; i < text1.length(); i++) {
            for (int j = 0; j < text2.length(); j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = text1.charAt(i) == text2.charAt(j) ? 1 : 0;
                    continue;
                }
                if (i == 0) {
                    dp[i][j] = text1.charAt(i) == text2.charAt(j) ? 1 : 0;
                    dp[i][j] = Math.max(dp[i][j], dp[i][j-1]);
                    continue;
                }
                if (j == 0) {
                    dp[i][j] = text1.charAt(i) == text2.charAt(j) ? 1 : 0;
                    dp[i][j] = Math.max(dp[i][j], dp[i-1][j]);
                    continue;
                }
                if (text1.charAt(i) == text2.charAt(j)) {
                    dp[i][j] = 1 + dp[i-1][j-1];
                } else {
                    dp[i][j] = dp[i - 1][j-1];
                }
                dp[i][j] = Math.max(dp[i][j], Math.max(dp[i-1][j], dp[i][j-1])); 
            }
        }
        return dp[text1.length() - 1][text2.length() - 1];
    }
}
```


## Leetcode 1035 Uncrossed Lines
### Problem
You are given two integer arrays nums1 and nums2. We write the integers of nums1 and nums2 (in the order they are given) on two separate horizontal lines.

We may draw connecting lines: a straight line connecting two numbers nums1[i] and nums2[j] such that:

nums1[i] == nums2[j], and
the line we draw does not intersect any other connecting (non-horizontal) line.
Note that a connecting line cannot intersect even at the endpoints (i.e., each number can only belong to one connecting line).

Return the maximum number of connecting lines we can draw in this way.
Example 1:
<img width="402" alt="Screenshot 2024-05-29 at 12 48 18 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/c390c489-7fde-48d9-bbb5-bc156a4806a2">

```
Input: nums1 = [1,4,2], nums2 = [1,2,4]
Output: 2
Explanation: We can draw 2 uncrossed lines as in the diagram.
We cannot draw 3 uncrossed lines, because the line from nums1[1] = 4 to nums2[2] = 4 will intersect the line from nums1[2]=2 to nums2[1]=2.
```
Example 2:
```
Input: nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
Output: 3
```
Example 3:
```
Input: nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
Output: 2
```

### Solution
```
class Solution {
    public int maxUncrossedLines(int[] nums1, int[] nums2) {
        int[][] dp = new int[nums1.length][nums2.length];
        for (int i = 0; i < nums1.length; i++) {
            for (int j = 0; j < nums2.length; j++) {
                if (i == 0 && j == 0) {
                    dp[i][j] = nums1[i] == nums2[j] ? 1 : 0;
                    continue;
                }
                if (i == 0) {
                    dp[i][j] = nums1[i] == nums2[j] ? 1 : 0;
                    dp[i][j] = Math.max(dp[i][j], dp[i][j-1]);
                    continue;
                }
                if (j == 0) {
                    dp[i][j] = nums1[i] == nums2[j] ? 1 : 0;
                    dp[i][j] = Math.max(dp[i][j], dp[i-1][j]);
                    continue;
                }
                if (nums1[i] == nums2[j]) {
                    dp[i][j] = 1 + dp[i-1][j-1];
                } else {
                    dp[i][j] = dp[i - 1][j-1];
                }
                dp[i][j] = Math.max(dp[i][j], Math.max(dp[i-1][j], dp[i][j-1])); 
            }
        }
        return dp[nums1.length - 1][nums2.length - 1];
    }
}
```

### Summary for both problems
- create a 2d array that for the row will be the elements for the first input, columns will be elements for the second input
- if first[i] == second[j] or a match, then we know we might get one potential candidate with 1 + dp[i-1][j-1] since we use element first[i] and element second[j] to become a new match which will add to the number of matches for first[0:i-1] and second[0:j-1] and compare with dp[i-1][j] and dp[i][j-1] since there might be greater values if we don't use this match
- if first[i] != second[j], then we compare dp[i-1][j-1], dp[i-1][j], dp[i][j-1] and put the maximum value for dp[i][j] since we are just inheriting previous values
- return the last updated value

