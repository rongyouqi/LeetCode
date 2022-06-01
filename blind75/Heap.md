# Blind 75 Day 4 (Heap)

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
```

## 1. Merge K Sorted Lists (hard)

[LeetCode 23](https://leetcode.com/problems/merge-k-sorted-lists/)

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

## 2. Top K Frequent Elements (medium)

[LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/)

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

## 3. Find Median from Data Stream

[LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/)

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

