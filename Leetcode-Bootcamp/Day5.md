# Day5
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
