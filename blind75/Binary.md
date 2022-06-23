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

## 1. Sum of Two Integers (medium)

[LeetCode 371](https://leetcode.com/problems/sum-of-two-integers/)

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

## 2. Number of 1 Bits (easy)

[LeetCode 191](https://leetcode.com/problems/number-of-1-bits/)

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

## 3. Counting Bits (easy)

[LeetCode 338](https://leetcode.com/problems/counting-bits/)

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

## 4. Missing Number (easy)

[LeetCode 268](https://leetcode.com/problems/missing-number/)

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

## 5. Reverse Bits (easy)

[LeetCode 190](https://leetcode.com/problems/reverse-bits/)

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

