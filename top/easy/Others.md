# Easy Collection Day 4 (Others)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/99/others/

## 1. Number of 1 Bits (easy)

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

## 2. Hamming Distance (easy)

[LeetCode 461](https://leetcode.com/problems/hamming-distance/)

### Solution 1

```java
public int hammingDistance(int x, int y) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        int bitX = (x >> i) & 1;
        int bitY = (y >> i) & 1;
        if (bitX != bitY) {
            result++;
        }
    }
    return result;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

### Solution 2

```java
public int hammingDistance(int x, int y) {
    return Integer.bitCount(x ^ y);
}
```

Time Complexity: O(1)

Space Complexity: O(1)

### Solution 3

```java
public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int result = 0;
    while (xor != 0) {
        if (xor % 2 == 1) {
            result++;
        }
        xor /= 2;
    }
    return result;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

### Solution 4

```java
public int hammingDistance(int x, int y) {
    int xor = x ^ y;
    int result = 0;
    while (xor != 0) {
        result++;
        xor &= xor - 1;
    }
    return result;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 3. Reverse Bits (easy)

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

## 4. Pascal's Triangle (easy)

[LeetCode 118](https://leetcode.com/problems/pascals-triangle/)

```java
public List<List<Integer>> generate(int numRows) {
    // assumption: numRows > 0
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>());
    result.get(0).add(1);
    for (int i = 1; i < numRows; i++) {
        List<Integer> current = new ArrayList<>();
        List<Integer> previous = result.get(i - 1);
        current.add(1);
        for (int j = 1; j < i; j++) {
            current.add(previous.get(j - 1) + previous.get(j));
        }
        current.add(1);
        result.add(current);
    }
    return result;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

## 5. Valid Parentheses (easy)

[LeetCode 20](https://leetcode.com/problems/valid-parentheses/)

```java
public boolean isValid(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }
    Map<Character, Character> map = new HashMap<>();
    map.put(')', '(');
    map.put(']', '[');
    map.put('}', '{');
    Deque<Character> stack = new ArrayDeque<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (map.containsKey(c)) {
            if (stack.isEmpty()) {
                return false;
            }
            if (stack.pollFirst() != map.get(c)) {
                return false;
            }
        } else {
            stack.offerFirst(c);
        }
    }
    return stack.isEmpty();
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 6. Missing Number (easy)

[LeetCode 268](https://leetcode.com/problems/missing-number/)

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