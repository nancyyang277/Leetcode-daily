# Day 3 (04/05/24)
## Leetcode 203
### Problem
Given the head of a linked list and an integer val, remove all the nodes of the linked list that has Node.val == val, and return the new head.
![4D4FFC75-4490-45C9-A335-4775AEB15E62_4_5005_c](https://github.com/nancyyang277/Leetcode-daily/assets/165972977/9ce0093d-e8bf-4099-b9b9-851e99fce885)

### Solution
```
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode curr = dummy;
        while (curr.next != null) {
            if (curr.next.val != val) {
                curr = curr.next;
            } else {
                curr.next = curr.next.next;
            }
        }
        return dummy.next;
    }
}
```

### Summary
- 为什么要define一个curr来遍历？
  
  因为用head遍历的话，头节点所指的数值一直在发生变化，最后无法返回原来linkedlist的头节点

- 用dummy head来指向head，curr等于dummy，如果curr.next为val，那么我们set curr.next = curr.next.next；如果不是val，那么我们遍历到下一个curr(curr = curr.next)

Runtime: O(n)

Space: O(1)

## Leetcode 707

### Problem
Design your implementation of the linked list. You can choose to use a singly or doubly linked list.
A node in a singly linked list should have two attributes: val and next. val is the value of the current node, and next is a pointer/reference to the next node.
If you want to use the doubly linked list, you will need one more attribute prev to indicate the previous node in the linked list. Assume all nodes in the linked list are 0-indexed.

Implement the MyLinkedList class:

MyLinkedList() Initializes the MyLinkedList object.
int get(int index) Get the value of the indexth node in the linked list. If the index is invalid, return -1.
void addAtHead(int val) Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list.
void addAtTail(int val) Append a node of value val as the last element of the linked list.
void addAtIndex(int index, int val) Add a node of value val before the indexth node in the linked list. If index equals the length of the linked list, the node will be appended to the end of the linked list. If index is greater than the length, the node will not be inserted.
void deleteAtIndex(int index) Delete the indexth node in the linked list, if the index is valid.
 

Example 1:

Input
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
Output
[null, null, null, null, 2, null, 3]

Explanation
```
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // linked list becomes 1->2->3
myLinkedList.get(1);              // return 2
myLinkedList.deleteAtIndex(1);    // now the linked list is 1->3
myLinkedList.get(1);              // return 3
```

### Solution
```
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) { val = x; }
}

class MyLinkedList {
    ListNode head;
    int size;
    public MyLinkedList() {
        head = new ListNode(0);
        size = 0;
    }
    
    public int get(int index) {
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode curr = head;
        for (int i = 0; i <= index; i++) {
            curr = curr.next;
        }
        return curr.val;
    }
    
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
        if (index > size) {
            return;
        }
        if (index < 0 ){
            index = 0;
        }
        ListNode curr = head;
        for (int i = 0; i < index; i++) {
            curr = curr.next;
        }
        ListNode temp = curr.next;
        ListNode insertNode = new ListNode(val);
        curr.next = insertNode;
        insertNode.next = temp;
        size++;
    }
    
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        ListNode curr = head;
        for (int i = 0; i < index; i++) {
            curr = curr.next;
        }
        // ListNode temp = curr.next;
        curr.next = curr.next.next;
        size--;
    }
}
```

### Summary
记住定义constructor head = new ListNode(0) and size

Runtime: O(1) for addAtHead. O(k) for get, addAtIndex, and deleteAtIndex, where k is an index of the element to get, add or delete. O(N) for addAtTail.

Space: O(1)

## Leetcode 206
### Problem
Given the head of a singly linked list, reverse the list, and return the reversed list.

### Solution

### Summary


