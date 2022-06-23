# Blind 75 Day 2 (Linked List)

## 1. Reverse a Linked List (easy)

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

## 2. Detect Cycle in a Linked List (easy)

[LeetCode 141](https://leetcode.com/problems/linked-list-cycle/)

### Solution: two pointers

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

## 3. Merge Two Sorted Lists (easy)

[LeetCode 21](https://leetcode.com/problems/merge-two-sorted-lists/)

### Solution: 谁小移谁

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

## 4. Merge K Sorted Lists (hard)

[LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/)

### Solution: priority queue (min heap)

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, new Comparator<ListNode>() {
        @Override
        public int compare(ListNode n1, ListNode n2) {
            if (n1.val == n2.val) {
                return 0;
            }
            return n1.val < n2.val ? -1 : 1;
        }
    });
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    for (ListNode node : lists) {
        if (node != null) {
            pq.offer(node);
        }
    }
    while (!pq.isEmpty()) {
        tail.next = pq.poll();
        tail = tail.next;
        if (tail.next != null) {
            pq.offer(tail.next);
        }
    }
    return dummy.next;
}
```

Time Complexity: O(nlogk)

Space Complexity: O(k)

## 5. Remove Nth Node From End Of List (medium)

[LeetCode 19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

### Solution: like sliding window

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

## 6. Reorder List (medium)

[LeetCode 143](https://leetcode.com/problems/reorder-list/)

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null) {
        return;
    }
    ListNode middle = findMiddle(head);
    ListNode one = head;
    ListNode two = middle.next;
    middle.next = null;
    head = merge(one, reverse(two));
    return;
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

private ListNode reverse(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode previous = null;
    ListNode current = head;
    ListNode next = null;
    while (current != null) {
        next = current.next;
        current.next = previous;
        previous = current;
        current = next;
    }
    return previous;
}

private ListNode merge(ListNode one, ListNode two) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    while (one != null && two != null) {
        current.next = one;
        one = one.next;
        current.next.next = two;
        two = two.next;
        current = current.next.next;
    }
    if (one != null) {
        current.next = one;
    }
    if (two != null) {
        current.next = two;
    }
    return dummy.next;
}
```

Time Complexity: O(n)

Space Complexity: O(1)