# Day11
### Leetcode 20
### Problem
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:
```
Input: s = "()"
Output: true
```
Example 2:
```
Input: s = "()[]{}"
Output: true
```
Example 3:
```
Input: s = "(]"
Output: false
```

### Solution
```
class Solution {
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch == ']' || ch == '}' || ch == ')') {
                if (stack.isEmpty()) {
                    return false;
                }
                char prev = stack.pop();
                if ((ch == ')' && prev != '(') || (ch == ']' && prev != '[') || (ch == '}' && prev != '{')) {
                    return false;
                }
            } else {
                stack.push(ch);
            }
        }
        if (stack.isEmpty()) {
            return true;
        }
        return false;
    }
}
```

### Summary
Pay attention to the case when there's no closing bracket (check if the stack is empty at the end) or no opening bracket (check if the stack is empty before popping)

## Leetcode 150
### Problem
You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are '+', '-', '*', and '/'.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.
 

Example 1:
```
Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9
```
Example 2:
```
Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6
```
### Solution
```
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<String> stack = new Stack<>();
        for (String token : tokens) {
            if (token.equals("+") || token.equals("-") || token.equals("/") || token.equals("*")) {
                int b = Integer.valueOf(stack.pop());
                int a = Integer.valueOf(stack.pop());
                int res = calculate(a, b, token);
                stack.push(res + "");
            } else {
                stack.push(token);
            }
        }
        return Integer.valueOf(stack.pop());
    }
    public int calculate(int a, int b, String operator) {
        if (operator.equals("+")) {
            return a + b;
        } else if (operator.equals("-")) {
            return a - b;
        } else if (operator.equals("/")) {
            return a / b;
        } else {
            return a * b;
        }
    }
}
```

## Leetcode 1047
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
