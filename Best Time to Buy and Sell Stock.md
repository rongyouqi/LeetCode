# Best Time to Buy and Sell Stock

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
    
}
```

Time Complexity: O(n)

Space Complexity: O(1)

