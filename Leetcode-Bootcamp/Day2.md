# Day 2 4/4/2024
## Leetcode 977 Squares of a Sorted Array (Easy)
### Problem
Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.

#### constraints:
nums is sorted in non-decreasing order.

### Solution:
```
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] result = new int[nums.length];
        int mid = nums.length / 2;
        int lowest = -1;
        // find the smallest number's index
        for (int i = 0; i < nums.length - 1; i++) {
            if (Math.abs(nums[i]) < Math.abs(nums[i+1])){
                lowest = i;
                break;
            }
        }
        if (lowest == - 1) {
            lowest = nums.length - 1;
        }
        result[0] = nums[lowest] * nums[lowest];
        int left = lowest - 1;
        int right = lowest + 1;
        int index = 0;
        while (!(left < 0 && right >= nums.length) && index < nums.length - 1) {
            index++;
            if (left < 0) {
                result[index] = nums[right] * nums[right];
                right++;
                continue;
            } 
            if (right >= nums.length) {
                result[index] = nums[left] * nums[left];
                left--;
                continue;
            }
            if (Math.abs(nums[left]) <= Math.abs(nums[right])) {
                result[index] = nums[left] * nums[left];
                left--;
            } else {
                result[index] = nums[right] * nums[right];
                right++;
            }
            
        }
        return result;
    }
}
```
### Summary
找到最小的数值然后存他的index，initialize left and right where we update the next number by comparing left index and right index. 

If Math.abs(nums[left]) < Math.abs(nums[right]), add left, update left by minus one.

else, add right, update right by plus one.

If left or right reach the boundary of nums, only update the other index until both left and right exit the boundary.

Runtime: O(n)

Space: O(n)


## Leetcode 209 Minimum Size Subarray Sum
### Problem
Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

### Solution
```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int result = Integer.MAX_VALUE;
        int slow = 0;
        int fast = 0;
        int temp= 0;
        while (fast <= nums.length) {
            if (temp >= target) {
                result = Math.min(result, fast - slow);
                if (slow == fast) {
                    return result;
                }
                temp -= nums[slow];
                slow++;
            } else {
                if (fast < nums.length) {
                    temp += nums[fast];
                }
                fast++;
            }
            
        }
        if (result == Integer.MAX_VALUE) {
            return 0;
        }
        return result;
    }
}
```
### Summary

窗口的起始位置如何移动：如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）。

窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。

Runtime: O(n)

主要是看每一个元素被操作的次数，每个元素在滑动窗后进来操作一次，出去操作一次，每个元素都是被操作两次，所以时间复杂度是 2 × n 也就是O(n)。

Space: O(n)

## Leetcode 59
### Problem
Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.
![47C96236-A980-47BC-8003-0FB9DB5260CC_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/73d61e36-bf57-4778-9b59-56b65042a338)

### Solution
```
class Solution {
    public int[][] generateMatrix(int n) {
        int elementNum = n - 1;
        int[][] result = new int[n][n];
        int row = 0;
        int column = 0;
        int num = 1;
        while (elementNum >= 1) {
            for (int i = 0; i < elementNum; i++) {
                result[row][column] = num;
                column++;
                num++;
            }
            for (int i = 0; i < elementNum; i++) {
                result[row][column] = num;
                row++;
                num++;
                
            }
            for (int i = 0; i < elementNum; i++) {                
                result[row][column] = num;
                column--;
                num++;
            }
            for (int i = 0; i < elementNum; i++) {
                result[row][column] = num;
                // 如果i为这个loop最后一个数值，此时不需要update row，因为这样row的数值会回到起点
                if (i != elementNum - 1) {
                    row--;
                }
                num++; 
            }
            // update column to new start
            column++;
            elementNum -= 2;
        }
        if (n % 2 == 1) {
            result[n/2][n/2] = num;
        }
        return result;
    }
}
```

### Summary
我们把这个n✖️n的正方体每一层分为一个整体，每一层再分成四份，第一个长方体的时候column是逐渐增加的，第二个长方体row逐渐增加，第三个column逐渐减少，第四个row逐渐减少。
第一层长方体的长度为n-1，第二层长方体为n-3... 直到最后一层为0，这个时候我们停止外面的while loop
如果n为单数，中间还需要填补一个最后的数值(n✖️n)

runtime: O(n^2)

Space: O(n^2)








