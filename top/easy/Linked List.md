# Easy Collection Day 2 (Linked List)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/93/linked-list/

## 1. Delete Node in a Linked List (easy)

[LeetCode 237](https://leetcode.com/problems/delete-node-in-a-linked-list/)

```java
public void deleteNode(ListNode node) {
    // assumption: node is not a tail node in the list
    node.val = node.next.val;
    node.next = node.next.next;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 2. Remove Nth Node From End Of List (medium)

[LeetCode 19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    // assumption: 1 <= n <= number of nodes in the list
    if (head == null) {
        return null;
    }
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode slow = dummy;
    ListNode fast = dummy;
    for (int i = 0; i <= n; i++) {
        fast = fast.next;
    }
    while (fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    slow.next = slow.next.next;
    return dummy.next;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. Reverse Linked List (easy)

[LeetCode 206](https://leetcode.com/problems/reverse-linked-list/)

### Solution 1: iterative

```java
public ListNode reverseList(ListNode head) {
    // corner case
    if (head == null) {
        return null;
    }
    ListNode previous = null;
    ListNode current = head;
    ListNode next = null;
    while (current != null) {
        next = current.next; // store next node
        current.next = previous; // reverse
        previous = current; // previous moves one step
        current = next; // next moves one step
    }
    return previous;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 2: recursive

```java
public ListNode reverseList(ListNode head) {
    // base case
    if (head == null || head.next == null) {
        return head;
    }
    // recursive step
    ListNode newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 4. Merge Two Sorted Lists (easy)

[LeetCode 21](https://leetcode.com/problems/merge-two-sorted-lists/)

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    if (list1 == null && list2 == null) {
        return null;
    }
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            tail.next = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
        }
        tail = tail.next;
    }
    if (list1 == null) {
        tail.next = list2;
    }
    if (list2 == null) {
        tail.next = list1;
    }
    return dummy.next;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Palindrome Linked List (easy)

[LeetCode 234](https://leetcode.com/problems/palindrome-linked-list/)

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }
    ListNode middle = findMiddle(head);
    ListNode newHead = reverseList(middle.next);
    while (newHead != null) {
        if (head.val != newHead.val) {
            return false;
        }
        head = head.next;
        newHead = newHead.next;
    }
    return true;
}

private ListNode findMiddle(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode slow = head;
    ListNode fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

private ListNode reverseList(ListNode head) {
    // corner case
    if (head == null) {
        return null;
    }
    ListNode previous = null;
    ListNode current = head;
    ListNode next = null;
    while (current != null) {
        next = current.next; // store next node
        current.next = previous; // reverse
        previous = current; // previous moves one step
        current = next; // next moves one step
    }
    return previous;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. Linked List Cycle (easy)

[LeetCode 141](https://leetcode.com/problems/linked-list-cycle/)

```java
public boolean hasCycle(ListNode head) {
    if (head == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```

Time Complexity: O(n)

Space omplexity: O(1)