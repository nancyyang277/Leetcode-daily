# Day13
## Leetcode 239 Sliding Window Maximum (Hard)
### Problem
You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

 

Example 1:
```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
Example 2:
```
Input: nums = [1], k = 1
Output: [1]
```

### Solution
```
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        Deque<Integer> deque = new ArrayDeque<>();
        List<Integer> res = new ArrayList<>();
        int i = 0;
        for (i = 0; i < k; i++) {
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
                deque.pollLast();
            }
            deque.offer(i);
        }
        res.add(nums[deque.peekFirst()]);
        for (int right = k; right < nums.length; right++) {
            if (deque.peekFirst() == right - k) {
                deque.pollFirst();
            }
            while (!deque.isEmpty() && nums[deque.peekLast()] < nums[right]) {
                deque.pollLast();
            }
            deque.offerLast(right);
            res.add(nums[deque.peekFirst()]);
        }
        return res.stream().mapToInt(j->j).toArray();
    }
}
```

### Summary
Learn how to use Deque

The main Logic is that in the deque we store the index of the largest value for a window of k, if the pollFirst()
index is smaller than current - k (which is not in the current sliding window), then we poll the value.

We loop through the array and if the current value is greater than the pollLast() of the deque, we know that the 
value on the deque (pollLast()) is not important to our result since we have encountered something larger. In this case,
we are creating a decreasing deque. If the current value is smaller than the pollLast() of the deque, we add to it.

For each iteration, since we are encountering a new number, the sliding window is shifting to right by 1, we add the leftmost (pollLeft())
result of deque to the res and convert it at the end. 

Runtime: O(n)

Space: O(n)

## Leetcode 347 Top K frequent Elements
### Problem
Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

 

Example 1:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
Example 2:
```
Input: nums = [1], k = 1
Output: [1]
```

### Solution
```
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Arrays.sort(nums);
        int prev = nums[0];
        int count = 1;
        Map<Integer, Integer> map = new HashMap<>();
        PriorityQueue<Integer> result = new PriorityQueue<Integer>(new Comparator<>() {
            @Override
            public int compare(Integer a, Integer b) {
                return map.get(b) - map.get(a);
            }
        });
        for (int i = 1; i < nums.length; i++) {
            int current = nums[i];
            if (current != prev) {
                map.put(prev, count);
                count = 1;
                result.add(prev);
            } else {
                count++;
            }
            prev = current;
        }
        map.put(prev, count);
        result.add(prev);
        int[] ans = new int[k];
        for (int i = 0; i < k; i++) {
            ans[i] = result.poll().intValue();
        }
        return ans;
    }
}
```

### Summary
Sort the array

Create a priority queue to count the occurances of each number. since the array is sorted, if the current value
is not equal to the previous value, we know that we have already collected all the occurance of previous value
and it can be inserted into the priority queue. Otherwise, we store its occurance in a map. 

At last, we poll the top k elements from the queue and return the result. 

runtime: O(n)

Space: O(n)


