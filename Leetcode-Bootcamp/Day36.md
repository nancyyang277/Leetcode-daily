# Day36
## Leetcode 738 Monotone Increasing Digits
### Problem
An integer has monotone increasing digits if and only if each pair of adjacent digits x and y satisfy x <= y.

Given an integer n, return the largest number that is less than or equal to n with monotone increasing digits.

 
Example 1:
```
Input: n = 10
Output: 9
```
Example 2:
```
Input: n = 1234
Output: 1234
```
Example 3:
```
Input: n = 332
Output: 299
```

### Solution
```
class Solution {
    public int monotoneIncreasingDigits(int n) {
        int length = Integer.toString(n).length();
        String s = Integer.toString(n);
        
        String current = s.charAt(0) + "";
        Stack<String> stack = new Stack<>();
        stack.push(current);
       
        StringBuilder sb = new StringBuilder();
        sb.append(current);
        for (int i = 1; i < length; i++) {
            current = s.charAt(i) + "";
            if (Integer.parseInt(stack.peek()) <= Integer.parseInt(current)) {
                stack.push(current);
                sb.append(current);
            } else {
                int val = Integer.parseInt(current); //current = 0

                while (!stack.isEmpty() && Integer.parseInt(stack.peek()) > val) {
                    val = Integer.parseInt(stack.pop()) - 1;
                    sb.deleteCharAt(sb.length() - 1);
                }
                if (val != 0) {
                    sb.append(val);
                }
                for (int j = stack.size() + 1; j < length; j++) {
                    sb.append("9");
                }
                break;
            }
        }
        return Integer.parseInt(sb.toString());
    }
}
```

### Summary
- We loop through the whole number (convert int to String)
-   - If the current digit is greater than or equal to the previous digit, we add the digit to both stack and stringBuilder
    - If smaller than the previous digit, we know all the digit after this digit is going to be 9, and we look for the first digit that is not 9
    - which is if we minus the digit by 1 and it is still greater than the previous digit. We loop back (pop() and remove()) from the stack to find that starting digit.
    - Then we add the (starting digit - 1) to the stringbuilder and continue adding 9 to it from the index(stack.size() + 1), and convert the stringbuilder to int at the end and return it
    - int -> string: Integer.toString(n)
    - String -> int: String.valueOf(s)
