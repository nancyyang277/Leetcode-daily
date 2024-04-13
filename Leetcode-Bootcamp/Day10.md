# Day10
## 232. Implement Queue using Stacks
### Problem
Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (push, peek, pop, and empty).

Implement the MyQueue class:

void push(int x) Pushes element x to the back of the queue.
int pop() Removes the element from the front of the queue and returns it.
int peek() Returns the element at the front of the queue.
boolean empty() Returns true if the queue is empty, false otherwise.
Notes:

You must use only standard operations of a stack, which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

### Solution
```
class MyQueue {
    Stack<Integer> s1;
    Stack<Integer> s2;
    int size;
    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
        size = 0;
    }
    
    public void push(int x) {
        s1.push(x);
        size++;
    }
    
    public int pop() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }
        size--;
        return s2.pop();
    }
    
    public int peek() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }
        int temp = s2.pop();
        s2.push(temp);
        return temp;
    }
    
    public boolean empty() {
        return size == 0;
    }
}
```

### Summary
Using two stacks that one of them is trying to record every element and the other one if used when popping 
a element, if the second stack is not empty, pop the first lement (O(1)). If it is empty, we loop through
all the elements in the first stack and record them in the second stack so that when pop is called, the top (last)
element in the second stack is our solution

**Amortized Analysis**
Amortized analysis gives the average performance (over time) of each operation in the worst case. The basic idea is that a worst case operation can alter the state in such a way that the worst case cannot occur again for a long time, thus amortizing its cost.

Pop and peek in here is Amoritized O(1) runtime

### Leetcode 225 Implement Stack using Queues
### Problem
Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

void push(int x) Pushes element x to the top of the stack.
int pop() Removes the element on the top of the stack and returns it.
int top() Returns the element on the top of the stack.
boolean empty() Returns true if the stack is empty, false otherwise.
Notes:

You must use only standard operations of a queue, which means that only push to back, peek/pop from front, size and is empty operations are valid.
Depending on your language, the queue may not be supported natively. You may simulate a queue using a list or deque (double-ended queue) as long as you use only a queue's standard operations.

### Solution
```
class MyStack {
    int top;
    int size;
    Queue<Integer> queue;
    public MyStack() {
        top = Integer.MAX_VALUE;
        size = 0;
        queue = new LinkedList<>();
    }
    
    public void push(int x) {
        if (top != Integer.MAX_VALUE && size != 0) {
            queue.add(top);
        }
        top = x;
        size++;
    }
    
    public int pop() {
        if (top != Integer.MAX_VALUE) {
            int temp = top;
            top = Integer.MAX_VALUE;
            size--;
            return temp;
        }
        int count = size;
        int val;
        for (int i = 0; i < count - 2; i++) {
            val = queue.remove();
            queue.add(val);
        }
        if (size >= 2) {
            int newTop = queue.remove();
            top = newTop;
        }
        size--;
        return queue.remove();
        
    }
    
    public int top() {
        if (top != Integer.MAX_VALUE) {
            return top;
        }
        int count = size;
        for (int i = 0; i < count - 1; i++) {
            queue.add(queue.remove());
        }
        int val = queue.remove();
        queue.add(val);
        top = val;
        return top;
    }
    
    public boolean empty() {
        return size == 0;
    }
}
```

### Summary
Using a top value to store the last pushed value, if set to MAX_VALUE, we need to go through the whole
stack again to get the top value. 

When pop is called, if top is not MAX_VALUE, we directly return it. Else, we go through the entire queue to find
the value. If the queue size is equal to or greater than 2, we stop at the last two elements and set the new top
and return the old top.



