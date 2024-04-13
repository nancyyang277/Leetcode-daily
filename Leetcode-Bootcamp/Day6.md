# Day5
## Leetcode 242
### Problem
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 

Example 1:
```
Input: s = "anagram", t = "nagaram"
Output: true
```
Example 2:
```
Input: s = "rat", t = "car"
Output: false
```

### Solution
```
class Solution {
    public boolean isAnagram(String s, String t) {
        int[] count1 = new int[26];
        // int sum = 0;
        if (s.length() != t.length()) {
            return false;
        }
        for (int i = 0; i < s.length(); i++) {
            
            count1[s.charAt(i) - 'a']++;
        }
        for (int j = 0; j < t.length(); j++) {
            if (count1[t.charAt(j) - 'a'] == 0) {
                return false;
            }
            count1[t.charAt(j) - 'a']--;
        }
        return true;
    }
}
```
### Summary
Build a int[] to count for the occurances of each letter in s and subtract it using t's letters. If none of the letter occurances reach below 0, then we know they are anagrams of each other. Remember s and t might be different in length.

Runtime: O(n)

Space: O(n)

## Leetcode 349
### Problem
Given two integer arrays nums1 and nums2, return an array of their 
intersection. Each element in the result must be unique and you may return the result in any order.

Example 1:
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
Example 2:
```
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

### Solution
```
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> first = new HashSet<>();
        List<Integer> res = new ArrayList<>();

        for (int i = 0; i < nums1.length; i++) {
            first.add(nums1[i]);
        }
        for (int i = 0; i < nums2.length; i++) {
            if (first.contains(nums2[i])) {
                first.remove(nums2[i]);
                res.add(nums2[i]);
            }
        }
        return res.stream().mapToInt(i -> i).toArray();
    }
}
```

### Summary
- Do not forget to remove the element in set 1 when we already find the element in nums2 since we don't want add a duplicate value to our result.
- convert an arraylist to array: **res.stream().mapToInt(i -> i).toArray();**

Runtime: O(m + n)

Space: O(m)

## Leetcode 202 Happy number
### Problem

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.

 

Example 1:
```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```
Example 2:
```
Input: n = 2
Output: false
```
### Solution
```
class Solution {
    public boolean isHappy(int n) {
        Set<Integer> result = new HashSet<>();
        int val = n;
        while (true) {
            int each = 0;
            while (val > 0) {
                int square = (int) Math.pow(val % 10, 2);
                each += square;
                val = val / 10;
            }
            if (each == 1) {
                return true;
            }
            if (!result.contains(each)) {
                result.add(each);
                val = each;
            } else {
                break;
            }
        }
        return false;
    }
}
```

### Summary
Continue calculating the sum of all digits until we have seen this sum before.

Runtime:


## Leetcode 1 Two Sum
### Problem
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

 

Example 1:
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```
Example 2:
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```
Example 3:
```
Input: nums = [3,3], target = 6
Output: [0,1]
```
### Solution
```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int a[]=new int[2];
       HashMap<Integer,Integer> s=new HashMap<Integer,Integer>();
        for(int i=0;i<nums.length;i++){
            if(s.containsKey(target-nums[i])){
                a[0]=s.get(target-nums[i]);
                a[1]=i;
                break;
            }
            s.put(nums[i],i);
        }
        return a;
    }
}
```

### summary
Initialize a map and for each value, if its complement (target - nums[i]) is not a key in map, we store the current value as a new key in the map until we find one

Runtime: O(n)
Space: O(n)
