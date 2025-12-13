### Q1. Implement Stack Using Array
```java
class myStack {
    int[] st;
    int idx = -1;

    public myStack(int n) {
        st = new int[n];
    }

    public boolean isEmpty() {
        return idx == -1;
    }

    public boolean isFull() {
        return idx == st.length - 1;
    }

    public void push(int x) {
        if (isFull()) {
            System.out.println("The stack is full, can't push");
            return;
        }
        st[++idx] = x;
    }

    public void pop() {
        if (isEmpty()) {
            System.out.println("The stack is empty, can't pop.");
            return;
        }
        st[idx--] = 0;
    }

    public int peek() {
        if (isEmpty()) {
            System.out.println("The stack is empty, can't peek.");
            return -1;
        }
        return st[idx];
    }
}
```
---
### Q2. Implement Queue Using Array
```java
class myQueue {
    int[] q;
    int start;
    int end;
    int size;
    
    public myQueue(int n) {
        q = new int[n];
        start = -1;
        end = -1;
        size = 0;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == q.length;
    }

    public void enqueue(int x) {
        if (isFull()) {
            return;
        } else if (isEmpty()) {
            start = 0;
            end = 0;
            q[end] = x;
            size++;
            return;
        }
        q[++end] = x;
        size++;
    }

    public void dequeue() {
        if (isEmpty()) {
            return;
        } else if (size == 1) {
            q[start] = 0;
            start = -1;
            end = -1;
            size = 0;
            return;
        }
        q[start++] = 0;
        size--;
    }

    public int getFront() {
        if(isEmpty()) {
            return -1;
        }
        return q[start];
    }

    public int getRear() {
        if (isEmpty()) {
            return -1;
        }
        return q[end];
    }
}
```
---
### Q3. Implement Stack using Linked list
```java
class Node {
    int data;
    Node next;

    Node(int new_data) {
        data = new_data;
        next = null;
    }
}

class myStack {
    Node top;
    int size;

    public myStack() {
        top = null;
        size = 0;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void push(int x) {
        Node temp = new Node(x);
        temp.next = top;
        top = temp;
        size++;
    }

    public void pop() {
        if(isEmpty()) return;
        top = top.next;
        size--;
    }

    public int peek() {
        if (isEmpty()) return -1;
        return tail.data;
    }

    public int size() {
        return size;
    }
}
```
---
### Q4. Implement Queue using Linked list
```java
class Node {
    int data;
    Node next;

    Node(int new_data) {
        data = new_data;
        next = null;
    }
}

class myQueue {
    Node head;
    Node tail;
    int size;

    public myQueue() {
        head = null;
        tail = null;
        size = 0;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void enqueue(int x) {
        if (isEmpty()) {
            Node temp = new Node(x);
            head = temp;
            tail = temp;
            size = 1;
            return;
        }
        Node temp = new Node(x);
        tail.next = temp;
        tail = tail.next;
        size++;
    }

    public void dequeue() {
        if (isEmpty()) return;
        head = head.next;
        size--;
    }

    public int getFront() {
        if (isEmpty()) return -1;
        return head.data;
    }

    public int size() {
        return this.size;
    }
}
```
---
### Q5. Implement Stack using Queue
```java
class MyStack {
    private Queue<Integer> q;

    public MyStack () {
        q = new LinkedList<>();
    }

    public void push(int x) {
        q.add(x);
        for (int i = 0; i < q.size() - 1; i++) {
            q.add(q.remove());
        }
    }

    public int pop() {
        return q.remove();
    }

    public int top() {
        return q.peek();
    }

    public boolean empty() {
        return q.isEmpty();
    }
}
```
---
### Q6. Implement Queue using Stack
```java
class MyQueue {
    Stack<Integer> st1;
    Stack<Integer> st2;
    int size;

    public MyQueue() {
        st1 = new Stack<>();
        st2 = new Stack<>();
        size = 0;
    }
    
    public void push(int x) {
        if (empty()) {
            st1.push(x);
            size++;
            return;
        }

        while (!st2.isEmpty()) {
            st1.push(st2.pop());
        }
        st1.push(x);
        size++;
    }
    
    public int pop() {
        if (empty()) {
            return -1;
        }

        while (!st1.isEmpty()) {
            st2.push(st1.pop());
        }
        size--;
        return st2.pop();
    }
    
    public int peek() {
        if (empty()) {
            return -1;
        }

        while (!st1.isEmpty()) {
            st2.push(st1.pop());
        }
        return st2.peek();
    }
    
    public boolean empty() {
        return size == 0;
    }
}
```

> [!NOTE] Note:
> You can also do this with one stack using Recursion.

---