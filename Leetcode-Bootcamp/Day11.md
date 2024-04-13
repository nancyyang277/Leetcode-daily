# Day11
### Leetcode 20

### Leetcode 150

### Leetcode 1047
### Problem
You are given a string s consisting of lowercase English letters. A duplicate removal consists of choosing two adjacent and equal letters and removing them.

We repeatedly make duplicate removals on s until we no longer can.

Return the final string after all such duplicate removals have been made. It can be proven that the answer is unique.

 

Example 1:
```
Input: s = "abbaca"
Output: "ca"
```
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
### Solution
```
class Solution {
    public String removeDuplicates(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (stack.isEmpty()) {
                stack.push(ch);
                continue;
            }
            if (!stack.isEmpty()) {
                char temp = stack.peek();
                if (temp == ch) {
                    stack.pop();
                } else {
                    stack.push(ch);
                }
            }
        }
        StringBuilder res = new StringBuilder();
        while (!stack.isEmpty()) {
            res.insert(0, stack.pop());
        }
        return res.toString();
    }
}
```

### Summary
Using stack is important in this problem, also the problem only states to remove two duplicate letters not all which makes the problem much easier
