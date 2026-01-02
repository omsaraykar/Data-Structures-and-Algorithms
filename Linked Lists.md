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
---
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
---
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
---
### Q4] Find First Node of Loop in Linked List
**Using Floyd's Cycle-Finding Algorithm** **`TC: O(n), SC: O(1)`**
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) return null;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) break;
        }

        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }

        return null;
    }
}
```
---
### Q5] Find length of Loop
**Using Floyd's Cycle-Finding Algorithm** **`TC: O(n), SC: O(1)`**
```java
class Solution {
    public int lengthOfLoop(Node head) {
        if (head == null || head.next == null) return 0;

        Node slow = head;
        Node fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) break;
        }
        
        int len = 0;
        if (slow == fast) {
            do {
                slow = slow.next;
                len++;
            } while (slow != fast);
        } 

        return len;
    }
}
```
---
### Q6] Palindrome Linked List
**Reversing the second half of the linked list** **`TC: O(n), SC: O(1)`**
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode mid = reverseList(slow.next);
        while(mid != null){
            if(head.val != mid.val) return false;
            mid = mid.next;
            head = head.next;
        }

        return true;
    }

    private ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode rev = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return rev;
    }
}
```
---
### Q7] Odd Even Linked List
```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;

        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenHead = even;

        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;

        return head;
    }
}
```
---
### Q8] Remove Nth Node from the Linked List
**Using two pointers**
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode slow = head;
        ListNode fast = head;

        for (int i = 1; i <= n; i++) {
            fast = fast.next;
        }
        if (fast == null) return head.next;

        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;

        return head;
    }
}
```
---
### Q9] Remove Middle Node from Linked List
> [!INFO]
> Keep prev to make life easy
```java
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }

        ListNode slow = head;
        ListNode fast = head;
        ListNode prev = null;

        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }

        prev.next = slow.next;

        return head;
    }
}

```
---
### Q10] Insertion Point of two Linked Lists
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA;
        ListNode b = headB;

        int lenA = 0;
        while (a != null) {
            lenA++;
            a = a.next;
        }

        int lenB = 0;
        while (b != null) {
            lenB++;
            b = b.next;
        }

        int diff = Math.abs(lenA - lenB);
        a = headA;
        b = headB;
        if (lenA > lenB) {
            for (int i = 0; i < diff; i++) {
                a = a.next;
            }
        } else {
            for (int i = 0; i < diff; i++) {
                b = b.next;
            }
        }

        while (a != b) {
            a = a.next;
            b = b.next;
        }

        return a;
    }
}
```
---
### Q11] Sort Linked List of 0's, 1's and 2's
**M: Count the number of 0's, 1's and 2's**
```java
class Solution {
    public Node segregate(Node head) {
        Node temp = head;
        
        int zeros = 0;
        int ones = 0;
        int twos = 0;
        
        while (temp != null) {
            if (temp.data == 0) zeros++;
            else if (temp.data == 1) ones++;
            else twos++;
            temp = temp.next;
        }
        
        temp = head;
        while (temp != null) {
            if (zeros != 0) {
                temp.data = 0;
                zeros--;
            }
            else if (ones != 0) {
                temp.data = 1;
                ones--;
            }
            else {
                temp.data = 2;
                twos--;
            }
            temp = temp.next;
        }
        
        return head;
    }
}
```
---
### Q12] Sort Linked List

---
### Q13] Add 1 to a Linked List Number
**M: Using Recursion**
```java
class Solution {
    public Node addOne(Node head) {
        int digit = addWithCarry(head);

        if (digit > 0) {
            Node newNode = new Node(digit);
            newNode.next = head;
            return newNode;
        }

        return head;
    }
    
    private int addWithCarry(Node head) {
        if (head == null) {
            return 1;
        }

        int num = head.data + addWithCarry(head.next);
        int digit_0 = num % 10;
        int digit_1 = num / 10;
        
        head.data = digit_0;
        return digit_1;
    }
}
```
---
### Q14] Add two Numbers
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;

        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry;

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
        }

        return dummy.next;
    }
}
```
---
### Q15] Delete all occurrences of a given key in DLL
```java
class Solution {
    public static Node deleteAllOccurOfX(Node head, int x) {
        Node curr = head;

        while (curr != null) {
            if (curr.data == x) {
                if (curr == head) {
                    head = head.next;
                }

                if (curr.prev != null) curr.prev.next = curr.next;
                if (curr.next != null) curr.next.prev = curr.prev;
            } 
            curr = curr.next;
        }
        return head;
    }
} 
```
---
### Q16] Find Pairs of Given Sum in DLL
**M: Two Pointers**
```java
class Solution {
    public static ArrayList<ArrayList<Integer>> findPairsWithGivenSum(int target, Node head) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        if (head == null) return res;

        Node left = head;
        Node right = head;
        while (right.next != null) {
            right = right.next;
        }

        while (left != null && right != null && left.data < right.data) {
            int sum = left.data + right.data;

            if (sum == target) {
                res.add(new ArrayList<>(Arrays.asList(left.data, right.data)));
                left = left.next;
                right = right.prev;
            }
            else if (sum < target) {
                left = left.next;
            }
            else {
                right = right.prev;
            }
        }

        return res;
    }
}
```
---
### Q17] Remove Duplicates from a Sorted DLL
```java
class Solution {
    Node removeDuplicates(Node head) {
        Node curr = head;

        while (curr != null && curr.next != null) {
            if (curr.data == curr.next.data) {
                curr.next = curr.next.next;
                if (curr.next != null) {
                    curr.next.prev = curr;
                }
            } else {
                curr = curr.next;
            }
        }

        return head;
    }
}
```
---
### Q18] Rotate List
```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;

        int length = 1;
        ListNode b = head;
        while (b.next != null) {
            length++;
            b = b.next;
        }
        k = k % length;
        if (k == 0) return head;

        ListNode a = head;
        for (int i = 0; i < length - k - 1; i++) {
            a = a.next;
        }
        
        ListNode newHead = a.next;
        a.next = null;
        b.next = head;

        return newHead;
    }
```
---
### Q19] Copy List with Random Pointer
**M1: Using HashMap** **`TC: O(n), SC: O(n)`**
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        
        Node newHead = new Node(0);
        Map<Node, Node> map = new HashMap<>();

        Node a = head;
        Node b = newHead;
        while (a != null) {
            b.next = new Node(a.val);
            map.put(a, b.next);
            b = b.next;
            a = a.next;
        }
        newHead = newHead.next;
        
        a = head;
        b = newHead;
        while (a != null) {
            Node random = map.get(a.random);
            b.random = random;
            a = a.next;
            b = b.next;
        }
        
        return newHead;
    }
}
```
**M2: 3 Pass Solution `TC: O(n), SC(1)`**
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;

        // 1. Clone nodes and insert them
        Node curr = head;
        while (curr != null) {
            Node copy = new Node(curr.val);
            copy.next = curr.next;
            curr.next = copy;
            curr = copy.next;
        }

        // 2. Assign random pointers
        curr = head;
        while (curr != null) {
            if (curr.random != null) {
                curr.next.random = curr.random.next; // 🤓
            }
            curr = curr.next.next;
        }

        // 3. Separate the copied list
        Node newHead = new Node(0);
        Node temp = newHead;
        curr = head;

        while (curr != null) {
            temp.next = curr.next;
            curr.next = curr.next.next;

            curr = curr.next;
            temp = temp.next;
        }

        return newHead.next;
    }
}
```
---
### Q20] Flatten a Linked List
**M1: Using Array List** **`TC: O(n log(n)), SC: O(n)`**
```java
class Solution {
    public Node flatten(Node root) {
        ArrayList<Node> list = new ArrayList<>();
        Node curr = root;
        while (curr != null) {
            Node temp = curr;
            while (temp != null) {
                list.add(temp);
                temp = temp.bottom;
            }
            curr = curr.next;
        }

        list.sort(Comparator.comparingInt(n -> n.data));
        list.add(null);

        for (int i = 0; i < list.size() - 1; i++) {
            list.get(i).bottom = list.get(i + 1);
        }

        return root;
    }
}
```
**M2: Using Recursion `TC: O(n), SC O(1)`**
```java
class Solution {
    public Node flatten(Node root) {
        if (root == null || root.next == null) {
            return root;
        }

        root.next = flatten(root.next);
        root = merge(root, root.next);

        return root;
    }
    
    private Node merge(Node a, Node b) {
        if (a == null) return b;
        if (b == null) return a;

        if (a.data < b.data) {
            a.bottom = merge(a.bottom, b);
            return a;
        } else {
            b.bottom = merge(a, b.bottom);
            return b;
        }
    }
}
```
---
### Q21] Reverse Nodes in k-Group
```java
class Solution {
    public int repeatedNTimes(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (i + 1 < n && nums[i] == nums[i + 1]) return nums[i];
            if (i + 2 < n && nums[i] == nums[i + 2]) return nums[i];
            if (i + 3 < n && nums[i] == nums[i + 3]) return nums[i];
        }

        return -1;
    }
}
```
---