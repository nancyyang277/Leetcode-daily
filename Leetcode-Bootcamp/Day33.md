# Day33
## Leetcode 134 Gas Station
### Problem
There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

 

Example 1:
```
Input: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```
Example 2:
```
Input: gas = [2,3,4], cost = [3,4,3]
Output: -1
Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```
### Solution
```
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int currGain = 0, totalGain = 0, answer = 0;

        for (int i = 0; i < gas.length; ++i) {
            // gain[i] = gas[i] - cost[i]
            totalGain += gas[i] - cost[i];
            currGain += gas[i] - cost[i];

            // If we meet a "valley", start over from the next station
            // with 0 initial gas.
            if (currGain < 0) {
                answer = i + 1;
                currGain = 0;
            }
        }
        // if total difference is smaller than 0, which indicates that the gas gained after traversing segment 1 isn't enough to support us passing through the remaining segments, and there is no valid station.
        return totalGain >= 0 ? answer : -1;
    }
}
}
```

### Summary
- If the total difference(gas-cost for each station) is greater than or equal to 0, it means we are guaranteeing a result
- otherwise return -1
- We then go through the whole array to check whether the current gas station is a good place to start
- Traverse through gas and cost. For each index i, increment total_gain and curr_gain by gas[i] - cost[i].
- If curr_gain is smaller than 0, we will test if station i + 1 is a valid starting station by setting answer as i + 1, resetting curr_gain to 0, and repeating step 2.
- When the iteration is complete, if total_gain is smaller than 0, return -1. Otherwise, return answer.
- runtime: O(n)
- space: O(1)

## Leetcode 135 Candy
### Problem
There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.

You are giving candies to these children subjected to the following requirements:

Each child must have at least one candy.
Children with a higher rating get more candies than their neighbors.
Return the minimum number of candies you need to have to distribute the candies to the children.

 

Example 1:
```
Input: ratings = [1,0,2]
Output: 5
Explanation: You can allocate to the first, second and third child with 2, 1, 2 candies respectively.
```
Example 2:
```
Input: ratings = [1,2,2]
Output: 4
Explanation: You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.
```

### Solution
```
public class Solution {
    public int candy(int[] ratings) {
        int[] candies = new int[ratings.length];
        Arrays.fill(candies, 1);
        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }
        int sum = candies[ratings.length - 1];
        for (int i = ratings.length - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                if (candies[i] <= candies[i+1]) {
                    candies[i] = candies[i+1] + 1;
                }
            }
            sum += candies[i];
        }
        return sum;
    }
}
```


### Summary
- initialize our result array with all values set to 1
- We first traverse from left to right whenever the current element's ratings is larger than the left neighbor and has less than or equal candies than its left neighbor, we update the current element's candies in the candies array as candies[i] = candies[i-1] + 1
- While updating we need not compare candies[i] and candies[i - 1], since candies[i] <= candies[i - 1] before updating.
- After this, we traverse the array from right-to-left. Now, we need to update the i'th element's candies in order to satisfy both the left neighbor and the right neighbor relation. Now, during the backward traversal, if ratings[i] > ratings[i + 1], considering only the right neighbor criteria, we could've updated candies[i] as candies[i] = candies[i + 1] + 1. But, this time we need to update candies[i] only if candies[i] <= candies[i + 1].
- This makes candies[i] satisfy both the left neighbor and the right neighbor criteria.
- runtime: O(n)
- Space: O(n)

## Leetcode 1005 Maximize Sum of Array after K negations
### Problem
Given an integer array nums and an integer k, modify the array in the following way:

choose an index i and replace nums[i] with -nums[i].
You should apply this process exactly k times. You may choose the same index i multiple times.

Return the largest possible sum of the array after modifying it in this way.

 

Example 1:
```
Input: nums = [4,2,3], k = 1
Output: 5
Explanation: Choose index 1 and nums becomes [4,-2,3].
```
Example 2:
```
Input: nums = [3,-1,0,2], k = 3
Output: 6
Explanation: Choose indices (1, 2, 2) and nums becomes [3,1,0,2].
```
Example 3:
```
Input: nums = [2,-3,-1,5,-4], k = 2
Output: 13
Explanation: Choose indices (1, 4) and nums becomes [2,3,-1,5,4].
```

### Solution
```
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        int sum = 0;
        int negSum = 0;
        int count = k;
        int largest = 0;
        boolean found = false;
        int countNeg = 0;
        Arrays.sort(nums);
        int smallest = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < 0 && count > 0) {
                nums[i] = -1 * nums[i];
                count--;
            }
            sum += nums[i];
            smallest = Math.min(smallest, nums[i]);
        }
        if (count % 2 == 0) {
            return sum;
        } 
        return sum -= 2 * smallest;

    }
}
```

### Summary
- we sort the array from smallest to largest and we negate all the negative values to make them positive if the number of negatives is smaller or equal to k
- If there are left some chances to negate (k > number of negatives), we check if count is divisible by 2, if it is, then we can choose any index i and negate it count times which would still be the original value
- Otherwise find the smallest absolution value in the array and let it become negative (deduct twice the value from sum)
- runtime: O(n)
- space: O(1)





