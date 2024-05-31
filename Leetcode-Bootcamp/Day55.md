# Day55
## Leetcode 647 Palindromic Substring
### Problem
Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

 

Example 1:
```
Input: s = "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
```
Example 2:
```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

### Solution
```
class Solution {
    public int countSubstrings(String s) {
        int n = s.length(), ans = 0;

        if (n == 0) 
            return 0;

        boolean[][] dp = new boolean[n][n];

        // Base case: single letter substrings
        for (int i = 0; i < n; ++i, ++ans) 
            dp[i][i] = true;

        // Base case: double letter substrings
        for (int i = 0; i < n - 1; ++i) {
            dp[i][i + 1] = (s.charAt(i) == s.charAt(i + 1));
            ans += (dp[i][i + 1] ? 1 : 0);
        }

        // All other cases: substrings of length 3 to n
        for (int len = 3; len <= n; ++len)
            for (int i = 0, j = i + len - 1; j < n; ++i, ++j) {
                dp[i][j] = dp[i + 1][j - 1] && (s.charAt(i) == s.charAt(j));
                ans += (dp[i][j] ? 1 : 0);
            }

        return ans;
    }
}
```

### Summary
- Remember that larger palindromes are made of smaller palindromes. Knowing that a string is made up of a palindrome helps us determine if the string itself is a palindrome.
- Two base cases:
  - single letter: dp[i][i] = true
  - double letter: dp[i][i+1] = true when dp[i] == dp[i+1] false otherwise
- For length from 3 to n(length of the string)
  - Its first and last characters are equal, and
  - The rest of the string (excluding the boundary characters) is also a palindrome.
  - dp[i][j] = true if dp[i+1][j-1] and dp[i] == dp[j] and false otherwise
- for each dp[i][j], if it is true, add to ans, return ans

## Leetcode 516 Longest Palindromic subsequence
### Problem
Given a string s, find the longest palindromic subsequence's length in s.

A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

 

Example 1:
```
Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".
```
Example 2:
```
Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".
```

### Solution
```
class Solution {
    public int longestPalindromeSubseq(String s) {
        StringBuilder reverse = new StringBuilder();
        reverse.append(s);
        reverse.reverse();
        return longestCommonSubsequence(s, reverse.toString());
    }
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

### Summary
- Reverse the string s and use two strings to run the longestCommonSubsequence algorithm, and the longest common subsequence is the longest palidromic in the problem
