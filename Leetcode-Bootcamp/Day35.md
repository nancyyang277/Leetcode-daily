# Day35
## Leetcode 435 Non-overlapping intervals
### Problem
Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.


Example 1:
```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```
Example 2:
```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

### Solution
```
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        int result = 0;
        Arrays.sort(intervals, (a,b) -> (a[0] - b[0]));
        int[] temp = intervals[0];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < temp[1]) {
                result++;
                temp[1] = Math.min(intervals[i][1], temp[1]);
            } else {
                temp = intervals[i];
            }
        }
        return result;
    }
}
```

### Summary
- Sort the array based on the start value ascendingly.
- If the current interval overlaps with the previous interval, we increment result since we need to remove one of the intervals
- And the interval we want to not remove is the one with smaller end value since there's a higher chance that the next interval won't overlap with the remained one
- If not overlap, update previous interval to current interval
- return result


## Leetcode 763 Partition Labels
### Problem
You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.

Return a list of integers representing the size of these parts.

Example 1:
```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```
Example 2:
```
Input: s = "eccbbbbdec"
Output: [10]
```

### Solution

### Summary


## Leetcode 456 Merge Intervals
### Problem
Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

 

Example 1:
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```
Example 2:
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

### Solution

### Summary







