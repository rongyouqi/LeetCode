# Blind 75 Day 5 (Interval)

* `Arrays.sort()`in Java:
    * Time Complexity: O(nlogn)
    * Space Complexity: O(logn)

## 1. [LeetCode 57](https://leetcode.com/problems/insert-interval/) Insert Interval (medium)

- You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.
- Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).
- Return `intervals` _after the insertion_.
- **Example 1:**
    - **Input:** intervals = `[[1,3],[6,9]]`, newInterval = [2,5]
    - **Output:** `[[1,5],[6,9]]`
- **Example 2:**
    - **Input:** intervals = `[[1,2],[3,5],[6,7],[8,10],[12,16]]`, newInterval = [4,8]
    - **Output:** `[[1,2],[3,10],[12,16]]`
    - **Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
- **Constraints:**
    -   `0 <= intervals.length <= 10^4`
    -   `intervals[i].length == 2`
    -   `0 <= starti <= endi <= 10^5`
    -   `intervals` is sorted by `starti` in **ascending** order.
    -   `newInterval.length == 2`
    -   `0 <= start <= end <= 10^5`

### Solution

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

## 2. [LeetCode 56](https://leetcode.com/problems/merge-intervals/) Merge Intervals (medium)

- Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.
- **Example 1:**
    - **Input:** intervals = `[[1,3],[2,6],[8,10],[15,18]]`
    - **Output:** `[[1,6],[8,10],[15,18]]`
    - **Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
- **Example 2:**
    - **Input:** intervals = `[[1,4],[4,5]]`
    - **Output:** `[[1,5]]`
    - **Explanation:** Intervals [1,4] and [4,5] are considered overlapping.
- **Constraints:**
    -   `1 <= intervals.length <= 10^4`
    -   `intervals[i].length == 2`
    -   `0 <= starti <= endi <= 10^4`

### Solution

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

## 3. [LeetCode 435](https://leetcode.com/problems/non-overlapping-intervals/) Non-overlapping Intervals (medium)

- Given an array of intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping_.
- **Example 1:**
    - **Input:** intervals = `[[1,2],[2,3],[3,4],[1,3]]`
    - **Output:** 1
    - **Explanation:** [1,3] can be removed and the rest of the intervals are non-overlapping.
- **Example 2:**
    - **Input:** intervals = `[[1,2],[1,2],[1,2]]`
    - **Output:** 2
    - **Explanation:** You need to remove two [1,2] to make the rest of the intervals non-overlapping.
- **Example 3:**
    - **Input:** intervals = `[[1,2],[2,3]]`
    - **Output:** 0
    - **Explanation:** You don't need to remove any of the intervals since they're already non-overlapping.
- **Constraints:**
    -   `1 <= intervals.length <= 10^5`
    -   `intervals[i].length == 2`
    -   `-5 * 10^4 <= starti < endi <= 5 * 10^4`

### Solution

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

## 4. [LeetCode 252](https://leetcode.com/problems/meeting-rooms/) Meeting Rooms (easy)

- Given an array of meeting time `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.
- **Example 1:**
    - **Input:** intervals = `[[0,30],[5,10],[15,20]]`
    - **Output:** false
- **Example 2:**
    - **Input:** intervals = `[[7,10],[2,4]]`
    - **Output:** true
- **Constraints:**
    -   `0 <= intervals.length <= 10^4`
    -   `intervals[i].length == 2`
    -   `0 <= starti < endi <= 10^6`

### Solution 1

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

### Solution 2

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

## 5. [LeetCode 253](https://leetcode.com/problems/meeting-rooms-ii/) Meeting Rooms II (medium)

- Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of conference rooms required_.
- **Example 1:**
    - **Input:** intervals = `[[0,30],[5,10],[15,20]]`
    - **Output:** 2
- **Example 2:**
    - **Input:** intervals = `[[7,10],[2,4]]`
    - **Output:** 1
- **Constraints:**
    -   `1 <= intervals.length <= 10^4`
    -   `0 <= starti < endi <= 10^6`

### Solution

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