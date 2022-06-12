# LeetCode

## Blind 75

https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions

## LeetCode Top Interview Questions

### Easy Collection

https://leetcode.com/explore/interview/card/top-interview-questions-easy/

### Medium Collection

https://leetcode.com/explore/interview/card/top-interview-questions-medium/

### Hard Collection

https://leetcode.com/explore/interview/card/top-interview-questions-hard/

## 花花酱

https://zxi.mytechroad.com/blog/leetcode-problem-categories/

https://www.youtube.com/channel/UC5xDNEcvb1vgw3lE21Ack2Q

## 肌肉记忆

### 1. Sorting Algorithms



### 2. Reverse Linked List (easy)

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

### 3. Tree Traversal



### 4. Hashmap



### 5. LRU Cache