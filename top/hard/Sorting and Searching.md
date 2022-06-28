# Hard Collection Day 5 (Sorting and Searching)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/120/sorting-and-searching/

## 1. [LeetCode 324](https://leetcode.com/problems/wiggle-sort-ii/) Wiggle Sort II (medium)

- Given an integer array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`.
- You may assume the input array always has a valid answer.
- **Example 1:**
    - **Input:** nums = [1,5,1,1,6,4]
    - **Output:** [1,6,1,5,1,4]
    - **Explanation:** [1,4,1,5,1,6] is also accepted.
- **Example 2:**
    - **Input:** nums = [1,3,2,2,3,1]
    - **Output:** [2,3,1,3,1,2]
- **Constraints:**
    -   `1 <= nums.length <= 5 * 10^4`
    -   `0 <= nums[i] <= 5000`
    -   It is guaranteed that there will be an answer for the given input `nums`.
- **Follow Up:** Can you do it in `O(n)` time and/or **in-place** with `O(1)` extra space?

### Solution 1: sort

```java
public void wiggleSort(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    int[] array = Arrays.copyOf(nums, nums.length);
    Arrays.sort(array);
    int index = nums.length - 1;
    for (int i = 1; i < nums.length; i += 2) {
        nums[i] = array[index--];
    }
    for (int i = 0; i < nums.length; i += 2) {
        nums[i] = array[index--];
    }
}
```

Time Complexity: (nlogn)

Space Complexity: O(n)

### Solution 2: index mapping

```java
public void wiggleSort(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    int len = nums.length;
    int median = kthSmallestNumber(nums, (len + 1) / 2);
    for (int i = 0, j = 0, k = len - 1; j <= k;) {
        if (nums[indexMapping(j, len)] > median) {
            swap(nums, indexMapping(i++, len), indexMapping(j++, len));
        } else if (nums[indexMapping(j, len)] < median) {
            swap(nums, indexMapping(j, len), indexMapping(k--, len));
        } else {
            j++;
        }
    }
}

private int kthSmallestNumber(int[] nums, int k) {
    Random rand = new Random();
    for (int i = nums.length - 1; i >= 0; i--) {
        swap(nums, i, rand.nextInt(i + 1));
    }
    int left = 0, right = nums.length - 1;
    k--;
    while (left < right) {
        int mid = getMiddle(nums, left, right);
        if (mid < k) {
            left = mid + 1;
        } else if (mid > k) {
            right = mid - 1;
        } else {
            break;
        }
    }
    return nums[k];
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}

private int getMiddle(int[] nums, int left, int right) {
    int i = left;
    for (int j = left + 1; j <= right; j++) {
        if (nums[j] < nums[left]) {
            swap(nums, ++i, j);
        }
    }
    swap(nums, left, i);
    return i;
}

private int indexMapping(int index, int len) {
    return (2 * index + 1) % (len | 1);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 3

```java
public void wiggleSort(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    int len = nums.length;
    int median = quickselect(nums, 0, len - 1, (len + 1) / 2);
	int largePos = 1;
    int smallPos = len % 2 == 0 ? len - 2 : len - 1;
    int i = 0;
    while (i < len) {
        if (nums[i] < median && (i < smallPos || i >= smallPos && i % 2 != 0)) {
            swap(nums, i, smallPos);
            smallPos -= 2;
        } else if (nums[i] > median && (i > largePos || i <= largePos && i % 2 == 0)) {
            swap(nums, i, largePos);
            largePos += 2;
        } else {
            i++;
        }
    }
}

private int quickselect(int[] nums, int start, int end, int k) {
    if (start == end) {
        return nums[start];
    }
    int left = start, right = end, key = nums[right];
    while (left < right) {
        while (left < right && nums[left] <= key) {
            left++;
        }
        if (left < right) {
            nums[right--] = nums[left];
        }
        while (left < right && nums[right] >= key) {
            right--;
        }
        if (left < right) {
            nums[left++] = nums[right];
        }
    }
    nums[left] = key;
    if (k < left) {
        return quickselect(nums, start, left - 1, k);
    }
    if (k > right) {
        return quickselect(nums, right + 1, end, k);
    }
    return nums[left];
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. [LeetCode 378](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) Kth Smallest Element in a Sorted Matrix (medium)

- Given an `n x n` `matrix` where each of the rows and columns is sorted in ascending order, return _the_ `kth` _smallest element in the matrix_.
- Note that it is the `kth` smallest element **in the sorted order**, not the `kth` **distinct** element.
- You must find a solution with a memory complexity better than `O(n2)`.
- **Example 1:**
    - **Input:** matrix = `[[1,5,9],[10,11,13],[12,13,15]]`, k = 8
    - **Output:** 13
    - **Explanation:** The elements in the matrix are [1,5,9,10,11,12,13,**13**,15], and the 8th smallest number is 13
- **Example 2:**
    - **Input:** matrix = `[[-5]]`, k = 1
    - **Output:** -5
- **Constraints:**
    -   `n == matrix.length == matrix[i].length`
    -   `1 <= n <= 300`
    -   `-10^9 <= matrix[i][j] <= 10^9`
    -   All the rows and columns of `matrix` are **guaranteed** to be sorted in **non-decreasing order**.
    -   `1 <= k <= n^2`
- **Follow up:**
    -   Could you solve the problem with a constant memory (i.e., `O(1)` memory complexity)?
    -   Could you solve the problem in `O(n)` time complexity? The solution may be too advanced for an interview but you may find reading [this paper](http://www.cse.yorku.ca/~andy/pubs/X+Y.pdf) fun.

### Solution 1: best first search (bfs2)

* Data structure: priority queue
* Initial state: `input[0][0]`
* Expand `node[i][j]`
    * Generate `node[i + 1][j]`
    * Generate `node[i][j + 1]`
* Termination condition: when the k-th element is popped out for expansion
* De-duplicate: which nodes have been generated (not expanded)
    * `boolean[][] generated;`

```java
static class Cell {
    int value;
    int row;
    int column;

    public Cell(int row, int column, int value) {
		this.value = value;
		this.row = row;
		this.column = column;
    }
}

public int kthSmallest(int[][] matrix, int k) {
    int rows = matrix.length;
    int columns = matrix[0].length;
    boolean[][] visited = new boolean[rows][columns];
    PriorityQueue<Cell> minHeap = new PriorityQueue<Cell>(k, new Comparator<Cell>() {
        @Override
        public int compare(Cell c1, Cell c2) {
            if (c1.value == c2.value) {
                return 0;
            }
            return c1.value < c2.value ? -1 : 1;
        }
    });
    minHeap.offer(new Cell(0, 0, matrix[0][0]));
    visited[0][0] = true;
    for (int i = 0; i < k - 1; i++) {
        Cell current = minHeap.poll();
        if (current.row + 1 < rows && !visited[current.row + 1][current.column]) {
            minHeap.offer(new Cell(current.row + 1, current.column, matrix[current.row + 1][current.column]));
            visited[current.row + 1][current.column] = true;
        }
        if (current.column + 1 < columns && !visited[current.row][current.column + 1]) {
            minHeap.offer(new Cell(current.row, current.column + 1, matrix[current.row][current.column + 1]));
            visited[current.row][current.column + 1] = true;
        }
    }
    return minHeap.peek().value;
}
```

Time Complexity: O(klogk)

Space Complexity: O(k + n^2)

### Solution 2: binary search

```java
public int kthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    int left = matrix[0][0], right = matrix[n - 1][n - 1];
    while (left < right) {
        int mid = left + (right - left) / 2;
        int[] array = new int[]{matrix[0][0], matrix[n - 1][n - 1]};
        int count = helper(matrix, mid, array);
        if (count > k) {
            right = array[0];
        } else if (count < k) {
            left = array[1];
        } else {
            return array[0];
        }
    }
    return left;
}

private int helper(int[][] matrix, int mid, int[] array) {
    int count = 0, n = matrix.length;
    int row = n - 1, col = 0;
    while (row >= 0 && col < n) {
        if (matrix[row][col] > mid) {
            array[1] = Math.min(array[1], matrix[row][col]);
            row--;
        } else {
            array[0] = Math.max(array[0], matrix[row][col]);
            count += row + 1;
            col++;
        }
    }
    return count;
}
```

Time Complexity: O(n * log(max - min))

Space Compleexity: O(1)

## 3. [LeetCode 4](https://leetcode.com/problems/median-of-two-sorted-arrays/) Median of Two Sorted Arrays (hard)

- Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.
- The overall run time complexity should be `O(log (m+n))`.
- **Example 1:**
    - **Input:** nums1 = [1,3], nums2 = [2]
    - **Output:** 2.00000
    - **Explanation:** merged array = [1,2,3] and median is 2.
- **Example 2:**
    - **Input:** nums1 = [1,2], nums2 = [3,4]
    - **Output:** 2.50000
    - **Explanation:** merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
- **Constraints:**
    -   `nums1.length == m`
    -   `nums2.length == n`
    -   `0 <= m <= 1000`
    -   `0 <= n <= 1000`
    -   `1 <= m + n <= 2000`
    -   `-10^6 <= nums1[i], nums2[i] <= 10^6`

### Solution: binary search

```java
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if (nums1 == null) {
        return findMedianSortedArrays(new int[0], nums2);
    }
    if (nums2 == null) {
        return findMedianSortedArrays(nums1, new int[0]);
    }
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }
    int m = nums1.length, n = nums2.length;
    int k = (m + n) / 2;
    int left = 0, right = m - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums1[mid] < nums2[k - mid - 1]) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    double candidate1 = Math.min(left == m ? Integer.MAX_VALUE : nums1[left], k - left == n ? Integer.MAX_VALUE : nums2[k - left]);
    if ((m + n) % 2 == 1) {
        return candidate1;
    }
    double candidate2 = Math.max(left == 0 ? Integer.MIN_VALUE : nums1[left - 1], k - left == 0 ? Integer.MIN_VALUE : nums2[k - left - 1]);
    return (candidate1 + candidate2) / 2;
}
```

Time Complexity: O(log(min(m, n)))

Space Complexity: O(1)
