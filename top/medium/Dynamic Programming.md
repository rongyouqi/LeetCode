# Medium Collection Day 3 (Dynamic Programming)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/111/dynamic-programming/

## 1. Jump Game (medium)

[LeetCode 55](https://leetcode.com/problems/jump-game/)

```java
public boolean canJump(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }
    if (nums.length == 1) {
        return true;
    }
    int lastPosition = nums.length - 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        if (i + nums[i] >= lastPosition) {
            lastPosition = i;
        }
    }
    return lastPosition == 0;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Unique Paths (medium)

[LeetCode 62](https://leetcode.com/problems/unique-paths/)

```java
public int uniquePaths(int m, int n) {
    // assumption: m > 0, n > 0
    int[][] M = new int[m][n];
    for (int[] row : M) {
        Arrays.fill(row, 1);
    }
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            M[i][j] = M[i - 1][j] + M[i][j - 1];
        }
    }
    return M[m - 1][n - 1];
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

## 3. Coin Change (medium)

[LeetCode 322](https://leetcode.com/problems/coin-change/)

```java
public int coinChange(int[] coins, int amount) {
    if (coins == null || coins.length == 0 || amount < 0) {
        return -1;
    }
    int[] M = new int[amount + 1];
    Arrays.fill(M, amount + 1);
    M[0] = 0;
    for (int i = 0; i <= amount; i++) {
        for (int j = 0; j < coins.length; j++) {
            if (coins[j] <= i) {
                M[i] = Math.min(M[i], M[i - coins[j]] + 1);
            }
        }
    }
    return M[amount] > amount ? -1 : M[amount];
}
```

Time Complexity: O(mn)

Space Complexity: O(n)

## 4. Longest Increasing Subsequence (medium)

[LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/)

```java
public int lengthOfLIS(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int[] M = new int[nums.length];
    Arrays.fill(M, 1);
    int max = 1;
    for (int i = 1; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                M[i] = Math.max(M[i], M[j] + 1);
                max = Math.max(max, M[i]);
            }
        }
    }
    return max;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(n)