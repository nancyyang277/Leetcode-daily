# Day 1 4/3/2024
## Leetcode 704 Binary Search (Easy)

### Problem:

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with **O(log n)** runtime complexity.


Some constraints to notice before writing the code:
All the integers in nums are unique.
Because if we have duplicated elements, then the answer is not guareented to be unique

nums is sorted in ascending order. 
We can use binary search since it is sorted.

### Solution:

```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            if (nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

Summary
二分法区间的两种写法
1. [left, right] where left and right are both available candidates.
- while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
- if (nums[mid] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 mid - 1
  ```
  if (nums[mid] > target)
      right = mid - 1
  ```
  
2. [left, right) where right is not an candidate
- while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
- if (nums[mid] > target) right 更新为 middle，因为当前nums[mid]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为mid，即：下一个查询区间不会去比较nums[mid]
    ```
    if (nums[mid] > target)
      right = mid
    ```

Runtime: O(logn)

Space: O(1)

## Leetcode 27 Remove Element (Easy)

### Problem

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

### Solution:

```
class Solution {
    public int removeElement(int[] nums, int val) {
        // 快慢指针
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.length; fastIndex++) {
            if (nums[fastIndex] != val) {
                nums[slowIndex] = nums[fastIndex];
                slowIndex++;
            }
        }
        return slowIndex;
    }
}
```

### Summary
元素的内存地址是连续的，所以不能删除某个元素，只能覆盖
不需要考虑数组中超出新长度后面的元素
双指针法
1. 一开始最先想到的是双向指针法
```
//相向双指针法
class Solution {
    public int removeElement(int[] nums, int val) {
        int left = 0;
        int right = nums.length - 1;
        while(right >= 0 && nums[right] == val) right--; //将right移到从右数第一个值不为val的位置
        while(left <= right) {
            if(nums[left] == val) { //left位置的元素需要移除
                //将right位置的元素移到left（覆盖），right位置移除
                nums[left] = nums[right];
                right--;
            }
            left++;
            while(right >= 0 && nums[right] == val) right--;
        }
        return left;
    }
}
```
3. 快慢指针
快指针(fast)：新数组所需要的元素（判断只要不是val，就可以赋给新数组）
慢指针(slow)：新数组的index，需要更新的index，any index smaller than slow is not val
快指针一直在寻找新数组所需要的元素，如果找到的数组是val的话，which means不是我们新数组所需要的元素，那么我们就不需要赋给新数组，不用更新新数组元素，所以我们的慢指针也不需要更新。

Runtime: O(n)

Space: O(1)

