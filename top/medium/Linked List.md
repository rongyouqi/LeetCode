# Medium Collection Day 1 (Linked List)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/107/linked-list/

## 1. Add Two Numbers (medium)

[LeetCode 2](https://leetcode.com/problems/add-two-numbers/)

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    int value = 0;
    while (l1 != null || l2 != null || value != 0) {
        if (l1 != null) {
            value += l1.val;
            l1 = l1.next;
        }
        if (l2 != null) {
            value += l2.val;
            l2 = l2.next;
        }
        current.next = new ListNode(value % 10);
        value /= 10;
        current = current.next;
    }
    return dummy.next;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Odd Even Linked List (medium)

[LeetCode 328](https://leetcode.com/problems/odd-even-linked-list/)

```java
public ListNode oddEvenList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
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
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. Intersection of Two Linked Lists (easy)

[LeetCode 160](https://leetcode.com/problems/intersection-of-two-linked-lists/)

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode pA = headA;
    ListNode pB = headB;
    while (pA != pB) {
        pA = pA == null ? headB : pA.next;
        pB = pB == null ? headA : pB.next;
    }
    return pA;
}
```

Time Complexity: O(m + n)

Space Complexity: O(1)

