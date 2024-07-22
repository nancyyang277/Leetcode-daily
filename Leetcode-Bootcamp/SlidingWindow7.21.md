# Leetcode 904 Fruit Into Baskets
## Problem
You are visiting a farm that has a single row of fruit trees arranged from left to right. The trees are represented by an integer array fruits where fruits[i] is the type of fruit the ith tree produces.

You want to collect as much fruit as possible. However, the owner has some strict rules that you must follow:

You only have two baskets, and each basket can only hold a single type of fruit. There is no limit on the amount of fruit each basket can hold.
Starting from any tree of your choice, you must pick exactly one fruit from every tree (including the start tree) while moving to the right. The picked fruits must fit in one of your baskets.
Once you reach a tree with fruit that cannot fit in your baskets, you must stop.
Given the integer array fruits, return the maximum number of fruits you can pick.

## Solution
```
class Solution {
    public int totalFruit(int[] fruits) {
        int max = 0;
        Map<Integer, Integer> map = new HashMap<>();
        int start = 0;
        for (int i = 0; i < fruits.length; i++) {
            int fruit = fruits[i];
            if (map.containsKey(fruit) || map.size() < 2) {
                map.put(fruit, i);
            } else {
                max = Math.max(max, i - start);
                int prev = fruits[i - 1];
                for (int key : map.keySet()) {
                    if (key != prev) {
                        int end = map.get(key) + 1;
                        map.remove(key);
                        start = end;
                        break;
                    }
                }
                map.put(fruit, i);
                
            }
        }
        max = Math.max(max, fruits.length - start);
        return max;
    }
}
```

## Summary
- Create a map (size of 2) that stores the last occurance of each fruit and a start index
- If we encounter a third fruit, then we need to update the start index of the sliding to be the last occurance + 1 of the fruit that is not the previous fruit of the third fruit
- compare the sliding window size and store it as maximum


# Leetcode 1838 Frequency of the Most Frequent Element
## Problem
The frequency of an element is the number of times it occurs in an array.

You are given an integer array nums and an integer k. In one operation, you can choose an index of nums and increment the element at that index by 1.

Return the maximum possible frequency of an element after performing at most k operations.

## Solution
```
class Solution {
    public int maxFrequency(int[] nums, int k) {
        Arrays.sort(nums);
        int left = 0, right = 0;
        long res = 0, total = 0;

        while (right < nums.length) {
            total += nums[right];

            while (nums[right] * (right - left + 1L) > total + k) {
                total -= nums[left];
                left += 1;
            }

            res = Math.max(res, right - left + 1L);
            right += 1;
        }

        return (int) res;        
    }
}
```

## Summary
- First sort the elements in the array
- start the sliding window and set left and right to 0
- result is comparing (right - left + 1) with the current max
- total is storing the total sum of all the elements in the sliding window and the key is
   - nums[right] * (right - left + 1L) > total + k where we update the left index if all the elements in the sliding is equal to the nums[right], and we currently have **total** value and we can maximum adding k values to total to help boost the total sum
   - if using k to boost the sum is not enough which is smaller than converting all the elements in the sliding window to nums[right], then we increment left index until we can use less than k operations. 
