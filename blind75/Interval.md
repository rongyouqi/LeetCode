# Blind 75 Day 5 (Interval)

* `Arrays.sort()`in Java:

    * Time Complexity: O(nlogn)

    * Space Complexity: O(logn)

## 1. Insert Interval (medium)

[LeetCode 57](https://leetcode.com/problems/insert-interval/)

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    if (intervals == null || newInterval == null || newInterval.length == 0) {
        return intervals;
    }
    List<int[]> result = new ArrayList<>();
    int i = 0;
    while (i < intervals.length && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.add(newInterval);
    while (i < intervals.length) {
        result.add(intervals[i]);
        i++;
    }
    return result.toArray(new int[result.size()][]);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Merge Intervals (medium)

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

## 3. Non-overlapping Intervals (medium)

[LeetCode 435](https://leetcode.com/problems/non-overlapping-intervals/)

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return 0;
    }
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    int result = 0, end = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (end > intervals[i][0]) {
            end = Math.min(end, intervals[i][1]);
            result++;
        } else {
            end = intervals[i][1];
        }
    }
    return result;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

## 4. Meeting Rooms (easy)

[LeetCode 252](https://leetcode.com/problems/meeting-rooms/)

### Solution 1:

```java
public boolean canAttendMeetings(int[][] intervals) {
    if (intervals == null || intervals.length == 0) {
        return true;
    }
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    for (int i = 0; i < intervals.length - 1; i++) {
        if (intervals[i][1] > intervals[i + 1][0]) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

### Solution 2:

```java
public boolean canAttendMeetings(int[][] intervals) {
    // assumption: start < end
    if (intervals == null || intervals.length == 0) {
        return true;
    }
    try {
        Arrays.sort(intervals, (a, b) -> {
            if (a[1] <= b[0]) { // a[0] < a[1] <= b[0]
                return -1;
            } else if (a[0] >= b[1]) { // b[0] < b[1] <= a[0]
                return 1;
            }
            throw new RuntimeException("overlap detected");
        });
        return true;
    } catch (RuntimeException e) {
        return false;
    }
}
```

Time Complexity: O(logn)

Space Complexity: O(n)

## 5. Meeting Rooms II (medium)

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