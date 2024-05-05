# Day29
## Leetcode 491 Non-decreasing Subsequences
### Problem
Given an integer array nums, return all the different possible non-decreasing subsequences of the given array with at least two elements. You may return the answer in any order.

Example 1:
```
Input: nums = [4,6,7,7]
Output: [[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```
Example 2:
```
Input: nums = [4,4,3,2,1]
Output: [[4,4]]
```

### Solution
```
class Solution {
    Set<List<Integer>> res = new HashSet<>();
    public List<List<Integer>> findSubsequences(int[] nums) {
        helper(nums, 0, new ArrayList<>());
        return new ArrayList<>(res);
    }

    private void helper(int[] nums, int startIndex, List<Integer> list) {
        if (startIndex == nums.length) {
            if (list.size() >= 2) {
                res.add(new ArrayList<>(list));
            }
            return;
        }
        if (list.size() == 0 || nums[startIndex] >= list.get(list.size() - 1)) {
            list.add(nums[startIndex]);
            helper(nums, startIndex + 1, list);
            list.remove(list.size() - 1);
        }
        helper(nums, startIndex + 1, list);
    }
}
```

### Summary
- Note that there might be duplicates among the found subsequences, and we do not want to include the same subsequence more than once. We can achieve this by maintaining them in a set.
- We should always recursively call helper(startIndex + 1) without appending the element (with an empty list and startIndex + 1 being the first element)

## Leetcode 46 Permutations
### Problem
Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

 

Example 1:
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
Example 2:
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```
Example 3:
```
Input: nums = [1]
Output: [[1]]
```

### Solution
```
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        if (nums.length == 0) {
            return res;
        }
        helper(nums, 0, new ArrayList<>());
        return res;
    }

    private void helper(int[] nums, int count, List<Integer> list) {
        if (count == nums.length) {
            res.add(new ArrayList<>(list));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (list.contains(nums[i])) {
                continue;
            }
            count++;
            list.add(nums[i]);
            helper(nums, count, list);
            count--;
            list.remove(list.size() - 1);
        }
    }
}
```
### Summary
- Simply for each recursively call, start from index 0 and add to the list if the current element is not on the list yet, then remove the element for backtracking
- Time complexity: O(n⋅n!)
- Space complexity: O(n)

## Leetcode 47 Permutation II
### Problem
Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

Example 1:
```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```
Example 2:
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

### Solution
```
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permuteUnique(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (!count.containsKey(nums[i])) {
                count.put(nums[i], 0);
            }
            count.put(nums[i], count.get(nums[i]) + 1);
        }
        helper(count, nums.length, new ArrayList<>());
        return res;

    }

    private void helper(Map<Integer, Integer> counter, int nums, List<Integer> list) {
        if (list.size() == nums) {
            res.add(new ArrayList<>(list));
            return;
        }
        for (Map.Entry<Integer, Integer> entry : counter.entrySet()) {
            Integer num = entry.getKey();
            Integer count = entry.getValue();
            if (count == 0) {
                continue;
            }
            list.add(num);
            counter.put(num, count - 1);
            helper(counter, nums, list);
            list.remove(list.size() - 1);
            counter.put(num, count);
        }
    }   
}
```

### Summary
- We don't want to have two combination with the same number on the same index (which will cause the repetition not generating unique values), we use a map to only have unique numbers as key (choose from)
