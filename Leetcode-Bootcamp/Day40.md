# Day40
## Leetcode 343 Integer Break
### Problem
Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

 

Example 1:
```
Input: n = 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```
Example 2:
```
Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

### Solution
```
class Solution {
    public int integerBreak(int n) {
        int[] result = new int[n + 1];
        result[1] = 1;

        for (int i = 2; i <= n; i++) {
            int left = 1;
            int right = i - 1;
            int max = 0;
            while (left <= right) {
                int leftMax = Math.max(result[left], left);
                int rightMax = Math.max(result[right], right);
                max = Math.max(max, leftMax * rightMax);
                left++;
                right--;
            }
            result[i] = max;
        }
        return result[n];
    }
}
```


### Summary
- We initialize an array called result that for each index result[i], we stores the largest product when we break i into the sum of k positive integers, where k >= 2
- Then since k >= 2, We can try all possible splits. Iterate i from 2 until num. For each value of i, try to update ans with i * dp(num - i) if it is larger.
- The first time we calculate result[i], we will store the result. In the future, we can reference this result for num instead of having to recalculate it.
- Time complexity: O(n^2)
- Space complexity: O(n)


## Leetcode 96 Unique Binary Search Trees
### Problem
Given an integer n, return the number of structurally unique BST's (binary search trees) which has exactly n nodes of unique values from 1 to n.

 

Example 1:
<img width="386" alt="Screenshot 2024-05-16 at 12 20 23 AM" src="https://github.com/nancyyang277/Leetcode-daily/assets/165972977/1e23637d-3637-4a8d-821f-12b205ecd9f3">

```
Input: n = 3
Output: 5
```
Example 2:
```
Input: n = 1
Output: 1
```

### Solution
```
public class Solution {
    public int numTrees(int n) {
        int[] G = new int[n + 1];
        G[0] = 1;
        G[1] = 1;

        for (int i = 2; i <= n; ++i) {
            for (int j = 1; j <= i; ++j) {
                G[i] += G[j - 1] * G[i - j];
            }
        }
        return G[n];
    }
}
```

### Summary
The problem is to calculate the number of unique BST.
To do so, we can define two functions:

- G(n): the number of unique BST for a sequence of length n.

- F(i,n): the number of unique BST, e.g.F(3,7), the number of unique BST tree with the number 3 as its root.
where the number i is served as the root of BST (1 <= i <= n).
- For G(n),it does not matter the content of the sequence, but the length of the sequence.
Therefore, F(3,7)=G(2)⋅G(4).
- runtime: O(n^2)
- space: O(N)
