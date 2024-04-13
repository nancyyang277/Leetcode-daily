# Day 8
## Reverse String 
### Problem
Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.

Example 1:
```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

### Solution
```
class Solution {
    public void reverseString(char[] s) {
        int left = 0;
        int right = s.length - 1;
        while (left < right) {
            char temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            left++;
            right--;
        }
    }
}
```

### Problem
Given a string s and an integer k, reverse the first k characters for every 2k characters counting from the start of the string.

If there are fewer than k characters left, reverse all of them. If there are less than 2k but greater than or equal to k characters, then reverse the first k characters and leave the other as original.

### Solution
```
class Solution {
    public String reverseStr(String s, int k) {
        int start = 0;
        char[] arr = s.toCharArray();
        while (s.length() - start >= 2 * k) {
            int left = start;
            int right = start + k - 1;
            while (left < right) {
                char temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
                left++;
                right--;
            }
            start += 2 * k;
        }
        if (s.length() - start < k) {
            int left = start;
            int right = s.length() - 1;
            while (left < right) {
                char temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
                left++;
                right--;
            }
        } else {
            int left = start;
            int right = start + k - 1;
            while (left < right) {
                char temp = arr[left];
                arr[left] = arr[right];
                arr[right] = temp;
                left++;
                right--;
            }
        }
        return String.valueOf(arr);
    }
}
```

### Summary
Use two pointers!!


