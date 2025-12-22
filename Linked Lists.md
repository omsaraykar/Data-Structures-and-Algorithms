## Questions
### Q1] Reverse a Linked List
**M1: Iterative (3 pointers)** `TC: O(n), SC: O(1)`
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
**M2: Recursive** `TC: O(n), SC: O(n)`
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