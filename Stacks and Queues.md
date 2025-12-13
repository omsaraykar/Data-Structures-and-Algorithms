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

```
---
### Q3. Implement Stack using Linkedlist
```java

```
---
### Q4. Implement Queue using Linkedlist
```java

```
---
### Q5. Implement Stack using Queue
```java

```
---
### Q6. Implement Queue using Stack
```java

```
---