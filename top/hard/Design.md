# Hard Collection Day 7 (Design)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/122/design/

## 1. [LeetCode 146](https://leetcode.com/problems/lru-cache/solution/) LRU Cache (medium)

- Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.
- Implement the `LRUCache` class:
    -   `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
    -   `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
    -   `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.
- The functions `get` and `put` must each run in `O(1)`average time complexity.
- **Example 1:**
    - **Input**
        - ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
        - `[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]`
    - **Output**
        - [null, null, null, 1, null, -1, null, -1, 3, 4]
    - **Explanation**
        ```
        LRUCache lruCache = new LRUCache(2);
        lruCache.put(1, 1); // cache is {1=1}
        lruCache.put(2, 2); // cache is {1=1, 2=2}
        lruCache.get(1);    // return 1
        lruCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
        lruCache.get(2);    // returns -1 (not found)
        lruCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
        lruCache.get(1);    // return -1 (not found)
        lruCache.get(3);    // return 3
        lruCache.get(4);    // return 4
        ```
- **Constraints:**
    -   `1 <= capacity <= 3000`
    -   `0 <= key <= 10^4`
    -   `0 <= value <= 10^5`
    -   At most `2 * 10^5` calls will be made to `get` and `put`.

### Solution

```java
class LRUCache {
    class Node {
        int key, value;
        Node previous, next;
        
        public Node() {
            
        }
        
        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }
    
    private int capacity;
    private HashMap<Integer, Node> map;
    private Node head, tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<>(capacity);
        head = new Node();
        tail = new Node();
        head.next = tail;
        tail.previous = head;
    }
    
    public int get(int key) {
        Node node = map.get(key);
        if (node == null) {
            return -1;
        }
        moveNodeToHead(node);
        return node.value;
    }
    
    public void put(int key, int value) {
        Node node = map.get(key);
        if (node == null) {
            if (map.size() >= capacity) {
                map.remove(tail.previous.key);
                removeTailNode();
            }
            Node newNode = new Node(key, value);
            map.put(key, newNode);
            addToHead(newNode);
        } else {
            node.value = value;
            moveNodeToHead(node);
        }
    }
    
    private void addToHead(Node newNode) {
        newNode.previous = head;
        newNode.next = head.next;
        head.next.previous = newNode;
        head.next = newNode;
    }
    
    private void moveNodeToHead(Node node) {
        removeNode(node);
        addToHead(node);
    }
    
    private void removeNode(Node node) {
        node.previous.next = node.next;
        node.next.previous = node.previous;
    }
    
    private void removeTailNode() {
        removeNode(tail.previous);
    }
}
```

## 2. [LeetCode 208](https://leetcode.com/problems/implement-trie-prefix-tree/) Implement Trie (Prefix Tree) (medium)

- A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.
- Implement the Trie class:
    -   `Trie()` Initializes the trie object.
    -   `void insert(String word)` Inserts the string `word` into the trie.
    -   `boolean search(String word)` Returns `true`if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
    -   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
- **Example 1:**
    - **Input**
        - ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
        - `[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]`
    - **Output**
        - [null, null, true, false, true, null, true]
    - **Explanation**
        ```
        Trie trie = new Trie();
        trie.insert("apple");
        trie.search("apple");   // return True
        trie.search("app");     // return False
        trie.startsWith("app"); // return True
        trie.insert("app");
        trie.search("app");     // return True
        ```
- **Constraints:**
    -   `1 <= word.length, prefix.length <= 2000`
    -   `word` and `prefix` consist only of lowercase English letters.
    -   At most `3 * 10^4` calls **in total** will be made to `insert`, `search`, and `startsWith`.

### Solution

```java
class Trie {
    static class TrieNode {
        Map<Character, TrieNode> children;
        boolean isWord;
        int count;
        
        public TrieNode() {
            children = new HashMap<>();
        }
    }
    
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        if (search(word)) {
            return;
        }
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode next = current.children.get(word.charAt(i));
            if (next == null) {
                next = new TrieNode();
                current.children.put(word.charAt(i), next);
            }
            current = next;
            current.count++;
        }
        current.isWord = true;
    }
    
    public boolean search(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode next = current.children.get(word.charAt(i));
            if (next == null) {
                return false;
            }
            current = next;
        }
        return current.isWord;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (int i = 0; i < prefix.length(); i++) {
            TrieNode next = current.children.get(prefix.charAt(i));
            if (next == null) {
                return false;
            }
            current = next;
        }
        return true;
    }
}
```

## 3. [LeetCode 341](https://leetcode.com/problems/flatten-nested-list-iterator/) Flatten Nested List Iterator (medium)

- You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.
- Implement the `NestedIterator` class:
    -   `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list `nestedList`.
    -   `int next()` Returns the next integer in the nested list.
    -   `boolean hasNext()` Returns `true` if there are still some integers in the nested list and `false`otherwise.
- Your code will be tested with the following pseudocode:
    ```
    initialize iterator with nestedList
    res = []
    while iterator.hasNext()
        append iterator.next() to the end of res
    return res
    ```
- If `res` matches the expected flattened list, then your code will be judged as correct.
- **Example 1:**
    - **Input:** nestedList = `[[1,1],2,[1,1]]`
    - **Output:** [1,1,2,1,1]
    - **Explanation:** By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
- **Example 2:**
    - **Input:** nestedList = `[1,[4,[6]]]`
    - **Output:** [1,4,6]
    - **Explanation:** By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].
- **Constraints:**
    -   `1 <= nestedList.length <= 500`
    -   The values of the integers in the nested list is in the range `[-10^6, 10^6]`.

### Solution

```java
public class NestedIterator implements Iterator<Integer> {
    private List<Integer> list;
    private int index;

    public NestedIterator(List<NestedInteger> nestedList) {
        list = new ArrayList<>();
        index = 0;
        flatten(nestedList);
    }
    
    private void flatten(List<NestedInteger> nestedList) {
        for (NestedInteger nestedInteger : nestedList) {
            if (nestedInteger.isInteger()) {
                list.add(nestedInteger.getInteger());
            } else {
                flatten(nestedInteger.getList());
            }
        }
    }

    @Override
    public Integer next() {
        if (!hasNext()) {
            return null;
        }
        return list.get(index++);
    }

    @Override
    public boolean hasNext() {
        return index < list.size();
    }
}
```

## 4. [LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/) Find Median from Data Stream (hard)

- The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.
    -   For example, for `arr = [2,3,4]`, the median is `3`.
    -   For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.
- Implement the MedianFinder class:
    -   `MedianFinder()` initializes the `MedianFinder` object.
    -   `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
    -   `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.
- **Example 1:**
    - **Input**
        - ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
        - `[[], [1], [2], [], [3], []]`
    - **Output**
        - [null, null, null, 1.5, null, 2.0]
    - **Explanation**
        ```
        MedianFinder medianFinder = new MedianFinder();
        medianFinder.addNum(1);    // arr = [1]
        medianFinder.addNum(2);    // arr = [1, 2]
        medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
        medianFinder.addNum(3);    // arr[1, 2, 3]
        medianFinder.findMedian(); // return 2.0
        ```
- **Constraints:**
    -   `-10^5 <= num <= 10^5`
    -   There will be at least one element in the data structure before calling `findMedian`.
    -   At most `5 * 10^4` calls will be made to `addNum` and `findMedian`.
- **Follow up:**
    -   If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?
    -   If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?

### Solution

```java
class MedianFinder {
    PriorityQueue<Integer> minHeap;
    PriorityQueue<Integer> maxHeap;
    
    public MedianFinder() {
        minHeap = new PriorityQueue<>();
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
    }
    
    public void addNum(int num) {
        maxHeap.offer(num);
        minHeap.offer(maxHeap.poll());
        if (minHeap.size() > maxHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
        // Time Complexity: O(logn)
    }
    
    public double findMedian() {
        if (minHeap.size() == maxHeap.size()) {
            return (double) (maxHeap.peek() + minHeap.peek()) * 0.5;
        } else {
            return maxHeap.peek();
        }
        // Time Complexity: O(1)
    }
}
```

## 5. [LeetCode 308](https://leetcode.com/problems/range-sum-query-2d-mutable/) Range Sum Query 2D - Mutable (hard)

- Given a 2D matrix `matrix`, handle multiple queries of the following types:
    1. **Update** the value of a cell in `matrix`.
    2. Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.
- Implement the NumMatrix class:
    -   `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
    -   `void update(int row, int col, int val)` **Updates** the value of `matrix[row][col]` to be `val`.
    -   `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2021/03/14/summut-grid.jpg)
    - **Input**
        - ["NumMatrix", "sumRegion", "update", "sumRegion"]
        - `[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [3, 2, 2], [2, 1, 4, 3]]`
    - **Output**
        - [null, 8, null, 10]
    - **Explanation**
        ```
        NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
        numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e. sum of the left red rectangle)
        numMatrix.update(3, 2, 2);       // matrix changes from left image to right image
        numMatrix.sumRegion(2, 1, 4, 3); // return 10 (i.e. sum of the right red rectangle)
        ```
- **Constraints:**
    -   `m == matrix.length`
    -   `n == matrix[i].length`
    -   `1 <= m, n <= 200`
    -   `-10^5 <= matrix[i][j] <= 10^5`
    -   `0 <= row < m`
    -   `0 <= col < n`
    -   `-10^5 <= val <= 10^5`
    -   `0 <= row1 <= row2 < m`
    -   `0 <= col1 <= col2 < n`
    -   At most `10^4` calls will be made to `sumRegion` and `update`.

### Solution 1: brute force

```java
class NumMatrix {
    private int[][] data;

    public NumMatrix(int[][] matrix) {
        data = matrix;
    }
    
    public void update(int row, int col, int val) {
        data[row][col] = val;
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        for (int i = row1; i <= row2; i++) {
            for (int j = col1; j <= col2; j++) {
                sum += data[i][j];
            }
        }
        return sum;
    }
}
```

### Solution 2: binary indexed tree

```java
class NumMatrix {
    private int rows;
    private int cols;
    private int[][] bit; // The BIT matrix

    private int lsb(int n) {
        // the line below allows us to directly capture the right most non-zero bit of a number
        return n & (-n);
    }

    private void updateBIT(int r, int c, int val) {
        // keep adding lsb(i) to i, lsb(j) to j and add val to bit[i][j]
        // Using two nested for loops, one for the rows and one for the columns
        for (int i = r; i <= rows; i += lsb(i)) {
            for (int j = c; j <= cols; j += lsb(j)) {
                this.bit[i][j] += val;
            }
        }
    }

    private int queryBIT(int r, int c) {
        int sum = 0;
        // keep subtracting lsb(i) to i, lsb(j) to j and obtain the final sum as the sum of non-overlapping sub-rectangles
        // Using two nested for loops, one for the rows and one for the columns
        for (int i = r; i > 0; i -= lsb(i)) {
            for (int j = c; j > 0; j -= lsb(j)) {
                sum += this.bit[i][j];
            }
        }
        return sum;
    }

    private void buildBIT(int[][] matrix) {
        for (int i = 1; i <= rows; ++i) {
            for (int j = 1; j <= cols; ++j) {
                // call update function on each of the entries present in the matrix
                int val = matrix[i - 1][j - 1];
                updateBIT(i, j, val);
            }
        }
    }

    public NumMatrix(int[][] matrix) {
        rows = matrix.length;
        if (rows == 0) return;
        cols = matrix[0].length;
        bit = new int[rows + 1][];
        // Using 1 based indexing, hence resizing the bit array to (rows + 1, cols + 1)
        for (int i = 1; i <= rows; ++i) {
            bit[i] = new int[cols + 1];
        }
        buildBIT(matrix);
    }

    public void update(int row, int col, int val) {
        int old_val = sumRegion(row, col, row, col);
        // handling 1-based indexing
        row++; col++;
        int diff = val - old_val;
        updateBIT(row, col, diff);
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        // handling 1-based indexing
        row1++; col1++; row2++; col2++;
        int a = queryBIT(row2, col2);
        int b = queryBIT(row1 - 1, col1 - 1);
        int c = queryBIT(row2, col1 - 1);
        int d = queryBIT(row1 - 1, col2);
        return (a + b) - (c + d);
    }
}
```

