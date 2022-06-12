# Easy Collection Day 3 (Dynamic Programming)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/97/dynamic-programming/

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

## 2. Best Time to Buy and Sell Stock (easy)

[LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length == 0) {
        return 0;
    }
    int minPriceSoFar = Integer.MAX_VALUE;
    int maxProfit = 0;
    for (int price : prices) {
        minPriceSoFar = Math.min(minPriceSoFar, price);
        maxProfit = Math.max(maxProfit, price - minPriceSoFar);
    }
    return maxProfit;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. Maximum Subarray (easy)

[LeetCode 53](https://leetcode.com/problems/maximum-subarray/)

### Solution: dynamic programming

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int lastMax = nums[0];
    int globalMax = nums[0];
    for (int i = 1; i < nums.length; i++) {
        lastMax = Math.max(lastMax + nums[i], nums[i]);
        // 继承遗产 or 另起炉灶
        globalMax = Math.max(globalMax, lastMax);
    }
    return globalMax;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. House Robber (medium)

[LeetCode 198](https://leetcode.com/problems/house-robber/)

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