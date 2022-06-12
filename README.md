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

[LeetCode 912](https://leetcode.com/problems/sort-an-array/)

#### Solution 1: bubble sort

todo

[LeetCode 75: Sort Colors (medium)](https://leetcode.com/problems/sort-colors/)

```java
public void sortColors(int[] nums) {
    // todo
}
```

Time Complexity: O(n)

Space Complexity: O(1)

[LeetCode 539: Minimum Time Difference (medium)](https://leetcode.com/problems/minimum-time-difference/)

```java
public int findMinDifference(List<String> timePoints) {
    // todo
}
```

Time Complexity: O(n)

Space Complexity: O(24 * 60) = O(1)

### 2. Reverse Linked List (easy)

[LeetCode 206](https://leetcode.com/problems/reverse-linked-list/)

#### Solution 1: iterative

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

#### Solution 2: recursive

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

### 3. Binary Tree Traversal

#### Preorder 根左右

[LeetCode 144](https://leetcode.com/problems/binary-tree-preorder-traversal/)

##### Solution 1: recursive

```java
public List<Integer> preorderTraversal(TreeNode root) {
    // todo
}
```

##### Solution 2: iterative

```java
public List<Integer> preorderTraversal(TreeNode root) {
    // todo
}
```

#### Inorder 左根右

[LeetCode 94](https://leetcode.com/problems/binary-tree-inorder-traversal/)

##### Solution 1: recursive

```java
public List<Integer> inorderTraversal(TreeNode root) {
    // todo
}
```

##### Solution 2: iterative

```java
public List<Integer> inorderTraversal(TreeNode root) {
    // todo
}
```

#### Postorder 左右根

[LeetCode 145](https://leetcode.com/problems/binary-tree-postorder-traversal/)

##### Solution 1: recursive

```java
public List<Integer> postorderTraversal(TreeNode root) {
    // todo
}
```

##### Solution 2: iterative

```java
public List<Integer> postorderTraversal(TreeNode root) {
    // todo
}
```

#### Level Order

[LeetCode 297: Serialize and Deserialize Binary Tree (hard)](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

```java
public String serialize(TreeNode root) {
    // todo
}

public TreeNode deserialize(String data) {
    // todo
}
```

[LeetCode 102: Binary Tree Level Order Traversal (medium)](https://leetcode.com/problems/binary-tree-level-order-traversal/)

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    // todo
}
```

[LeetCode 103: Binary Tree Zigzag Level Order Traversal (medium)](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    // todo
}
```

[LeetCode 107: Binary Tree Level Order Traversal II (medium)](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)

```java
public List<List<Integer>> levelOrderBottom(TreeNode root) {
    // todo
}
```

[LeetCode 314: Binary Tree Vertical Order Traversal (medium)](https://leetcode.com/problems/binary-tree-vertical-order-traversal/)

```java
public List<List<Integer>> verticalOrder(TreeNode root) {
    // todo
}
```

### 4. Priority Queue

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

### 5. Implement Hashmap

todo

### 6. Implement LRU Cache

todo