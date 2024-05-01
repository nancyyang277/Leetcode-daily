# Day 90
## Leetcode 93 Restore IP Addresses
### Problem
A valid IP address consists of exactly four integers separated by single dots. Each integer is between 0 and 255 (inclusive) and cannot have leading zeros.

For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses, but "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses.
Given a string s containing only digits, return all possible valid IP addresses that can be formed by inserting dots into s. You are not allowed to reorder or remove any digits in s. You may return the valid IP addresses in any order.

 

Example 1:
```
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
```
Example 2:
```
Input: s = "0000"
Output: ["0.0.0.0"]
```
Example 3:
```
Input: s = "101023"
Output: ["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

### Solution
```
class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> result = new ArrayList<>();
        if (s.length() == 0 || s.length() > 12) {
            return result;
        }
        helper(new StringBuilder(""), 0, result, s, 0);
        return result;
    }

    private void helper(StringBuilder current, int count, List<String> result, String s, int index) {
        if (count == 4 && index == s.length()) {
            if (!result.contains(current.toString())) {
                result.add(current.toString().substring(0, current.length() - 1));
            }
            return;
        }
        int startIndex = index;
        for (int i = 1; i <= 3; i++) {
            if (startIndex + i > s.length()) {
                break;
            } 
            if (s.charAt(startIndex) == '0' && i == 2) {
                break;
            }
            int num = Integer.parseInt(s.substring(startIndex, startIndex + i));
            if (num >= 0 && num <= 255) {
                helper(current.append(s.substring(startIndex, startIndex + i)).append("."), count + 1, result, s, startIndex + i);
                current.delete(current.toString().length() - i - 1, current.toString().length());
            }
        }
    }
}
```
## Leetcode 78 Subset
### Problem
Given an integer array nums of unique elements, return all possible 
subsets
 (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

 

Example 1:
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```
Example 2:
```
Input: nums = [0]
Output: [[],[0]]
```

### Solution
```
class Solution {
    List<List<Integer>> ans = new ArrayList<>();
    public List<List<Integer>> subsets(int[] nums) {
        for (int i = 0; i <= nums.length; i++) {
            backtracking(nums, 0, i, new ArrayList<Integer>());
        }
        return ans;
    }

    private void backtracking(int[] nums, int start, int k, List<Integer> res) {
        if (res.size() == k) {
            ans.add(new ArrayList<>(res));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            res.add(nums[i]);
            backtracking(nums, i + 1, k, res);
            res.remove(res.size() - 1);
        }
    }
}
```

### Summary
So, this time let us loop over _the length of combination, rather than the candidate numbers, and generate all combinations for a given length with the help of backtracking technique.

Then we define a variable in each iteration called start that we start to add nums[start] to our combination and increment start by 1 each time until we complete the combination with a certain length k

## Leetcode 90 Subset II with duplicates
### Problem
Given an integer array nums that may contain duplicates, return all possible 
subsets
 (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

 

Example 1:
```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```
Example 2:
```
Input: nums = [0]
Output: [[],[0]]
```

### Solution
```
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> subsets = new ArrayList<>();
        List<Integer> currentSubset = new ArrayList<>();

        subsetsWithDupHelper(subsets, currentSubset, nums, 0);
        return subsets;
    }

    private void subsetsWithDupHelper(List<List<Integer>> subsets, List<Integer> currentSubset, int[] nums, int index) {
        // Add the subset formed so far to the subsets list.
        subsets.add(new ArrayList<>(currentSubset));

        for (int i = index; i < nums.length; i++) {
            // If the current element is a duplicate, ignore.
            if (i != index && nums[i] == nums[i - 1]) {
                continue;
            }
            currentSubset.add(nums[i]);
            subsetsWithDupHelper(subsets, currentSubset, nums, i + 1);
            currentSubset.remove(currentSubset.size() - 1);
        }
    }
}
```

### Summary
At each function call, add a new subset to the output list of subsets. Scan through all the elements in the nums array from the starting index (written in blue in the above diagram) to the end. Consider one element at a time and decide whether to keep it or not. If we haven't seen the current element before, then add it to the current list and make a recursive function call with the starting index incremented by one. Otherwise, the subset is a duplicate and so we ignore it.
The Key is to understand that since we are adding a new subset **each time** by adding a value, if this value is a duplicate, then our subset would be the same as the previous subset since all the previous elements are the same.
