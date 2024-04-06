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
