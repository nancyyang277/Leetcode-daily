# Day54
## Leetcode 72 Edit Distance
### Problem
Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character
 

Example 1:
```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```
Example 2:
```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
```

### Solution
```
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length() + 1][word2.length() + 1];
        for (int i = 0; i <= word1.length(); i++) {
            for (int j = 0; j <= word2.length(); j++) {
                if (i == 0) {
                    dp[i][j] = j;
                } else if (j == 0) {
                    dp[i][j] = i;
                } else {
                    if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                        dp[i][j] = dp[i-1][j-1];
                    } else {
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }
                    dp[i][j] = Math.min(dp[i][j], Math.min(dp[i-1][j] + 1, dp[i][j-1] + 1));
                }
            }
        }
        return dp[word1.length()][word2.length()];
    }
}
```

### Summary
- Two cases
  - If word1[i] == word2[j], then we know we don't need replacement since [i] = [j], then we can just copy over the minimum number of operations required to convert word1[0:i-1] to word2[0:j-1]
  - If word1[i] != word2[j-1], then we know we need 1 more replacement to make [i] = [j] and add to dp[i-1][j-1]
  - set dp[i][j] to appropriate value
- For both cases, we need to see if we can insert/delete the character word1[i] or word2[j] to have less operations
  - if delete word1[i] to word1 or insert word1[i] to word2, then we add 1 more operation to dp[i-1][j]
  - if delete word2[j] to word2 or insert word2[j] to word1, then we add 1 more operation to dp[i][j-1]
  - take minimum among dp[i][j-1], dp[i - 1][j], dp[i][j]
