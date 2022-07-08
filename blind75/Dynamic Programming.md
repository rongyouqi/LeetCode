# Blind 75 Day 3 (Dynamic Programming)

**表象上填表格, 实质上用空间换取时间**

1. base case: M[0]
2. induction rule
     * 英文物理意义: M[i] represents what
     * 数学表达式: relationship between M[i] and M[i - 1], etc.

## 1. [LeetCode 70](https://leetcode.com/problems/climbing-stairs/) Climbing Stairs (easy)

- You are climbing a staircase. It takes `n` steps to reach the top.
- Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?
- **Example 1:**
    - **Input:** n = 2
    - **Output:** 2
    - **Explanation:** There are two ways to climb to the top.
        1. 1 step + 1 step
        2. 2 steps
- **Example 2:**
    - **Input:** n = 3
    - **Output:** 3
    - **Explanation:** There are three ways to climb to the top.
        1. 1 step + 1 step + 1 step
        2. 1 step + 2 steps
        3. 2 steps + 1 step
- **Constraints:**
    -   `1 <= n <= 45`

### Solution

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

### Solution 1: space optimized dynamic programming

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

### Solution 2: matrix multiplication

```java
public int climbStairs(int n) {
    int[][] q = {{1, 1}, {1, 0}};
    int[][] result = power(q, n);
    return result[0][0];
}

private int[][] power(int[][] a, int n) {
    int[][] result = {{1, 0}, {0, 1}};
    while (n > 0) {
        if ((n & 1) == 1) {
            result = multiply(result, a);
        }
        n >>= 1;
        a = multiply(a, a);
    }
    return result;
}

private int[][] multiply(int[][] a, int[][] b) {
    int[][] result = new int[2][2];
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 2; j++) {
            result[i][j] = a[i][0] * b[0][j] + a[i][1] * b[1][j];
        }
    }
    return result;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

### Solution 3: Fibonacci formula

```java
public int climbStairs(int n) {
    double sqrt5 = Math.sqrt(5);
    double phi = (1 + sqrt5) / 2;
    double psi = (1 - sqrt5) / 2;
    return (int) ((Math.pow(phi, n + 1) - Math.pow(psi, n + 1)) / sqrt5);
}
```

Time Complexity: O(logn) // pow method

Space Complexity: O(1)

## 2. [LeetCode 322](https://leetcode.com/problems/coin-change/) Coin Change (medium)

- You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.
- Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.
- You may assume that you have an infinite number of each kind of coin.
- **Example 1:**
    - **Input:** coins = [1,2,5], amount = 11
    - **Output:** 3
    - **Explanation:** 11 = 5 + 5 + 1
- **Example 2:**
    - **Input:** coins = [2], amount = 3
    - **Output:** -1
- **Example 3:**
    - **Input:** coins = [1], amount = 0
    - **Output:** 0
- **Constraints:**
    -   `1 <= coins.length <= 12`
    -   `1 <= coins[i] <= 2^31 - 1`
    -   `0 <= amount <= 10^4`

### Solution

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
    Arrays.fill(M, amount + 1); // Integer.MAX_VALUE ??? why wrong ? overflow ?
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

## 3. [LeetCode 300](https://leetcode.com/problems/longest-increasing-subsequence/) Longest Increasing Subsequence (medium)

- Given an integer array `nums`, return the length of the longest strictly increasing subsequence.
- A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.
- **Example 1:**
    - **Input:** nums = [10,9,2,5,3,7,101,18]
    - **Output:** 4
    - **Explanation:** The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
- **Example 2:**
    - **Input:** nums = [0,1,0,3,2,3]
    - **Output:** 4
- **Example 3:**
    - **Input:** nums = [7,7,7,7,7,7,7]
    - **Output:** 1
- **Constraints:**
    -   `1 <= nums.length <= 2500`
    -   `-10^4 <= nums[i] <= 10^4`
- **Follow up:** Can you come up with an algorithm that runs in `O(nlogn)` time complexity?

### Solution 1: dynamic programming

| nums |    10    |    9     |    2     |    5     |    3     |    7     | 101    |    18    |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|    M     |    1     |    1     |    1     |    2     |    2     |    3     |    4     |    4     |

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

Time Complexity: O(n^2)

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

## 4. [LeetCode 1143](https://leetcode.com/problems/longest-common-subsequence/) Longest Common Subsequence (medium)

- Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.
- A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
    -   For example, `"ace"` is a subsequence of `"abcde"`.
- A **common subsequence** of two strings is a subsequence that is common to both strings.
- **Example 1:**
    - **Input:** text1 = "abcde", text2 = "ace" 
    - **Output:** 3  
    - **Explanation:** The longest common subsequence is "ace" and its length is 3.
- **Example 2:**
    - **Input:** text1 = "abc", text2 = "abc"
    - **Output:** 3
    - **Explanation:** The longest common subsequence is "abc" and its length is 3.
- **Example 3:**
    - **Input:** text1 = "abc", text2 = "def"
    - **Output:** 0
    - **Explanation:** There is no such common subsequence, so the result is 0.
- **Constraints:**
    -   `1 <= text1.length, text2.length <= 1000`
    -   `text1` and `text2` consist of only lowercase English characters.

### Solution

|            |    _     |    a     |    b     |    c     |    d     |    e     |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|    _     |    0     |    0     |    0     |    0     |    0     |    0     |
|    a     |    0     |    1     |    1     |    1     |    1     |    1     |
|    c     |    0     |    1     |    1     |    2     |    2     |    2     |
|    e     |    0     |    1     |    1     |    2     |    2     |    3     |

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

## 5. [LeetCode 139](https://leetcode.com/problems/word-break/) Word Break Problem (medium)

- Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.
- **Note** that the same word in the dictionary may be reused multiple times in the segmentation.
- **Example 1:**
    - **Input:** s = "leetcode", wordDict = ["leet","code"]
    - **Output:** true
    - **Explanation:** Return true because "leetcode" can be segmented as "leet code".
- **Example 2:**
    - **Input:** s = "applepenapple", wordDict = ["apple","pen"]
    - **Output:** true
    - **Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple".
        - Note that you are allowed to reuse a dictionary word.
- **Example 3:**
    - **Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
    - **Output:** false
- **Constraints:**
    -   `1 <= s.length <= 300`
    -   `1 <= wordDict.length <= 1000`
    -   `1 <= wordDict[i].length <= 20`
    -   `s` and `wordDict[i]` consist of only lowercase English letters.
    -   All the strings of `wordDict` are **unique**.

### Solution

s = "leetcode", wordDict = ["leet", "code"]

|    s     |            |     l     |     e     |     e     |    t     |     c     |     o     |     d     |    e     |
| :--: | :--: | :---: | :---: | :---: | :--: | :---: | :---: | :---: | :--: |
|    M     | true | false | false | false | true | false | false | false | true |

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

Time Complexity: O(n^3) // string API is very slow

Space Complexity: O(n)

## 6. [LeetCode 377](https://leetcode.com/problems/combination-sum-iv/) Combination Sum (medium)

- Given an array of **distinct** integers `nums` and a target integer `target`, return _the number of possible combinations that add up to_ `target`.
- The test cases are generated so that the answer can fit in a **32-bit** integer.
- **Example 1:**
    - **Input:** nums = [1,2,3], target = 4
    - **Output:** 7
    - **Explanation:** The possible combination ways are:
        - (1, 1, 1, 1)
        - (1, 1, 2)
        - (1, 2, 1)
        - (1, 3)
        - (2, 1, 1)
        - (2, 2)
        - (3, 1)
        - Note that different sequences are counted as different combinations.
- **Example 2:**
    - **Input:** nums = [9], target = 3
    - **Output:** 0
- **Constraints:**
    -   `1 <= nums.length <= 200`
    -   `1 <= nums[i] <= 1000`
    -   All the elements of `nums` are **unique**.
    -   `1 <= target <= 1000`
- **Follow up:** What if negative numbers are allowed in the given array? How does it change the problem? What limitation we need to add to the question to allow negative numbers?

### Solution

nums = [1, 2, 3], target = 4

| target |    0     |    1     |    2     |    3     |    4     |
| :----: | :--: | :--: | :--: | :--: | :--: |
|     M        |    1     |    1     |    2     |    4     |    7     |

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

## 7. [LeetCode 198](https://leetcode.com/problems/house-robber/) House Robber (medium)

- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
- Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.
- **Example 1:**
    - **Input:** nums = [1,2,3,1]
    - **Output:** 4
    - **Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
        - Total amount you can rob = 1 + 3 = 4.
- **Example 2:**
    - **Input:** nums = [2,7,9,3,1]
    - **Output:** 12
    - **Explanation:** Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
        - Total amount you can rob = 2 + 9 + 1 = 12.
- **Constraints:**
    -   `1 <= nums.length <= 100`
    -   `0 <= nums[i] <= 400`

### Solution

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

## 8. [LeetCode 213](https://leetcode.com/problems/house-robber-ii/) House Robber II (medium)

- You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.
- Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.
- **Example 1:**
    - **Input:** nums = [2,3,2]
    - **Output:** 3
    - **Explanation:** You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
- **Example 2:**
    - **Input:** nums = [1,2,3,1]
    - **Output:** 4
    - **Explanation:** Rob house 1 (money = 1) and then rob house 3 (money = 3).
        - Total amount you can rob = 1 + 3 = 4.
- **Example 3:**
    - **Input:** nums = [1,2,3]
    - **Output:** 3
- **Constraints:**
    -   `1 <= nums.length <= 100`
    -   `0 <= nums[i] <= 1000`

### Solution

* two subproblems:
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

## 9. [LeetCode 91](https://leetcode.com/problems/decode-ways/) Decode Ways (medium)

- A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:
    ```
    'A' -> "1"
    'B' -> "2"
    ...
    'Z' -> "26"
    ```
- To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:
    -   `"AAJF"` with the grouping `(1 1 10 6)`
    -   `"KJF"` with the grouping `(11 10 6)`
- Note that the grouping `(1 11 06)` is invalid because `"06"` cannot be mapped into `'F'` since `"6"` is different from `"06"`.
- Given a string `s` containing only digits, return _the **number** of ways to **decode** it_.
- The test cases are generated so that the answer fits in a **32-bit** integer.
- **Example 1:**
    - **Input:** s = "12"
    - **Output:** 2
    - **Explanation:** "12" could be decoded as "AB" (1 2) or "L" (12).
- **Example 2:**
    - **Input:** s = "226"
    - **Output:** 3
    - **Explanation:** "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
- **Example 3:**
    - **Input:** s = "06"
    - **Output:** 0
    - **Explanation:** "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
- **Constraints:**
    -   `1 <= s.length <= 100`
    -   `s` contains only digits and may contain leading zero(s).

### Solution

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

## 10. [LeetCode 62](https://leetcode.com/problems/unique-paths/) Unique Paths (medium)

- There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner**(i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.
- Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.
- The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png"  />
    - **Input:** m = 3, n = 7
    - **Output:** 28
- **Example 2:**
    - **Input:** m = 3, n = 2
    - **Output:** 3
    - **Explanation:** From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
        1. Right -> Down -> Down
        2. Down -> Down -> Right
        3. Down -> Right -> Down
- **Constraints:**
    -   `1 <= m, n <= 100`

### Solution 1: dynamic programming

|            |    1     |    1     |    1     |    1     |    1     |    1     |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|    1     |    2     |    3     |    4     |    5     |    6     |    7     |
|    1     |    3     |    6     |    10    |    15    |    21    |    28    |

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

### Solution 2: math `combination(m + n, m) = (m + n)! / (m! * n!)`

```java
public int uniquePaths(int m, int n) {
    if (m == 1 || n == 1) {
        return 1;
    }
    if (m < n) {
        return uniquePaths(n, m);
    }
    m--;
    n--;
    long result = 1;
    for (int i = m + 1, j = 1; i <= m + n; i++, j++) {
        result *= i;
        result /= j;
    }
    return (int)result;
}
```

Time Complexity: O(min(m, n))

Space Complexity: O(1)

## 11. [LeetCode 55](https://leetcode.com/problems/jump-game/) Jump Game (medium)

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
