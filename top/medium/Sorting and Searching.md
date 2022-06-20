# Medium Collection Day 3 (Sorting and Searching)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/110/sorting-and-searching/

## 1. Sort Colors (medium)

[LeetCode 75](https://leetcode.com/problems/sort-colors/)

```java
public void sortColors(int[] nums) {
    if (nums == null || nums.length == 0) {
        return;
    }
    int i = 0, j = 0, k = nums.length - 1;
    while (j <= k) {
        if (nums[j] == 0) {
            swap(nums, i++, j++);
        } else if (nums[j] == 1) {
            j++;
        } else {
            swap(nums, j, k--);
        }
    }
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

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

## 3. Kth Largest Element in an Array (medium)

[LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/)

### Solution 1: minHeap

```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    for (int i = 0; i < k; i++) {
        minHeap.offer(nums[i]);
    }
    for (int i = k; i < nums.length; i++) {
        if (nums[i] > minHeap.peek()) {
            minHeap.poll();
            minHeap.offer(nums[i]);
        }
    }
    return minHeap.peek();
}
```

Time Complexity: O(nlogk)

Space Complexity: O(k)

### Solution 2: quick select

```java
public int findKthLargest(int[] nums, int k) {
    return quickselect(nums, 0, nums.length - 1, nums.length - k);
}

private int quickselect(int[] nums, int left, int right, int k_smallest) {
    if (left == right) {
        return nums[left];
    }
    int pivot = left + new Random().nextInt(right - left);
    pivot = partition(nums, left, right, pivot);
    if (k_smallest == pivot) {
        return nums[k_smallest];
    } else if (k_smallest < pivot) {
        return quickselect(nums, left, pivot - 1, k_smallest);
    }
    return quickselect(nums, pivot + 1, right, k_smallest);
}

private int partition(int[] nums, int left, int right, int pivot) {
    int pivot_num = nums[pivot];
    swap(nums, pivot, right);
    int temp = left;
    for (int i = left; i <= right; i++) {
        if (nums[i] < pivot_num) {
            swap(nums, temp, i);
            temp++;
        }
    }
    swap(nums, temp, right);
    return temp;
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. Find Peak Element (medium)

[LeetCode 162](https://leetcode.com/problems/find-peak-element/)

```java
public int findPeakElement(int[] nums) {
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < nums[mid + 1]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return left;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 5. Search for a Range (medium)

[LeetCode 34: Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```java
public int[] searchRange(int[] nums, int target) {
    int first = firstOccurrence(nums, target);
    if (first == - 1) {
        return new int[]{-1, -1};
    }
    int last = lastOccurrence(nums, target);
    return new int[]{first, last};
}

private int firstOccurrence(int[] array, int target) {
    if (array == null || array.length == 0) {
        return -1;
    }
    int left = 0, right = array.length - 1;
    while (left < right - 1) {
        int mid = left + (right - left) / 2;
        if (array[mid] >= target) {
            right = mid;
        } else {
            left = mid;
        }
    }
    if (array[left] == target) {
        return left;
    }
    if (array[right] == target) {
        return right;
    }
    return -1;
}

private int lastOccurrence(int[] array, int target) {
    if (array == null || array.length == 0) {
        return -1;
    }
    int left = 0, right = array.length - 1;
    while (left < right - 1) {
        int mid = left + (right - left) / 2;
        if (array[mid] <= target) {
            left = mid;
        } else {
            right = mid;
        }
    }
    if (array[right] == target) {
        return right;
    }
    if (array[left] == target) {
        return left;
    }
    return -1;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 6. Merge Intervals (medium)

[LeetCode 56](https://leetcode.com/problems/merge-intervals/solution/)

```java
public int[][] merge(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return intervals;
    }
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    List<int[]> result = new ArrayList<>();
    for (int[] interval : intervals) {
        if (result.isEmpty() || result.get(result.size() - 1)[1] < interval[0]) {
            result.add(interval);
        } else {
            result.get(result.size() - 1)[1] = Math.max(result.get(result.size() - 1)[1], interval[1]);
        }
    }
    return result.toArray(new int[result.size()][]);
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

## 7. Meeting Rooms II (medium)

[LeetCode 253](https://leetcode.com/problems/meeting-rooms-ii/)

```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    PriorityQueue<Integer> minHeap = new PriorityQueue<>(intervals.length);
    minHeap.offer(intervals[0][1]);
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= minHeap.peek()) {
            minHeap.poll();
        }
        minHeap.offer(intervals[i][1]);
    }
    return minHeap.size();
}
```

Time Complexity: O(nlogn)

Space Complexity: O(n)

## 8. Search a 2D Matrix II (medium)

[LeetCode 240](https://leetcode.com/problems/search-a-2d-matrix-ii/)

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return false;
    }
    int row = matrix.length - 1, col = 0;
    while (row >= 0 && col < matrix[0].length) {
        if (matrix[row][col] > target) {
            row--;
        } else if (matrix[row][col] < target) {
            col++;
        } else {
            return true;
        }
    }
    return false;
}
```

Time Complexity: O(m + n)

Space Complexity: O(1)