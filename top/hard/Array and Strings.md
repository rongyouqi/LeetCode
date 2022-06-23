# Hard Collection Day 1 (Array and Strings)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/116/array-and-strings/

## 1. Product of Array Except Self (medium)

[LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/)

```java
public int[] productExceptSelf(int[] nums) {
    if (nums == null || nums.length == 0) {
        throw new IllegalArgumentException("illegal input array");
    }
    int[] result = new int[nums.length];
    result[0] = 1;
    for (int i = 1; i < nums.length; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    int right = 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        result[i] = result[i] * right;
        right *= nums[i];
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Spiral Matrix (medium)

[LeetCode 54](https://leetcode.com/problems/spiral-matrix/)

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix == null || matrix.length == 0) {
        return result;
    }
    int m = matrix.length;
    int n = matrix[0].length;
    int left = 0, right = n - 1;
    int up = 0, down = m - 1;
    while (left < right && up < down) {
        for (int i = left; i <= right; i++) {
            result.add(matrix[up][i]);
        }
        for (int i = up + 1; i <= down - 1; i++) {
            result.add(matrix[i][right]);
        }
        for (int i = right; i >= left; i--) {
            result.add(matrix[down][i]);
        }
        for (int i = down - 1; i >= up + 1; i--) {
            result.add(matrix[i][left]);
        }
        left++;
        right--;
        up++;
        down--;
    }
    if (left == right) {
        for (int i = up; i <= down; i++) {
            result.add(matrix[i][left]);
        }
    } else if (up == down) {
        for (int i = left; i <= right; i++) {
            result.add(matrix[up][i]);
        }
    }
    return result;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 3. 4Sum II (medium)

[LeetCode 454](https://leetcode.com/problems/4sum-ii/)

```java
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int num1 : nums1) {
        for (int num2 : nums2) {
            map.put(num1 + num2, map.getOrDefault(num1 + num2, 0) + 1);
        }
    }
    int result = 0;
    for (int num3 : nums3) {
        for (int num4 : nums4) {
            result += map.getOrDefault(-(num3 + num4), 0);
        }
    }
    return result;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(n<sup>2</sup>)

## 4. Container With Most Water (medium)

[LeetCode 11](https://leetcode.com/problems/container-with-most-water/)

```java
public int maxArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }
    int result = 0;
    int left = 0, right = height.length - 1;
    while (left < right) {
        result = Math.max(result, (right - left) * Math.min(height[left], height[right]));
        if (height[left] > height[right]) {
            right--;
        } else {
            left++;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Game of Life (medium)

[LeetCode 289](https://leetcode.com/problems/game-of-life/)

```java
public void gameOfLife(int[][] board) {
    int[] dirs = new int[]{-1, 0, 1};
    for (int row = 0; row < board.length; row++) {
        for (int col = 0; col < board[0].length; col++) {
            int live = 0;
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (i == 1 && j == 1) {
                        continue;
                    }
                    int r = row + dirs[i];
                    int c = col + dirs[j];
                    if ((r >= 0 && r < board.length) && (c >= 0 && c < board[0].length) && Math.abs(board[r][c]) == 1) {
                        live += 1;
                    }
                }
            }
            if (board[row][col] == 1 && (live < 2 || live > 3)) {
                board[row][col] = -1;
            }
            if (board[row][col] == 0 && live == 3) {
                board[row][col] = 2;
            }
        }
    }
    for (int row = 0; row < board.length; row++) {
        for (int col = 0; col < board[0].length; col++) {
            if (board[row][col] > 0) {
                board[row][col] = 1;
            } else {
                board[row][col] = 0;
            }
        }
    }
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 6. First Missing Positive (hard)

[LeetCode 41](https://leetcode.com/problems/first-missing-positive/)

```java
public int firstMissingPositive(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] > 0 && nums[i] <= nums.length && nums[nums[i] - 1] != nums[i]) {
            swap(nums, i, nums[i] - 1);
        } else {
            i++;
        }
    }
    for (int j = 0; j < nums.length; j++) {
        if (nums[j] != j + 1) {
            return j + 1;
        }
    }
    return nums.length + 1;
}

private void swap(int[] array, int x, int y) {
    int temp = array[x];
    array[x] = array[y];
    array[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 7. Longest Consecutive Sequence (medium)

[LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/)

```java
public int longestConsecutive(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        set.add(num);
    }
    int result = 0;
    for (int num : set) {
        if (!set.contains(num - 1)) {
            int current = num, temp = 1;
            while (set.contains(current + 1)) {
                temp++;
                current++;
            }
            result = Math.max(result, temp);
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8. Find the Duplicate Number (medium)

[LeetCode 287](https://leetcode.com/problems/find-the-duplicate-number/)

### Solution 1

```java
public int findDuplicate(int[] nums) {
    int result = -1;
    for (int i = 0; i < nums.length; i++) {
        int current = Math.abs(nums[i]);
        if (nums[current] < 0) {
            result = current;
            break;
        }
        nums[current] *= -1;
    }
    for (int i = 0; i < nums.length; i++) {
        nums[i] = Math.abs(nums[i]);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 2: Floyd's cycle detection algorithm (tortoise/slow and hare/fast pointers)

https://www.youtube.com/watch?v=PvrxZaH_eZ4

```java
public int findDuplicate(int[] nums) {
    int slow = 0, fast = 0;
    while (true) {
        slow = nums[slow];
        fast = nums[nums[fast]];
        if (slow == fast) {
            break;
        }
    }
    slow = 0;
    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }
    return slow;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 9. Longest Substring with At Most K Distinct Characters (medium)

[LeetCode 340](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

```java
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    if (s == null || s.length() * k == 0) {
        return 0;
    }
    // assumption: s only consists of ASCII characters
    int[] array = new int[256];
    int left = 0, result = 0, count = 0;
    for (int right = 0; right < s.length(); right++) {
        if (++array[s.charAt(right)] == 1) {
            count++;
        }
        while (count > k) {
            if (--array[s.charAt(left++)] == 0) {
                count--;
            }
        }
        result = Math.max(result, right - left + 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 10. Basic Calculator II (medium)

[LeetCode 227](https://leetcode.com/problems/basic-calculator-ii/)

```java
public int calculate(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int result = 0, num = 0, temp = 0;
    char operation = '+';
    for (char c : s.toCharArray()) {
        if (Character.isDigit(c)) {
            temp = temp * 10 + (c - '0');
        } else if (c != ' ') {
            num = helper(num, temp, operation);
            if (c == '+' || c == '-') {
                result += num;
                num = 0;
            }
            temp = 0;
            operation = c;
        }
    }
    return result + helper(num, temp, operation);
}

private int helper(int num, int temp, char operation) {
    if (operation == '+') {
        return num + temp;
    } else if (operation == '-') {
        return num - temp;
    } else if (operation == '*') {
        return num * temp;
    }
    return num / temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 11. Sliding Window Maximum (hard)

[LeetCode 239](https://leetcode.com/problems/sliding-window-maximum/)

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums == null || nums.length * k == 0) {
        return new int[0];
    }
    int[] result = new int[nums.length - k + 1];
    Deque<Integer> deque = new ArrayDeque<>();
    for (int i = 0; i < nums.length; i++) {
        while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {
            deque.pollLast();
        }
        if (!deque.isEmpty() && deque.peekFirst() <= i - k) {
            deque.pollFirst();
        }
        deque.offerLast(i);
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(k)

## 12. Minimum Window Substring (hard)

[LeetCode 76](https://leetcode.com/problems/minimum-window-substring/)

```java
public String minWindow(String s, String t) {
    if (s == null || t == null) {
        return null;
    }
    if (s.length() == 0 || t.length() == 0) {
        return "";
    }
    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < t.length(); i++) {
        map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
    }
    int[] result = {-1, 0, 0};
    int left = 0, right = 0, count = 0;
    Map<Character, Integer> window = new HashMap<>();
    while (right < s.length()) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);
        if (map.containsKey(c) && map.get(c).equals(window.get(c))) {
            count++;
        }
        while (left <= right && count == map.size()) {
            c = s.charAt(left);
            if (result[0] == -1 || right - left + 1 < result[0]) {
                result[0] = right - left + 1;
                result[1] = left;
                result[2] = right;
            }
            window.put(c, window.get(c) - 1);
            if (map.containsKey(c) && window.get(c).intValue() < map.get(c).intValue()) {
                count--;
            }
            left++;
        }
        right++;
    }
    return result[0] == -1 ? "" : s.substring(result[1], result[2] + 1);
}
```

Time Complexity: O(m + n)

Space Complexity: O(m + n)