# Blind 75 Day 4 (Heap)

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

## 1. [LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/) Merge K Sorted Lists (hard)

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

### Solution: priority queue

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

## 2. [LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/) Top K Frequent Elements (medium)

- Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.
- **Example 1:**
    - **Input:** nums = [1,1,1,2,2,3], k = 2
    - **Output:** [1,2]
- **Example 2:**
    - **Input:** nums = [1], k = 1
    - **Output:** [1]
- **Constraints:**
    -   `1 <= nums.length <= 10^5`
    -   `k` is in the range `[1, the number of unique elements in the array]`.
    -   It is **guaranteed** that the answer is **unique**.
- **Follow up:** Your algorithm's time complexity must be better than `O(nlogn)`, where n is the array's size.

### Solution 1: hashmap + heap

```java
public int[] topKFrequent(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return null;
    }
    if (nums.length <= k) {
        return nums;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    PriorityQueue<Map.Entry<Integer, Integer>> minHeap = new PriorityQueue<>(k, new Comparator<Map.Entry<Integer, Integer>>() {
        @Override
        public int compare(Map.Entry<Integer, Integer> e1, Map.Entry<Integer, Integer> e2) {
            return e1.getValue().compareTo(e2.getValue());
        }
    });
    for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
        if (minHeap.size() < k) {
            minHeap.offer(entry);
        } else if (entry.getValue() > minHeap.peek().getValue()) {
            minHeap.poll();
            minHeap.offer(entry);
        }
    }
    int[] result = new int[minHeap.size()];
    for (int i = minHeap.size() - 1; i >= 0; i--) {
        result[i] = minHeap.poll().getKey();
    }
    return result;
}
```

Time Complexity: O(nlogk)

Space Complexity: O(n)

### Solution 2: quick select

```java
public int[] topKFrequent(int[] nums, int k) {
    if (nums == null || nums.length == 0) {
        return null;
    }
    if (nums.length <= k) {
        return nums;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    int n = map.size();
    int[] unique = new int[n];
    int i = 0;
    for (int num : map.keySet()) {
        unique[i] = num;
        i++;
    }
    quickselect(0, n - 1, n - k, map, unique);
    return Arrays.copyOfRange(unique, n - k, n);
}

private void quickselect(int left, int right, int k_smallest, Map<Integer, Integer> map, int[] unique) {
    if (left == right) {
        return;
    }
    int pivot = left + new Random().nextInt(right - left);
    pivot = partition(left, right, pivot, map, unique);
    if (pivot == k_smallest) {
        return;
    } else if (pivot < k_smallest) {
        quickselect(pivot + 1, right, k_smallest, map, unique);
    } else {
        quickselect(left, pivot - 1, k_smallest, map, unique);
    }
}

private int partition(int left, int right, int pivot, Map<Integer, Integer> map, int[] unique) {
    int frequency = map.get(unique[pivot]);
    swap(unique, pivot, right);
    int index = left;
    for (int i = left; i < right; i++) {
        if (map.get(unique[i]) < frequency) {
            swap(unique, index, i);
            index++;
        }
    }
    swap(unique, index, right);
    return index;
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 3. [LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/) Find Median from Data Stream

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

