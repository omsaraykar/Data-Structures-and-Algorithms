## Questions
### Q1] Middle of the Linked List
**`TC: O(n), SC: O(1)`**
```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
    }
}
```
### Q2] Reverse a Linked List
**M1: Iterative (3 pointers)** **`TC: O(n), SC: O(1)`**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode curr = head;
        ListNode prev = null;
        ListNode next = null;

        while (curr != null) {
            next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }
}
```
**M2: Recursive** **`TC: O(n), SC: O(n)`**
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode node = reverseList(head.next);
        head.next.next = head; // 🤓
        head.next = null;

        return node;
    }
}
```
### Q3] Detect a cycle in Linked List
**Using Floyd's Cycle-Finding Algorithm** **`TC: O(n), SC: O(1)`**
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }

        return false;
    }
}
```
