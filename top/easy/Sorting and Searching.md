# Easy Collection Day 3 (Sorting and Searching)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/96/sorting-and-searching/

## 1. Merge Sorted Array (easy)

[LeetCode 88](https://leetcode.com/problems/merge-sorted-array/)

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int tail1 = m - 1, tail2 = n - 1, finished = m + n - 1;
    while (tail1 >= 0 && tail2 >= 0) {
        nums1[finished--] = nums1[tail1] > nums2[tail2] ? nums1[tail1--] : nums2[tail2--];
    }
    while (tail2 >= 0) {
        nums1[finished--] = nums2[tail2--];
    }
}
```

Time Complexity: O(m + n)

Space Complexity: O(1)

## 2. First Bad Version (easy)

[LeetCode 278](https://leetcode.com/problems/first-bad-version/)

```java
public int firstBadVersion(int n) {
    int left = 1, right = n;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (isBadVersion(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)