# Blind 75 Day 3 (Dynamic Programming)

**表象上填表格, 实质上用空间换取时间**

1. base case: M[0]

2. induction rule

   * 英文物理意义: M[i] represents what

   * 数学表达式: relationship between M[i] and M[i - 1], etc.

## 1. Climbing Stairs (easy)

[LeetCode 70](https://leetcode.com/problems/climbing-stairs/)

1. base case
   * M[0] = 0
   * M[1] = 1
   * M[2] = 2
2. induction rule
   * M[i] represents how many distinct ways to climb to step i
   * M[i] = M[i - 1] + M[i - 2]

```java
public int climbStairs(int n) {
  // assumption: n >= 0
  if (n == 0 || n == 1 || n == 2) {
    return n;
  }
  int[] M = new int[n + 1];
  M[1] = 1;
  M[2] = 2;
  for (int i = 3; i <= n; i++) {
    M[i] = M[i - 1] + M[i - 2];
  }
  return M[n];
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution: space optimized

```java
public int climbStairs(int n) {
  // assumption: n >= 0
  if (n == 0 || n == 1 || n == 2) {
    return n;
  }
  int twoStep = 1;
  int oneStep = 2;
  int result = oneStep;
  for (int i = 3; i <= n; i++) {
    result = oneStep + twoStep;
    twoStep = oneStep;
    oneStep = result;
  }
  return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Coin Change (medium)

[LeetCode 322](https://leetcode.com/problems/coin-change/)

1. base case: M[0] = 0
2. induction rule
   * M[i] represents the fewest number of coins to make up i
   * M[i] = Math.min(M[i], M[i - j] + 1), j is coin denomination

```java
public int coinChange(int[] coins, int amount) {
  // assumption: amount << Integer.MAX_VALUE
  if (coins == null || coins.length == 0 || amount < 0) {
    return -1;
  }
  int[] M = new int[amount + 1];
  Arrays.fill(M, amount + 1); // Integer.MAX_VALUE ??? why wrong
  M[0] = 0;
  for (int i = 1; i <= amount; i++) {
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

## 3. Longest Increasing Subsequence (medium)

[LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/)

### Solution 1: dynamic programming

| nums |  10  |  9   |  2   |  5   |  3   |  7   | 101  |  18  |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  M   |  1   |  1   |  1   |  2   |  2   |  3   |  4   |  4   |

1. base case: M[0] = 1
2. induction rule
   * M[i] represents the length of the longest strictly increasing subsequence stopping at index i
   * M[i] = Math.max(M[i], M[j] + 1), nums[j] < nums[i], 0 <= j < i

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

### Solution 2: binary search

```java
public int lengthOfLIS(int[] nums) {
  // ??? why
  List<Integer> subsequence = new ArrayList<>();
  subsequence.add(nums[0]);
  for (int i = 1; i < nums.length; i++) {
    if (nums[i] > subsequence.get(subsequence.size() - 1)) {
      subsequence.add(nums[i]);
    } else {
      int j = binarySearch(subsequence, nums[i]);
      subsequence.set(j, nums[i]);
    }
  }
  return subsequence.size();
}

private int binarySearch(List<Integer> list, int target) {
  int left = 0, right = list.size() - 1;
  while (left < right) {
    int mid = left + (right - left) / 2;
    if (list.get(mid) == target) {
      return mid;
    } else if (list.get(mid) < target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }
  return left;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(n)

## 4. Longest Common Subsequence (medium)

[LeetCode 1143](https://leetcode.com/problems/longest-common-subsequence/)

|      |  _   |  a   |  b   |  c   |  d   |  e   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  _   |  0   |  0   |  0   |  0   |  0   |  0   |
|  a   |  0   |  1   |  1   |  1   |  1   |  1   |
|  c   |  0   |  1   |  1   |  2   |  2   |  2   |
|  e   |  0   |  1   |  1   |  2   |  2   |  3   |

1. base case: `M[i][0] = m[0][j] = 0`
2. induction rule:
   * `M[i][j]` represents the length of longest common subsequence stopping at text1[i] and text2[j]
   * `M[i][j] = M[i - 1][j - 1] + 1` if text1[i] == text2[j]
   * `M[i][j] = Math.max(M[i - 1][j], M[i][j - 1])` otherwise

```java
public int longestCommonSubsequence(String text1, String text2) {
  if (text1 == null || text1.length() == 0 || text2 == null || text2.length() == 0) {
    return 0;
  }
  int[][] M = new int[text1.length() + 1][text2.length() + 1];
  for (int i = 1; i <= text1.length(); i++) {
    for (int j = 1; j <= text2.length(); j++) {
      if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
        M[i][j] = M[i - 1][j - 1] + 1;
      } else {
        M[i][j] = Math.max(M[i - 1][j], M[i][j - 1]);
      }
    }
  }
  return M[text1.length()][text2.length()];
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

## 5. Word Break Problem (medium)

[LeetCode 139](https://leetcode.com/problems/word-break/)

s = "leetcode", wordDict = ["leet", "code"]

|  s   |      |   l   |   e   |   e   |  t   |   c   |   o   |   d   |  e   |
| :--: | :--: | :---: | :---: | :---: | :--: | :---: | :---: | :---: | :--: |
|  M   | true | false | false | false | true | false | false | false | true |

1. base case: M[0] = true
2. induction rule
   * M[i] represents whether we can partition the first i letters of the input into words
   * M[i] = OR{M[j] for all 0 <= j < i and input[j..i) is a word}

```java
public boolean wordBreak(String s, List<String> wordDict) {
  if (s == null || s.length() == 0 || wordDict == null || wordDict.size() == 0) {
    return false;
  }
  Set<String> set = new HashSet<>();
  for (String word : wordDict) {
    set.add(word);
  }
  boolean[] M = new boolean[s.length() + 1];
  M[0] = true;
  for (int i = 1; i <= s.length(); i++) {
    for (int j = 0; j < i; j++) {
      if (M[j] && set.contains(s.substring(j, i))) {
        M[i] = true;
        break;
      }
    }
  }
  return M[s.length()];
}
```

Time Complexity: O(n<sup>3</sup>) // string API is very slow

Space Complexity: O(n)

## 6. Combination Sum (medium)

[LeetCode 377](https://leetcode.com/problems/combination-sum-iv/)

nums = [1, 2, 3], target = 4

| target |  0   |  1   |  2   |  3   |  4   |
| :----: | :--: | :--: | :--: | :--: | :--: |
|   M    |  1   |  1   |  2   |  4   |  7   |

1. base case: M[0] = 1
2. induction rule
   * M[i] represents the number of possible combinations to add up to i
   * M[i] = sum(M[i - nums[j])

```java
public int combinationSum4(int[] nums, int target) {
  if (nums == null || nums.length == 0) {
    return 0;
  }
  int[] M = new int[target + 1];
  M[0] = 1;
  for (int i = 1; i <= target; i++) {
    for (int num : nums) {
      if (num <= i) {
        M[i] += M[i - num];
      }
    }
  }
  return M[target];
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 7. House Robber (medium)

[LeetCode 198](https://leetcode.com/problems/house-robber/)

1. base case
   * M[0] = nums[0]
   * M[1] = Math.max(nums[0], nums[1])
2. induction rule
   * M[i] represents the maximum amount of money you can rob stopping at index i
   * M[i] = Math.max(M[i - 2] + nums[i], M[i - 1])

```java
public int rob(int[] nums) {
  if (nums == null || nums.length == 0) {
    return 0;
  }
  if (nums.length == 1) {
    return nums[0];
  }
  int[] M = new int[nums.length];
  M[0] = nums[0];
  M[1] = Math.max(nums[0], nums[1]);
  for (int i = 2; i < nums.length; i++) {
    M[i] = Math.max(M[i - 2] + nums[i], M[i - 1]);
  }
  return M[nums.length - 1];
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution: space optimized

```java
public int rob(int[] nums) {
  if (nums == null || nums.length == 0) {
    return 0;
  }
  if (nums.length == 1) {
    return nums[0];
  }
  int twoStep = nums[0];
  int oneStep = Math.max(nums[0], nums[1]);
  int result = oneStep;
  for (int i = 2; i < nums.length; i++) {
    result = Math.max(twoStep + nums[i], oneStep);
    twoStep = oneStep;
    oneStep = result;
  }
  return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 8. House Robber II (medium)

[LeetCode 213](https://leetcode.com/problems/house-robber-ii/)

two subproblems:

1. rob 0 to nums.length - 2
2. rob 1 to nums.length - 1

```java
public int rob(int[] nums) {
  if (nums == null || nums.length == 0) {
    return 0;
  }
  if (nums.length == 1) {
    return nums[0];
  }
  return Math.max(rob(nums, 0, nums.length - 2), rob(nums, 1, nums.length -1));
}

private int rob(int[] nums, int left, int right) {
  int include = 0, exclude = 0;
  for (int i = left; i <= right; i++) {
    int oneStep = include, twoStep = exclude;
    include = twoStep + nums[i];
    exclude = Math.max(oneStep, twoStep);
  }
  return Math.max(include, exclude);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 9. Decode Ways (medium)

[LeetCode 91](https://leetcode.com/problems/decode-ways/)

```java
public int numDecodings(String s) {
  if (s == null || s.length() == 0) {
    return 0;
  }
  int[] M = new int[s.length() + 1];
  M[0] = 1;
  M[1] = s.charAt(0) != '0' ? 1 : 0;
  for (int i = 2; i <= s.length(); i++) {
    if (s.charAt(i - 1) == '0') {
      if (s.charAt(i - 2) == '1' || s.charAt(i - 2) == '2') {
        M[i] = M[i - 2];
      }
    } else if ((s.charAt(i - 2) == '1' && s.charAt(i - 1) != '0') || (s.charAt(i - 2) == '2' && s.charAt(i - 1) > '0' && s.charAt(i - 1) < '7')) {
      M[i] = M[i - 1] + M[i - 2];
    } else {
      M[i] = M[i - 1];
    }
  }
  return M[s.length()];
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 10. Unique Paths (medium)

[LeetCode 62](https://leetcode.com/problems/unique-paths/)

|      |  1   |  1   |  1   |  1   |  1   |  1   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  2   |  3   |  4   |  5   |  6   |  7   |
|  1   |  3   |  6   |  10  |  15  |  21  |  28  |

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

## 11. Jump Game (medium)

[LeetCode 55](https://leetcode.com/problems/jump-game/)

### Solution 1: dynamic programming

1. base case: M[n - 1] = true
2. induction rule
   * M[i] represents whether we could reach the target from index i
   * M[i] = true, if there exists a j, where M[j] == true AND j <= i + input[i]
   * M[i] = false, otherwise

```java
public boolean canJump(int[] nums) {
  if (nums == null || nums.length == 0) {
    return false;
  }
  if (nums.length == 1) {
    return true;
  }
  boolean[] M = new boolean[nums.length];
  for (int i = nums.length - 2; i >= 0; i--) {
    if (i + nums[i] >= nums.length - 1) {
      M[i] = true;
    } else {
      for (int j = nums[i]; j >= 1; j--) {
        if (M[j + i]) {
          M[i] = true;
          break;
        }
      }
    }
  }
  return M[0];
}
```

Time Complexity: O(n<sup>2</sup>)

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
