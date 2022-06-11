# Easy Collection Day 1 (Array)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/

## 1. Remove Duplicates from Sorted Array (easy)

[LeetCode 26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

```java
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int left = 0;
    for (int right = 1; right < nums.length; right++) {
        if (nums[right] != nums[left]) {
            left++;
            nums[left] = nums[right];
        }
    }
    return left + 1;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Best Time to Buy and Sell Stock II (medium)

[LeetCode 122](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/solution/)

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
        return 0;
    }
    int result = 0;
    for (int i = 0; i < prices.length - 1; i++) {
        if (prices[i] < prices[i + 1]) {
            result += prices[i + 1] - prices[i];
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. Rotate Array (medium)

[LeetCode 189](https://leetcode.com/problems/rotate-array/)

```java
public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

private void reverse(int[] nums, int left, int right) {
    while (left < right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
        left++;
        right--;
    }
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. Contains Duplicate (easy)

[LeetCode 217](https://leetcode.com/problems/contains-duplicate/)

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        if (set.contains(num)) {
            return false;
        }
        set.add(num);
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 5. Single Number (easy)

[LeetCode 136](https://leetcode.com/problems/single-number/)

### Solution 1: hashmap

```java
public int singleNumber(int[] nums) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    for (int num : nums) {
        if (map.get(num) == 1) {
            return num;
        }
    }
    return 0;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: bit manipulation

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. Intersection of Two Arrays II (easy)

[LeetCode 350](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

### Solution 1: hashmap

```java
public int[] intersect(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) {
        return intersect(nums2, nums1);
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int num : nums1) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    int i = 0;
    for (int num : nums2) {
        int count = map.getOrDefault(num, 0);
        if (count > 0) {
            nums1[i++] = num;
            map.put(num, count - 1);
        }
    }
    return Arrays.copyOfRange(nums1, 0, i);
}
```

Time Complexity: O(m + n)

Space Complexity: O(min(m, n))

### Solution 2: sort

```java
public int[] intersect(int[] nums1, int[] nums2) {
    Arrays.sort(nums1);
    Arrays.sort(nums2);
    int i = 0, j = 0, k = 0;
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] < nums2[j]) {
            i++;
        } else if (nums1[i] > nums2[j]) {
            j++;
        } else {
            nums1[k++] = nums1[i++];
            j++;
        }
    }
    return Arrays.copyOfRange(nums1, 0, k);
}
```

Time Complexity: O(nlogn + mlogm)

Space Complexity: (logn + logm)

## 7. Plus One (easy)

[LeetCode 66](https://leetcode.com/problems/plus-one/)

```java
public int[] plusOne(int[] digits) {
    for (int i = digits.length - 1; i >= 0; i--) {
        if (digits[i] == 9) {
            digits[i] = 0;
        } else {
            digits[i]++;
            return digits;
        }
    }
    int[] result = new int[digits.length + 1];
    result[0] = 1;
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 8. Move Zeroes (easy)

[LeetCode 283](https://leetcode.com/problems/move-zeroes/)

```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    for (int i = 0, j = 0; j < nums.length; j++) {
        if (nums[j] != 0) {
            swap(nums, i++, j);
        }
    }
}

private void swap(int[] nums, int x, int y) {
    int temp = nums[x];
    nums[x] = nums[y];
    nums[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 9. Two Sum (easy)

[LeetCode 1](https://leetcode.com/problems/two-sum/)

### Solution 1: brute force

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return null;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

### Solution 2: one pass hashmap

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return null;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 10. Valid Sudoku (medium)

[LeetCode 36](https://leetcode.com/problems/valid-sudoku/)

### Solution 1: hashset

```java
public boolean isValidSudoku(char[][] board) {
    int N = 9;
    HashSet<Character>[] rows = new HashSet[N];
    HashSet<Character>[] cols = new HashSet[N];
    HashSet<Character>[] boxes = new HashSet[N];
    for (int i = 0; i < N; i++) {
        rows[i] = new HashSet<Character>();
        cols[i] = new HashSet<Character>();
        boxes[i] = new HashSet<Character>();
    }
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            char c = board[row][col];
            if (c == '.') {
                continue;
            }
            if (rows[row].contains(c)) {
                return false;
            }
            rows[row].add(c);
            if (cols[col].contains(c)) {
                return false;
            }
            cols[col].add(c);
            int index = (row / 3) * 3 + col / 3;
            if (boxes[index].contains(c)) {
                return false;
            }
            boxes[index].add(c);
        }
    }
    return true;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(n<sup>2</sup>)

### Solution 2: array

```java
public boolean isValidSudoku(char[][] board) {
    int N = 9;
    int[][] rows = new int[N][N];
    int[][] cols = new int[N][N];
    int[][] boxes = new int[N][N];
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            if (board[row][col] == '.') {
                continue;
            }
            int position = board[row][col] - '1';
            if (rows[row][position] == 1) {
                return false;
            }
            rows[row][position] = 1;
            if (cols[col][position] == 1) {
                return false;
            }
            cols[col][position] = 1;
            int index = (row / 3) * 3 + col / 3;
            if (boxes[index][position] == 1) {
                return false;
            }
            boxes[index][position] = 1;
        }
    }
    return true;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(n<sup>2</sup>)

### Solution 3: bitmasking

```java
public boolean isValidSudoku(char[][] board) {
    int N = 9;
    int[] rows = new int[N];
    int[] cols = new int[N];
    int[] boxes = new int[N];
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            if (board[row][col] == '.') {
                continue;
            }
            int k = board[row][col] - '1';
            int pos = 1 << k;
            if ((rows[row] & pos) > 0) {
                return false;
            }
            rows[row] |= pos;
            if ((cols[col] & pos) > 0) {
                return false;
            }
            cols[col] |= pos;
            int index = (row / 3) * 3 + col / 3;
            if ((boxes[index] & pos) > 0) {
                return false;
            }
            boxes[index] |= pos;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 11. Rotate Image (medium)

[LeetCode 48](https://leetcode.com/problems/rotate-image/)

```java
public void rotate(int[][] matrix) {
    if (matrix == null || matrix.length == 0) {
        return;
    }
    for (int i = 0; i < matrix.length / 2; i++) {
        int[] temp = matrix[i];
        matrix[i] = matrix[matrix.length - 1 - i];
        matrix[matrix.length - 1 - i] = temp;
    }
    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < i; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    return;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)
