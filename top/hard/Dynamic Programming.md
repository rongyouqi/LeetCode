# Hard Collection Day 6 (Dynamic Programming)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/121/dynamic-programming/

## 1. [LeetCode 152](https://leetcode.com/problems/maximum-product-subarray/) Maximum Product Subarray (medium)

- Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return _the product_.
- The test cases are generated so that the answer will fit in a **32-bit**integer.
- A **subarray** is a contiguous subsequence of the array.
- **Example 1:**
    - **Input:** nums = [2,3,-2,4]
    - **Output:** 6
    - **Explanation:** [2,3] has the largest product 6.
- **Example 2:**
    - **Input:** nums = [-2,0,-1]
    - **Output:** 0
    - **Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.
- **Constraints:**
    -   `1 <= nums.length <= 2 * 10^4`
    -   `-10 <= nums[i] <= 10`
    -   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

### Solution

```java
public int maxProduct(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int lastMin = nums[0];
    int lastMax = nums[0];
    int globalMax = nums[0];
    for (int i = 1; i < nums.length; i++) {
        int temp = lastMin;
        lastMin = Math.min(Math.min(lastMin * nums[i], lastMax * nums[i]), nums[i]);
        lastMax = Math.max(Math.max(temp * nums[i], lastMax * nums[i]), nums[i]);
        globalMax = Math.max(globalMax, lastMax);
    }
    return globalMax;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. [LeetCode](https://leetcode.com/problems/decode-ways/) Decode Ways (medium)

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

## 3. [LeetCode 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) Best Time to Buy and Sell Stock with Cooldown (medium)

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
    -   After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
- **Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- **Example 1:**
    - **Input:** prices = [1,2,3,0,2]
    - **Output:** 3
    - **Explanation:** transactions = [buy, sell, cooldown, buy, sell]
- **Example 2:**
    - **Input:** prices = [1]
    - **Output:** 0
- **Constraints:**
    -   `1 <= prices.length <= 5000`
    -   `0 <= prices[i] <= 1000`

### Solution

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
        return 0;
    }
    int sell = Integer.MIN_VALUE, buy = Integer.MIN_VALUE, cooldown = 0;
    for (int price : prices) {
        int temp = sell;
        sell = buy + price;
        buy = Math.max(buy, cooldown - price);
        cooldown = Math.max(cooldown, temp);
    }
    return Math.max(sell, cooldown);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. [LeetCode 279](https://leetcode.com/problems/perfect-squares/) Perfect Squares (medium)

- Given an integer `n`, return _the least number of perfect square numbers that sum to_`n`.
- A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.
- **Example 1:**
    - **Input:** n = 12
    - **Output:** 3
    - **Explanation:** 12 = 4 + 4 + 4.
- **Example 2:**
    - **Input:** n = 13
    - **Output:** 2
    - **Explanation:** 13 = 4 + 9.
- **Constraints:**
    -   `1 <= n <= 10^4`

### Solution 1: dynamic programming

```java
public int numSquares(int n) {
    int[] dp = new int[n + 1];
    dp[0] = 0;
    for (int i = 1; i <= n; i++) {
        dp[i] = i;
        for (int j = 1; j * j <= i; j++) {
            dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
        }
    }
    return dp[n];
}
```

Time Complexity: O(n^(3/2))

Space Complexity: O(n)

### Solution 2: mathematics

```java
public int numSquares(int n) {
    while (n % 4 == 0) {
        n /= 4;
    }
    if (n % 8 == 7) {
        return 4;
    }
    if (isSquare(n)) {
        return 1;
    }
    for (int i = 1; i * i <= n; i++) {
        if (isSquare(n - i * i)) {
            return 2;
        }
    }
    return 3;
}

private boolean isSquare(int n) {
    int num = (int)Math.sqrt(n);
    return n == num * num;
}
```

Time Complexity: O(n^(1/2))

Space Complexity: O(1)

## 5. [LeetCode 139](https://leetcode.com/problems/word-break/) Word Break (medium)

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

Time Complexity: O(n^3)

Space Complexity: O(n)

## 6. [LeetCode 140](https://leetcode.com/problems/word-break-ii/) Word Break II (hard)

- Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.
- **Note** that the same word in the dictionary may be reused multiple times in the segmentation.
- **Example 1:**
    - **Input:** s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
    - **Output:** ["cats and dog","cat sand dog"]
- **Example 2:**
    - **Input:** s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
    - **Output:** ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
    - **Explanation:** Note that you are allowed to reuse a dictionary word.
- **Example 3:**
    - **Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
    - **Output:** []
- **Constraints:**
    -   `1 <= s.length <= 20`
    -   `1 <= wordDict.length <= 1000`
    -   `1 <= wordDict[i].length <= 10`
    -   `s` and `wordDict[i]` consist of only lowercase English letters.
    -   All the strings of `wordDict` are **unique**.

### Solution: backtracking

```java
public List<String> wordBreak(String s, List<String> wordDict) {
    return helper(s, wordDict, new HashMap<>());
}

private List<String> helper(String s, List<String> wordDict, Map<String, List<String>> map) {
    if (map.containsKey(s)) {
        return map.get(s);
    }
    List<String> result = new ArrayList<>();
    for (String word : wordDict) {
        if (!s.startsWith(word)) {
            continue;
        }
        if (s.length() == word.length()) {
            result.add(word);
            continue;
        }
        List<String> subs = helper(s.substring(word.length()), wordDict, map);
        for (String sub : subs) {
            StringBuilder sb = new StringBuilder();
            sb.append(word).append(' ').append(sub);
            result.add(sb.toString());
        }
    }
    map.put(s, result);
    return result;
}
```

## 7. [LeetCode 312](https://leetcode.com/problems/burst-balloons/solution/) Burst Balloons (hard)

- You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.
- If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.
- Return _the maximum coins you can collect by bursting the balloons wisely_.
- **Example 1:**
    - **Input:** nums = [3,1,5,8]
    - **Output:** 167
    - **Explanation:**
        - nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
        - coins =  `3*1*5 + 3*5*8 + 1*3*8 + 1*8*1 = 167`
- **Example 2:**
    - **Input:** nums = [1,5]
    - **Output:** 10
- **Constraints:**
    -   `n == nums.length`
    -   `1 <= n <= 300`
    -   `0 <= nums[i] <= 100`

### Solution

```java
public int maxCoins(int[] nums) {
    int[] array = new int[nums.length + 2];
    int i = 1;
    for (int num : nums) {
        array[i++] = num;
    }
    array[0] = array[i] = 1;
    int[][] dp = new int[array.length][array.length];
    for (int j = 2; j < array.length; j++) {
        for (int left = 0; left < array.length - j; left++) {
            int right = left + j;
            for (int k = left + 1; k < right; k++) {
                dp[left][right] = Math.max(dp[left][right], array[left] * array[k] * array[right] + dp[left][k] + dp[k][right]);
            }
        }
    }
    return dp[0][array.length - 1];
}
```

Time Complexity: O(n^3)

Space Complexity: O(n^2)

