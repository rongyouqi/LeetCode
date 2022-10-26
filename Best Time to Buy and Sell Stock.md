# Best Time to Buy and Sell Stock

```java
// base case
dp[-1][...][0] = dp[...][0][0] = 0
dp[-1][...][1] = dp[...][0][1] = -infinity

// induction rule
dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])
```

<img src="https://labuladong.github.io/algo/images/%e8%82%a1%e7%a5%a8%e9%97%ae%e9%a2%98/1.png" alt="img" style="zoom:67%;" />

```java
// 同时考虑交易次数的限制、冷冻期和手续费
int maxProfit_all_in_one(int max_k, int[] prices, int cooldown, int fee) {
    int n = prices.length;
    if (n <= 0) {
        return 0;
    }
    if (max_k > n / 2) {
        // 交易次数 k 没有限制的情况
        return maxProfit_k_inf(prices, cooldown, fee);
    }

    int[][][] dp = new int[n][max_k + 1][2];
    // k = 0 时的 base case
    for (int i = 0; i < n; i++) {
        dp[i][0][1] = Integer.MIN_VALUE;
        dp[i][0][0] = 0;
    }

    for (int i = 0; i < n; i++) 
        for (int k = max_k; k >= 1; k--) {
            if (i - 1 == -1) {
                // base case 1
                dp[i][k][0] = 0;
                dp[i][k][1] = -prices[i] - fee;
                continue;
            }

            // 包含 cooldown 的 base case
            if (i - cooldown - 1 < 0) {
                // base case 2
                dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
                // 别忘了减 fee
                dp[i][k][1] = Math.max(dp[i-1][k][1], -prices[i] - fee);
                continue;
            }
            dp[i][k][0] = Math.max(dp[i-1][k][0], dp[i-1][k][1] + prices[i]);
            // 同时考虑 cooldown 和 fee
            dp[i][k][1] = Math.max(dp[i-1][k][1], dp[i-cooldown-1][k-1][0] - prices[i] - fee);     
        }
    return dp[n - 1][max_k][0];
}

// k 无限制，包含手续费和冷冻期
int maxProfit_k_inf(int[] prices, int cooldown, int fee) {
    int n = prices.length;
    int[][] dp = new int[n][2];
    for (int i = 0; i < n; i++) {
        if (i - 1 == -1) {
            // base case 1
            dp[i][0] = 0;
            dp[i][1] = -prices[i] - fee;
            continue;
        }

        // 包含 cooldown 的 base case
        if (i - cooldown - 1 < 0) {
            // base case 2
            dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
            // 别忘了减 fee
            dp[i][1] = Math.max(dp[i-1][1], -prices[i] - fee);
            continue;
        }
        dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
        // 同时考虑 cooldown 和 fee
        dp[i][1] = Math.max(dp[i - 1][1], dp[i - cooldown - 1][0] - prices[i] - fee);
    }
    return dp[n - 1][0];
}
```

## 1. [LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) Best Time to Buy and Sell Stock (easy)

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
- Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.
- **Example 1:**
    - **Input:** prices = [7,1,5,3,6,4]
    - **Output:** 5
    - **Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6 - 1 = 5.
        - Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
- **Example 2:**
    - **Input:** prices = [7,6,4,3,1]
    - **Output:** 0
    - **Explanation:** In this case, no transactions are done and the max profit = 0.
- **Constraints:**
    -   `1 <= prices.length <= 10^5`
    -   `0 <= prices[i] <= 10^4`

### Solution

```java
public int maxProfit(int[] prices) {
    int min = Integer.MAX_VALUE, result = 0;
    for (int price : prices) {
        min = Math.min(min, price);
        result = Math.max(result, price - min);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. [LeetCode 122](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) Best Time to Buy and Sell Stock II (medium)

- You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.
- Find and return _the **maximum** profit you can achieve_.
- **Example 1:**
    - **Input:** prices = [7,1,5,3,6,4]
    - **Output:** 7
    - **Explanation:**
        - Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5 - 1 = 4.
        - Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6 - 3 = 3.
        - Total profit is 4 + 3 = 7.
- **Example 2:**
    - **Input:** prices = [1,2,3,4,5]
    - **Output:** 4
    - **Explanation:**
        - Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5 - 1 = 4.
        - Total profit is 4.
- **Example 3:**
    - **Input:** prices = [7,6,4,3,1]
    - **Output:** 0
    - **Explanation:** There is no way to make a positive profit, so we never buy the stock to achieve the maximum profit of 0.
- **Constraints:**
    -   `1 <= prices.length <= 3 * 10^4`
    -   `0 <= prices[i] <= 10^4`

### Solution

```java
public int maxProfit(int[] prices) {
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

## 3. [LeetCode 123](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/) Best Time to Buy and Sell Stock III (hard)

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- Find the maximum profit you can achieve. You may complete **at most two transactions**.
- **Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- **Example 1:**
    - **Input:** prices = [3,3,5,0,0,3,1,4]
    - **Output:** 6
    - **Explanation:**
        - Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3 - 0 = 3.
        - Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4 - 1 = 3.
- **Example 2:**
    - **Input:** prices = [1,2,3,4,5]
    - **Output:** 4
    - **Explanation:**
        - Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5 - 1 = 4.
        - Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
- **Example 3:**
    - **Input:** prices = [7,6,4,3,1]
    - **Output:** 0
    - **Explanation:** In this case, no transaction is done, i.e. max profit = 0.
- **Constraints:**
    -   `1 <= prices.length <= 10^5`
    -   `0 <= prices[i] <= 10^5`

### Solution

```java
public int maxProfit(int[] prices) {
    return maxProfit(2, prices);
}

private int maxProfit(int k, int[] prices) {
	int[] profit = new int[k + 1], cost = new int[k + 1];
	profit[0] = 0;
	Arrays.fill(cost, Integer.MAX_VALUE);
	for (int price : prices) {
    	for (int i = 0; i < k; i++) {
        	cost[i + 1] = Math.min(cost[i + 1], price - profit[i]);
        	profit[i + 1] = Math.max(profit[i + 1], price - cost[i + 1]);
    	}
	}
	return profit[k];
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. [LeetCode 188](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/) Best Time to Buy and Sell Stock IV (hard)

- You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `k`.
- Find the maximum profit you can achieve. You may complete at most `k` transactions.
- **Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- **Example 1:**
    - **Input:** k = 2, prices = [2,4,1]
    - **Output:** 2
    - **Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
- **Example 2:**
    - **Input:** k = 2, prices = [3,2,6,5,0,3]
    - **Output:** 7
    - **Explanation:** Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
- **Constraints:**
    -   `1 <= k <= 100`
    -   `1 <= prices.length <= 1000`
    -   `0 <= prices[i] <= 1000`

### Solution

```java
public int maxProfit(int k, int[] prices) {
	int[] profit = new int[k + 1], cost = new int[k + 1];
	profit[0] = 0;
	Arrays.fill(cost, Integer.MAX_VALUE);
	for (int price : prices) {
    	for (int i = 0; i < k; i++) {
        	cost[i + 1] = Math.min(cost[i + 1], price - profit[i]);
        	profit[i + 1] = Math.max(profit[i + 1], price - cost[i + 1]);
    	}
	}
	return profit[k];
}
```

Time Complexity: O(nk)

Space Complexity: O(k)

## 5. [LeetCode 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) Best Time to Buy and Sell Stock with Cooldown (medium)

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
	int sold = 0, hold = Integer.MIN_VALUE, rest = 0;
    for (int price : prices) {
        int prevSold = sold;
        sold = hold + price;
        hold = Math.max(hold, rest - price);
        rest = Math.max(rest, prevSold);
    }
    return Math.max(sold, rest);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. [LeetCode 714](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) Best Time to Buy and Sell Stock with Transaction Fee (medium)

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day, and an integer `fee` representing a transaction fee.
- Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.
- **Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
- **Example 1:**
    - **Input:** prices = [1,3,2,8,4,9], fee = 2
    - **Output:** 8
    - **Explanation:** The maximum profit can be achieved by:
        - Buying at prices[0] = 1
        - Selling at prices[3] = 8
        - Buying at prices[4] = 4
        - Selling at prices[5] = 9
        - The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
- **Example 2:**
    - **Input:** prices = [1,3,7,5,10,3], fee = 3
    - **Output:** 6
- **Constraints:**
    -   `1 <= prices.length <= 5 * 10^4`
    -   `1 <= prices[i] < 5 * 10^4`
    -   `0 <= fee < 5 * 10^4`

### Solution

```java
public int maxProfit(int[] prices, int fee) {
	int sold = 0, hold = -prices[0];
    for (int price : prices) {
        sold = Math.max(sold, hold + price - fee);
        hold = Math.max(hold, sold - price);
    }
    return sold;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## [LeetCode 2291](https://leetcode.com/problems/maximum-profit-from-trading-stocks/) Maximum Profit From Trading Stocks (medium)

- You are given two **0-indexed** integer arrays of the same length `present` and `future` where `present[i]` is the current price of the `ith` stock and `future[i]` is the price of the `ith` stock a year in the future. You may buy each stock at most **once**. You are also given an integer `budget` representing the amount of money you currently have.
- Return _the maximum amount of profit you can make._
- **Example 1:**
    - **Input:** present = [5,4,6,2,3], future = [8,5,4,3,5], budget = 10
    - **Output:** 6
    - **Explanation:** One possible way to maximize your profit is to:
        - Buy the 0th, 3rd, and 4th stocks for a total of 5 + 2 + 3 = 10.
        - Next year, sell all three stocks for a total of 8 + 3 + 5 = 16.
        - The profit you made is 16 - 10 = 6.
        - It can be shown that the maximum profit you can make is 6.
- **Example 2:**
    - **Input:** present = [2,2,5], future = [3,4,10], budget = 6
    - **Output:** 5
    - **Explanation:** The only possible way to maximize your profit is to:
        - Buy the 2nd stock, and make a profit of 10 - 5 = 5.
        - It can be shown that the maximum profit you can make is 5.
- **Example 3:**
    - **Input:** present = [3,3,12], future = [0,3,15], budget = 10
    - **Output:** 0
    - **Explanation:** One possible way to maximize your profit is to:
        - Buy the 1st stock, and make a profit of 3 - 3 = 0.
        - It can be shown that the maximum profit you can make is 0.
- **Constraints:**
    -   `n == present.length == future.length`
    -   `1 <= n <= 1000`
    -   `0 <= present[i], future[i] <= 100`
    -   `0 <= budget <= 1000`

### Solution

```java
public int maximumProfit(int[] present, int[] future, int budget) {
    int[] dp = new int[budget + 1];
    for (int i = 0; i < present.length; i++) {
        for (int j = budget; j >= 0; j--) {
            if (present[i] <= j && present[i] < future[i]) {
                dp[j] = Math.max(dp[j], dp[j - present[i]] + future[i] - present[i]);
            }
        }
    }
    return dp[budget];
}
```

Time Complexity: O(n * budget)

Space Complexity: O(budget)
