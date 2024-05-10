# Day34
## Leetcode 860 Lemonade Change
### Problem
At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill. You must provide the correct change to each customer so that the net transaction is that the customer pays $5.

Note that you do not have any change in hand at first.

Given an integer array bills where bills[i] is the bill the ith customer pays, return true if you can provide every customer with the correct change, or false otherwise.

 

Example 1:
```
Input: bills = [5,5,5,10,20]
Output: true
Explanation: 
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```
Example 2:
```
Input: bills = [5,5,10,10,20]
Output: false
Explanation: 
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.
```
### Solution
```
class Solution {
    public boolean lemonadeChange(int[] bills) {
        Map<Integer, Integer> count = new HashMap<>();
        count.put(5, 0);
        count.put(10, 0);
        for (int i = 0; i < bills.length; i++) {
            int current = bills[i];
            if (current == 5) {
                count.put(5, count.get(5) + 1);
                continue;
            }
            if (current == 10) {
                if (count.get(5) >= 1) {
                    count.put(5, count.get(5) - 1);
                    count.put(10, count.get(10) + 1);
                } else {
                    return false;
                }
            } else if (current == 20) {
                if (count.get(5) >= 1 && count.get(10) >= 1) {
                    count.put(5, count.get(5) - 1);
                    count.put(10, count.get(10) - 1);
                } else if (count.get(5) >= 3) {
                    count.put(5, count.get(5) - 3);
                } else {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### Summary
- Just use a map to count the number of 5, 10 currency left and see if we can make the change

## Leetcode 406 Queue Reconstruction by Height
### Problem
You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).


Example 1:
```
Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```
Example 2:
```
Input: people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
Output: [[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

### Solution
```
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            @Override
            public int compare(int[] a, int[] b) {
                return a[0] == b[0] ? a[1] - b[1] : b[0] - a[0];
            }
        });
        List<int[]> output = new LinkedList<>();
        for (int[] each : people) {
            // arraylist.add(index, element);
            output.add(each[1], each);

        }
        int n = people.length;
        return output.toArray(new int[n][2]);
    }
}
```

### Summary
- Algorithm
  - Sort people:
    - In descending order by height.
    - Among the guys of the same height, in the ascending order by k-values.
  - Take guys one by one, and place them in the output array at the indexes equal to their k-values.
  - Return output array.
- Let's now consider a queue with people of two different heights: 7 and 6. For simplicity, let's have just one 6-height guy. First, follow the strategy above and arrange guys of height 7. Now it's time to find a place for the guy of height 6. Since he is "invisible" to the 7-height guys, he could take whatever place without disturbing the 7-height guys' order. However, for him the others are visible, and hence he should take the position equal to his k-value, in order to have his proper place.


## Leetcode 452 Minimum Number of Arrows to Burst Ballons
### Problem
There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array points where points[i] = [xstart, xend] denotes a balloon whose horizontal diameter stretches between xstart and xend. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up directly vertically (in the positive y-direction) from different points along the x-axis. A balloon with xstart and xend is burst by an arrow shot at x if xstart <= x <= xend. There is no limit to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array points, return the minimum number of arrows that must be shot to burst all balloons.

 

Example 1:
```
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 6, bursting the balloons [2,8] and [1,6].
- Shoot an arrow at x = 11, bursting the balloons [10,16] and [7,12].
```
Example 2:
```
Input: points = [[1,2],[3,4],[5,6],[7,8]]
Output: 4
Explanation: One arrow needs to be shot for each balloon for a total of 4 arrows.
```
Example 3:
```
Input: points = [[1,2],[2,3],[3,4],[4,5]]
Output: 2
Explanation: The balloons can be burst by 2 arrows:
- Shoot an arrow at x = 2, bursting the balloons [1,2] and [2,3].
- Shoot an arrow at x = 4, bursting the balloons [3,4] and [4,5].
```

### Solution
```
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, new Comparator<int[]>() {
            @Override
            public int compare (int[] o1, int[] o2) {
                return (o1[1] < o2[1]) ? -1 : ((o1[1] == o2[1]) ? 0 : 1);
            }
        } );
        int last = points[0][1];
        int count = 1;
        for (int i = 1; i < points.length; i++) {
            if (points[i][0] > last) {
                last = points[i][1];
                count++;
            }
        }
        return count;
    }
}
```

### Summary
- Sort the array based on the Xend ascendingly and if both have the same Xend, sort ascendingly based on Xstart
  - It is because if two ballons overlap, we want to set the arrow position at the smaller Xend since it is the farthest arrow position that could burst two ballons.
- If the next ballons overlap with the previous ballon but the last arrow could not hit it, we increment count and update new arrow position at the current's ballon's Xend
- return end




