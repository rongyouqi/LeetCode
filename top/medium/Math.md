# Medium Collection Day 4 (Math)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/113/math/

## 1. Happy Number (easy)

[LeetCode 202](https://leetcode.com/problems/happy-number/)

```java
public boolean isHappy(int n) {
    while (n != 1 && n != 4) {
        n = next(n);
    }
    return n == 1;
}

private int next(int n) {
    int result = 0;
    while (n > 0) {
        int digit = n % 10;
        result += digit * digit;
        n /= 10;
    }
    return result;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 2. Factorial Trailing Zeroes (medium)

[LeetCode 172](https://leetcode.com/problems/factorial-trailing-zeroes/)

```java
public int trailingZeroes(int n) {
    int result = 0;
    while (n > 0) {
        n /= 5;
        result += n;
    }
    return result;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 3. Excel Sheet Column Number (easy)

[LeetCode 171](https://leetcode.com/problems/excel-sheet-column-number/)

```java
public int titleToNumber(String columnTitle) {
    int result = 0;
    for (int i = 0; i < columnTitle.length(); i++) {
        result *= 26;
        result += (columnTitle.charAt(i) - 'A' + 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. Pow(x, n) (medium)

[LeetCode 50](https://leetcode.com/problems/powx-n/)

```java
public double myPow(double x, int n) {
    long ln = n;
    if (ln < 0) {
        x = 1 / x;
        ln = -ln;
    }
    return helper(x, ln);
}

private double helper(double x, long ln) {
    if (ln == 0) {
        return 1.0;
    }
    double half = helper(x, ln / 2);
    if (ln % 2 == 0) {
        return half * half;
    }
    return half * half * x;
}
```

Time Complexity: O(logn)

Space Complexity: O(logn)

## 5. Sqrt(x) (easy)

[LeetCode 69](https://leetcode.com/problems/sqrtx/)

```java
public int mySqrt(int x) {
    if (x < 2) {
        return x;
    }
    int left = 2, right = x / 2;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        long pow = (long)mid * mid;
        if (pow == x) {
            return mid;
        } else if (pow > x) {
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    return right;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 6. Divide Two Integers (medium)

[LeetCode 29](https://leetcode.com/problems/divide-two-integers/)

```java
public int divide(int dividend, int divisor) {
    if (dividend == Integer.MIN_VALUE && divisor == -1) {
        return Integer.MAX_VALUE;
    }
    boolean negative = dividend < 0 ^ divisor < 0;
    dividend = Math.abs(dividend);
    divisor = Math.abs(divisor);
    int result = 0;
    while (dividend - divisor >= 0) {
        int i = 0;
        while (dividend - (divisor << i << 1) >= 0) {
            i++;
        }
        result += 1 << i;
        dividend -= divisor << i;
    }
    return negative ? -result : result;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 7. Fraction to Recurring Decimal (medium)

[LeetCode 166](https://leetcode.com/problems/fraction-to-recurring-decimal/)

```java
public String fractionToDecimal(int numerator, int denominator) {
    if (numerator == 0) {
        return "0";
    }
    StringBuilder fraction = new StringBuilder();
    if (numerator < 0 ^ denominator < 0) {
        fraction.append("-");
    }
    long dividend = Math.abs(Long.valueOf(numerator));
    long divisor = Math.abs(Long.valueOf(denominator));
    fraction.append(String.valueOf(dividend / divisor));
    long remainder = dividend % divisor;
    if (remainder == 0) {
        return fraction.toString();
    }
    fraction.append(".");
    Map<Long, Integer> map = new HashMap<>();
    while (remainder != 0) {
        if (map.containsKey(remainder)) {
            fraction.insert(map.get(remainder), "(");
            fraction.append(")");
            break;
        }
        map.put(remainder, fraction.length());
        remainder *= 10;
        fraction.append(String.valueOf(remainder / divisor));
        remainder %= divisor;
    }
    return fraction.toString();
}
```

