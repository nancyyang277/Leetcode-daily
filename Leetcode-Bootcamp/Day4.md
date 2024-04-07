# Day4
## Leetcode 24 Swap Nodes in Pairs (medium)
### Problem
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

![72D8DA65-F0CE-4695-BAF4-C9A8481B434D_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/794cb5d3-608c-4a0f-b337-a277dc40e7c9)

### Solution
```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode prev = dummy;

        while (head != null && head.next != null) {
            ListNode firstNode = head;
            ListNode secondNode = head.next;

            prev.next = secondNode;
            firstNode.next = secondNode.next;
            secondNode.next = firstNode;

            prev = prev.next.next;
            head = prev.next;
        }
        return dummy.next;
    }
}
```

### Summary
操作指针一定要指向我要反转两个节点的前一个节点 (that's why we are introducing a new dummy head)
while的条件：head != null && head.next != null (后面还有至少两个数值可以swap，要不然只有一个的话是不需要swap)
每次swap的顺序：
1. initialize prev, first, second
2. prev.next = second;
3. first.next = second.next;
4. second.next = first;
   
然后更新
1. prev = prev.next.next;
2. head = prev.next;

Runtime: O(n)

Space: O(1)

## Leetcode 19 Remove Nth Node from the end of list (medium)
### Problem
Given the head of a linked list, remove the nth node from the end of the list and return its head.
![693D2519-491D-49B6-9B73-8B065DFCC5D6_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/7a24febf-8a6f-4e8f-bb9a-f8cfcbd3fa0c)

### Solution
- Two passes
  ```
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int count = 0;
        ListNode curr = head;
        while (curr != null) {
            count++;
            curr = curr.next;
        }
        int index = count - n;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        int i = 0;
        ListNode prev = dummy;
        while (prev != null) {
            if (i == index) {
                if (prev.next.next == null) {
                    prev.next = null;
                } else {
                    prev.next = prev.next.next;
                }
                break; 
            }
            i++;
            prev = prev.next;
        }
        return dummy.next;
      }
  }
  ```
- One pass
  ```
  class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;
        int i = 0;
        while (i < n + 1) {
            fast = fast.next;
            i++;
        }
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
      }
  }
  ```
  
### Summary

- Two Pointers
  - 定义fast指针和slow指针，初始值为虚拟头结点
    ![FB6AB09C-1D4A-4F85-9C5E-3583F0B6550B_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/150c3bc5-cbe5-4b2e-b327-a33bb7aad825)
    
  - fast首先走n + 1步 ，为什么是n+1呢，因为只有这样同时移动的时候slow才能指向删除节点的上一个节点（方便做删除操作）
    ![63162A7C-E8CF-4217-881C-D760565E2F27_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/0e95d90e-f91b-4deb-a221-83bdbd1b56df)

  - fast和slow同时移动，直到fast指向末尾
    
    ![5730C2EF-3902-4633-9CD9-496F0F094FE4_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/4f04dddb-6010-434a-9951-b6786bd096b8)
    
  - 删除slow指向的下一个节点
    
Runtime: O(n)


Space: O(1)

## Leetcode 160 Intersection of Two LinkedList (Easy)
### Problem
Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:
![AC5A5F04-9079-4D92-B976-ADD8DE403263_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/9f788416-0075-4827-96bd-a60bb6a52043)

### Solution
```
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode pA = headA;
        ListNode pB = headB;
        while (pA != pB) {
            pA = pA == null ? headB : pA.next;
            pB = pB == null ? headA : pB.next;
        }
        return pA;
    }
}
```
### Summary
![20C31371-82A9-4D5B-B8E6-0C68B7853572_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/d6807954-975a-412f-bb33-c04068d47235)

In the above diagram, we would have one pointer points at the beginning of headA (pA), and another at headB (pB). let pA go through a + c first, and after goes through c, it will reach a null value, then we determine if pA == null, then we point it at headB. We do the same thing for pB, to let it go through b + c, and after goes through c, it will reach a null value, then we determine if pB == null, then we point it at headA. Since a + c + b (+c) = b + c + a (+ c), and if both loop through a + b + c, then both will reach the beginning of intersection if they intersect (at c). If they don't intersect, pA and pB will both equal to null at the end and return null. 

Runtime: O(n + m)

Space: O(1)

## Leetcode 142 Linked List Cycle II (Medium)
### Problem
Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return null.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to (0-indexed). It is -1 if there is no cycle. Note that pos is not passed as a parameter.

Do not modify the linked list.

![9B7F352D-23FA-4B02-9E20-DFB84869585A_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/b2da10c4-58d6-480a-b98f-92da6d81db35)

### Solution
```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                break;
            }
        }
        if (fast == null || fast.next == null) {
            return null;
        }
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```
### Summary
- 怎么判断会有intersection？
  Consider two pointers at different speed - a slow pointer and a fast pointer. The slow pointer moves one step at a time while the fast    pointer moves two steps at a time.

If there is no cycle in the list, the fast pointer will eventually reach the end and we can return false in this case.

Now consider a cyclic list and imagine the slow and fast pointers are two runners racing around a circle track. The fast runner will eventually meet the slow runner. Why? Consider this case (we name it case A) - The fast runner is just one step behind the slow runner. In the next iteration, they both increment one and two steps respectively and meet each other.

How about other cases? For example, we have not considered cases where the fast runner is two or three steps behind the slow runner yet. This is simple, because in the next or next's next iteration, this case will be reduced to case A mentioned above.

- 为什么算法能work
  
![000458B3-42BA-429B-A786-DAC526ABE33D_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/40dffa0f-a173-475f-ab90-b847609f52f1)

(x + y) * 2 = x + y + n (y + z)

两边消掉一个（x+y）: x + y = n (y + z)

因为要找环形的入口，那么要求的是x，因为x表示 头结点到 环形入口节点的的距离。

所以要求x ，将x单独放在左面：x = n (y + z) - y ,

再从n(y+z)中提出一个 （y+z）来，整理公式之后为如下公式：x = (n - 1) (y + z) + z 注意这里n一定是大于等于1的，因为 fast指针至少要多走一圈才能相遇slow指针。

这个公式说明什么呢？

先拿n为1的情况来举例，意味着fast指针在环形里转了一圈之后，就遇到了 slow指针了。

当 n为1的时候，公式就化解为 x = z，

这就意味着，从头结点出发一个指针，从相遇节点 也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点。

This tells us that the number of times the fast laps the cycle times the length of the cycle equals the distance from the head of the list to the meeting point.

https://leetcode.com/problems/linked-list-cycle-ii/editorial/

