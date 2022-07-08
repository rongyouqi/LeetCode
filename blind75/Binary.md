# Blind 75 Day 2 (Binary)

* bit operation
    1. `&` (bitwise AND)
    2. `|` (bitwise OR)
    3. `~` (bitwise NOT)
    4. `^` (bitwise XOR)
    5. `<<` (left shift): 右侧补充零
    6. `>>` (right shift): 左侧补充原先的符号位
    7. `>>>`: 无符号右移, 忽略符号位, 空位都以 0 补齐
* building blocks: k = 0 -> least significant bit (从右往左数第 k 位)
    1. (**bit tester**) Given an integer x, test whether x's k-th bit is one.
         `int bit_k = (x >> k) & 1;`
    2. (**bit setter**) Given an integer x, set x's k-th bit to 1. (把第 k 位设成1, 其他位都不变)
         `x |= (1 << k);`
    3. (**bit resetter**) Given an integer x, set x's k-th bit to 0. (把第 k 位设成0, 其他位都不变)
         `x &= ~(1 << k);`

## 1. [LeetCode 371](https://leetcode.com/problems/sum-of-two-integers/) Sum of Two Integers (medium)

- Given two integers `a` and `b`, return _the sum of the two integers without using the operators_ `+` _and_ `-`.
- **Example 1:**
    - **Input:** a = 1, b = 2
    - **Output:** 3
- **Example 2:**
    - **Input:** a = 2, b = 3
    - **Output:** 5
- **Constraints:**
    -   `-1000 <= a, b <= 1000`

### Solution: bit manipulation

```java
public int getSum(int a, int b) {
    while (b != 0) {
        int answer = a ^ b;
        int carry = (a & b) << 1;
        a = answer;
        b = carry;
    }
    return a;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 2. [LeetCode 191](https://leetcode.com/problems/number-of-1-bits/) Number of 1 Bits (easy)

- Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).
- **Note:**
    -   Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
    -   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 3**, the input represents the signed integer. `-3`.
- **Example 1:**
    - **Input:** n = 00000000000000000000000000001011
    - **Output:** 3
    - **Explanation:** The input binary string **00000000000000000000000000001011** has a total of three '1' bits.
- **Example 2:**
    - **Input:** n = 00000000000000000000000010000000
    - **Output:** 1
    - **Explanation:** The input binary string **00000000000000000000000010000000** has a total of one '1' bit.
- **Example 3:**
    - **Input:** n = 11111111111111111111111111111101
    - **Output:** 31
    - **Explanation:** The input binary string **11111111111111111111111111111101** has a total of thirty one '1' bits.
- **Constraints:**
    -   The input must be a **binary string** of length `32`.
- **Follow up:** If this function is called many times, how would you optimize it?

### Solution: bit tester

```java
public int hammingWeight(int n) {
    int count = 0;
    for (int k = 0; k < 32; k++) {
        count += (n >> k) & 1;
    }
    return count;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 3. [LeetCode 338](https://leetcode.com/problems/counting-bits/) Counting Bits (easy)

- Given an integer `n`, return _an array_ `ans` _of length_ `n + 1` _such that for each_ `i` (`0 <= i <= n`)_,_ `ans[i]` _is the **number of**_ `1`_**'s** in the binary representation of_ `i`.
- **Example 1:**
    - **Input:** n = 2
    - **Output:** [0,1,1]
    - **Explanation:**
        - 0 --> 0
        - 1 --> 1
        - 2 --> 10
- **Example 2:**
    - **Input:** n = 5
    - **Output:** [0,1,1,2,1,2]
    - **Explanation:**
        - 0 --> 0
        - 1 --> 1
        - 2 --> 10
        - 3 --> 11
        - 4 --> 100
        - 5 --> 101
- **Constraints:**
    -   `0 <= n <= 10^5`
- **Follow up:**
    -   It is very easy to come up with a solution with a runtime of `O(nlogn)`. Can you do it in linear time `O(n)` and possibly in a single pass?
    -   Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

### Solution: dynamic programming

```java
public int[] countBits(int n) {
    if (n < 0) {
        return null;
    }
    int[] result = new int[n + 1];
    for (int i = 1; i <= n; i++) {
        result[i] = result[i >> 1] + (i & 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. [LeetCode 268](https://leetcode.com/problems/missing-number/) Missing Number (easy)

- Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._
- **Example 1:**
    - **Input:** nums = [3,0,1]
    - **Output:** 2
    - **Explanation:** n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
- **Example 2:**
    - **Input:** nums = [0,1]
    - **Output:** 2
    - **Explanation:** n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
- **Example 3:**
    - **Input:** nums = [9,6,4,2,3,5,7,0,1]
    - **Output:** 8
    - **Explanation:** n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
- **Constraints:**
    -   `n == nums.length`
    -   `1 <= n <= 10^4`
    -   `0 <= nums[i] <= n`
    -   All the numbers of `nums` are **unique**.
- **Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### Solution 1: sort

```java
public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    Arrays.sort(nums);
    if (nums[nums.length - 1] != nums.length) {
        return nums.length;
    }
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != i) {
            return i;
        }
    }
    return -1;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(1)

### Solution 2: hashset

```java
public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        set.add(num);
    }
    for (int i = 0; i <= nums.length; i++) {
        if (!set.contains(i)) {
            return i;
        }
    }
    return -1;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 3: sum

```java
public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    long target = (nums.length + 0L) * (nums.length + 1) / 2;
    long sum = 0L;
    for (int num : nums) {
        sum += num;
    }
    return (int) (target - sum);
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 4: bit manipulation (recommended)

step 1: XOR every element in input -> temp_result

step 2: temp_result XOR from 1 to n -> missing number

```java
public int missingNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    int xor = 0;
    for (int num : nums) {
        xor ^= num;
    }
    for (int i = 1; i <= nums.length; i++) {
        xor ^= i;
    }
    return xor;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. [LeetCode 190](https://leetcode.com/problems/reverse-bits/) Reverse Bits (easy)

- Reverse bits of a given 32 bits unsigned integer.
- **Note:**
    -   Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
    -   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.
- **Example 1:**
    - **Input:** n = 00000010100101000001111010011100
    - **Output:**    964176192 (00111001011110000010100101000000)
    - **Explanation:** The input binary string **00000010100101000001111010011100** represents the unsigned integer 43261596, so return 964176192 which its binary representation is **00111001011110000010100101000000**.
- **Example 2:**
    - **Input:** n = 11111111111111111111111111111101
    - **Output:**   3221225471 (10111111111111111111111111111111)
    - **Explanation:** The input binary string **11111111111111111111111111111101** represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is **10111111111111111111111111111111**.
- **Constraints:**
    -   The input must be a **binary string** of length `32`
- **Follow up:** If this function is called many times, how would you optimize it?

### Solution

```java
public int reverseBits(int n) {
    int i = 0, j = 31;
    while (i < j) {
        n = swap(n, i, j);
        i++;
        j--;
    }
    return n;
}

private int swap(int x, int i, int j) {
    int bit_i = (x >> i) & 1;
    int bit_j = (x >> j) & 1;
    if (bit_i == bit_j) {
        return x;
    }
    return x ^ ((1 << i) + (1 << j));
}
```

Time Complexity: O(1)

Space Complexity: O(1)

