# Blind 75 Day 2 (Linked List)

## 1. [LeetCode 206](https://leetcode.com/problems/reverse-linked-list/) Reverse a Linked List (easy)

- Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" style="zoom: 67%;" />
    - **Input:** head = [1,2,3,4,5]
    - **Output:** [5,4,3,2,1]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2]
    - **Output:** [2,1]
- **Example 3:**
    - **Input:** head = []
    - **Output:** []
- **Constraints:**
    -   The number of nodes in the list is the range `[0, 5000]`.
    -   `-5000 <= Node.val <= 5000`
- **Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

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

## 2. [LeetCode 141](https://leetcode.com/problems/linked-list-cycle/) Detect Cycle in a Linked List (easy)

- Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
- There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.
- Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png" style="zoom: 67%;" />
    - **Input:** head = [3,2,0,-4], pos = 1
    - **Output:** true
    - **Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png" style="zoom:67%;" />
    - **Input:** head = [1,2], pos = 0
    - **Output:** true
    - **Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.
- **Example 3:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png" style="zoom:67%;" />
    - **Input:** head = [1], pos = -1
    - **Output:** false
    - **Explanation:** There is no cycle in the linked list.
- **Constraints:**
    -   The number of the nodes in the list is in the range `[0, 10^4]`.
    -   `-10^5 <= Node.val <= 10^5`
    -   `pos` is `-1` or a **valid index** in the linked-list.
- **Follow up:** Can you solve it using `O(1)`(i.e. constant) memory?

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

## 3. [LeetCode 21](https://leetcode.com/problems/merge-two-sorted-lists/) Merge Two Sorted Lists (easy)

- You are given the heads of two sorted linked lists `list1` and `list2`.
- Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.
- Return _the head of the merged linked list_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg" style="zoom:67%;" />
    - **Input:** list1 = [1,2,4], list2 = [1,3,4]
    - **Output:** [1,1,2,3,4,4]
- **Example 2:**
    - **Input:** list1 = [], list2 = []
    - **Output:** []
- **Example 3:**
    - **Input:** list1 = [], list2 = [0]
    - **Output:** [0]
- **Constraints:**
    -   The number of nodes in both lists is in the range `[0, 50]`.
    -   `-100 <= Node.val <= 100`
    -   Both `list1` and `list2` are sorted in **non-decreasing** order.

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

## 4. [LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/) Merge K Sorted Lists (hard)

- You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.
- _Merge all the linked-lists into one sorted linked-list and return it._
- **Example 1:**
    - **Input:** lists = `[[1,4,5],[1,3,4],[2,6]]`
    - **Output:** [1,1,2,3,4,4,5,6]
- **Example 2:**
    - **Input:** lists = `[]`
    - **Output:** []
- **Example 3:**
    - **Input:** lists = `[[]]`
    - **Output:** []
- **Constraints:**
    -   `k == lists.length`
    -   `0 <= k <= 10^4`
    -   `0 <= lists[i].length <= 500`
    -   `-10^4 <= lists[i][j] <= 10^4`
    -   `lists[i]` is sorted in **ascending order**.
    -   The sum of `lists[i].length` will not exceed `10^4`.

### Solution: priority queue (min heap)

```java
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    PriorityQueue<ListNode> pq = new PriorityQueue<>(lists.length, (n1, n2) -> {
        if (n1.val == n2.val) {
            return 0;
        }
        return n1.val < n2.val ? -1 : 1;
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

## 5. [LeetCode 19](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) Remove Nth Node From End Of List (medium)

- Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4,5], n = 2
    - **Output:** [1,2,3,5]
- **Example 2:**
    - **Input:** head = [1], n = 1
    - **Output:** []
- **Example 3:**
    - **Input:** head = [1,2], n = 1
    - **Output:** [1]
- **Constraints:**
    -   The number of nodes in the list is `sz`.
    -   `1 <= sz <= 30`
    -   `0 <= Node.val <= 100`
    -   `1 <= n <= sz`
- **Follow up:** Could you do this in one pass?

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

## 6. [LeetCode 143](https://leetcode.com/problems/reorder-list/) Reorder List (medium)

- You are given the head of a singly linked-list. The list can be represented as: `L0 → L1 → … → Ln - 1 → Ln`
- _Reorder the list to be on the following form:_ `L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …`
- You may not modify the values in the list's nodes. Only nodes themselves may be changed.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4]
    - **Output:** [1,4,2,3]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4,5]
    - **Output:** [1,5,2,4,3]
- **Constraints:**
    -   The number of nodes in the list is in the range `[1, 5 * 10^4]`.
    -   `1 <= Node.val <= 1000`

### Solution

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