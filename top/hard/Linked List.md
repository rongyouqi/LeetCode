# Hard Collection Day 2 (Linked List)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/117/linked-list/

## 1. Merge K Sorted Lists (hard)

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

## 2. Sort List (medium)

[LeetCode 148](https://leetcode.com/problems/sort-list/)

### Solution 1

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode mid = findMiddle(head);
    ListNode midNext = mid.next;
    mid.next = null;
    ListNode left = sortList(head);
    ListNode right = sortList(midNext);
    return merge(left, right);
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

private ListNode merge(ListNode one, ListNode two) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    while (one != null && two != null) {
        if (one.val < two.val) {
            current.next = one;
            one = one.next;
        } else {
            current.next = two;
            two = two.next;
        }
        current = current.next;
    }
    current.next = one != null ? one : two;
    return dummy.next;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

### Solution 2

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    int len = getLength(head);
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    for (int width = 1; width < len; width *= 2) {
        ListNode left = dummy.next;
        ListNode beforeMerge = null;
        for (int j = 0; j < len; j += width * 2) {
            ListNode temp = left;
            for (int i = 1; i < width && temp != null; i++) {
                temp = temp.next;
            }
            if (temp == null || temp.next == null) {
                break;
            }
            ListNode right = temp.next;
            temp.next = null;
            ListNode rest = null;
            if (right.next != null) {
                rest = right.next;
                ListNode beforeRest = right;
                for (int i = 1; i < width && rest != null; i++) {
                    beforeRest = beforeRest.next;
                    rest = rest.next;
                }
                beforeRest.next = null;
            }
            ListNode merged = merge(left, right);
            temp = merged;
            while (temp.next != null) {
                temp = temp.next;
            }
            temp.next = rest;
            if (beforeMerge != null) {
                beforeMerge.next = merged;
            }
            beforeMerge = temp;
            left = temp.next;
            if (j == 0) {
                dummy.next = merged;
            }
        }
    }
    return dummy.next;
}

private int getLength(ListNode head) {
    int len = 0;
    while (head != null) {
        len++;
        head = head.next;
    }
    return len;
}

private ListNode merge(ListNode one, ListNode two) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    while (one != null && two != null) {
        if (one.val < two.val) {
            current.next = one;
            one = one.next;
        } else {
            current.next = two;
            two = two.next;
        }
        current = current.next;
    }
    current.next = one != null ? one : two;
    return dummy.next;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(1)

## 3. Copy List with Random Pointer (medium)

[LeetCode 138](https://leetcode.com/problems/copy-list-with-random-pointer/)

### Solution 1: hashmap

```java
public Node copyRandomList(Node head) {
    if (head == null) {
        return head;
    }
    Map<Node, Node> map = new HashMap<>();
    Node oldNode = head;
    Node newNode = new Node(head.val);
    map.put(oldNode, newNode);
    while (oldNode != null) {
        if (oldNode.random != null) {
            if (!map.containsKey(oldNode.random)) {
                map.put(oldNode.random, new Node(oldNode.random.val));
            }
            newNode.random = map.get(oldNode.random);
        }
        if (oldNode.next != null) {
            if (!map.containsKey(oldNode.next)) {
                map.put(oldNode.next, new Node(oldNode.next.val));
            }
            newNode.next = map.get(oldNode.next);
        }
        oldNode = oldNode.next;
        newNode = newNode.next;
    }
    return map.get(head);
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2

```java
public Node copyRandomList(Node head) {
    if (head == null) {
        return head;
    }
    Node ptr = head;
    while (ptr != null) {
        Node newNode = new Node(ptr.val);
        newNode.next = ptr.next;
        ptr.next = newNode;
        ptr = newNode.next;
        // A->A'->B->B'->C->C'
    }
    ptr = head;
    while (ptr != null) {
        ptr.next.random = (ptr.random == null) ? null : ptr.random.next;
        ptr = ptr.next.next;
    }
    Node oldNode = head;
    Node newNode = head.next;
    Node result = head.next;
    while (oldNode != null) {
        oldNode.next = oldNode.next.next;
        newNode.next = (newNode.next == null) ? null : newNode.next.next;
        oldNode = oldNode.next;
        newNode = newNode.next;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

