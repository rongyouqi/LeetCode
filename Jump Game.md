# Jump Game

## 1. [LeetCode 55](https://leetcode.com/problems/jump-game/) Jump Game (medium)

- You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.
- Return `true` _if you can reach the last index, or_ `false` _otherwise_.
- **Example 1:**
    - **Input:** nums = [2,3,1,1,4]
    - **Output:** true
    - **Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.
- **Example 2:**
    - **Input:** nums = [3,2,1,0,4]
    - **Output:** false
    - **Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `0 <= nums[i] <= 10^5`

### Solution 1: dynamic programming

```java
public boolean canJump(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }
    if (nums.length == 1) {
        return true;
    }
    boolean[] dp = new boolean[nums.length];
    for (int i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= nums.length - 1) {
            dp[i] = true;
        } else {
            for (int j = nums[i]; j > 0; j--) {
                if (dp[i + j]) {
                    dp[i] = true;
                    break;
                }
            }
        }
    }
    return dp[0];
}
```

Time Complexity: O(n^2)

Space Complexity: O(n)

### Solution 2: greedy

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

## 2. [LeetCode 45](https://leetcode.com/problems/jump-game-ii/) Jump Game II (medium)

- You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.
- Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:
    -   `0 <= j <= nums[i]` and
    -   `i + j < n`
- Return _the minimum number of jumps to reach_ `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.
- **Example 1:**
    - **Input:** nums = [2,3,1,1,4]
    - **Output:** 2
    - **Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
- **Example 2:**
    - **Input:** nums = [2,3,0,1,4]
    - **Output:** 2
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `0 <= nums[i] <= 1000`

### Solution: greedy

```java
public int jump(int[] nums) {
	int result = 0, currentJumpEnd = 0, farthest = 0;
    for (int i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);
        if (i == currentJumpEnd) {
            result++;
            currentJumpEnd = farthest;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)