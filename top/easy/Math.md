# Easy Collection Day 4 (Math)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/102/math/

## 1. Fizz Buzz (easy)

[LeetCode 412](https://leetcode.com/problems/fizz-buzz/)

```java
public List<String> fizzBuzz(int n) {
    List<String> result = new ArrayList<>();
    Map<Integer, String> map = new HashMap<>();
    map.put(3, "Fizz");
    map.put(5, "Buzz");
    int[] divisors = new int[]{3, 5};
    for (int i = 1; i <= n; i++) {
        String s = "";
        for (int divisor : divisors) {
            if (i % divisor == 0) {
                s += map.get(divisor);
            }
        }
        if (s.equals("")) {
            s += Integer.toString(i);
        }
        result.add(s);
    }
    return result;
}
```

Time Complexity: O(mn)

Space Complexity: O(m)

## 2. Count Primes (medium)

[LeetCode 204](https://leetcode.com/problems/count-primes/)

### Solution 1

```java
public int countPrimes(int n) {
    if (n <= 2) {
        return 0;
    }
    boolean[] numbers = new boolean[n];
    for (int i = 2; i <= (int)Math.sqrt(n); i++) {
        if (numbers[i] == false) {
            for (int j = i * i; j < n; j += i) {
                numbers[j] = true;
            }
        }
    }
    int result = 0;
    for (int i = 2; i < n; i++) {
        if (numbers[i] == false) {
            result++;
        }
    }
    return result;
}
```

Time Complexity: O(n<sup>1/2</sup>loglogn)

Space Complexity: O(n)

### Solution 2

```java
public int countPrimes(int n) {
    if (n < 3) {
        return 0;
    }
    int result = n / 2;
    boolean[] numbers = new boolean[n];
    for (int i = 3; i * i < n; i += 2) {
        if (numbers[i]) {
            continue;
        }
        for (int j = i * i; j < n; j += i * 2) {
            if (!numbers[j]) {
                numbers[j] = true;
                result--;
            }
        }
    }
    return result;
}
```

Time Complexity: O(n<sup>1/2</sup>loglogn)

Space Complexity: O(n)

## 3. Power of Three (easy)

[LeetCode 326](https://leetcode.com/problems/power-of-three/)

### Solution 1

```java
public boolean isPowerOfThree(int n) {
    if (n < 1) {
        return false;
    }
    while (n % 3 == 0) {
        n /= 3;
    }
    return n == 1;
}
```

Time Complexity: O(log<sub>3</sub>n)

Space Complexity: O(1)

### Solution 2

3<sup>log<sub>3</sub>Integer.MAX_VALUE</sup> = 3<sup>19</sup> = 1162261467

```java
public boolean isPowerOfThree(int n) {
    return n > 0 && 1162261467 % n == 0;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 4. Roman to Integer (easy)

[LeetCode 13](https://leetcode.com/problems/roman-to-integer/)

### Solution 1

```java
public int romanToInt(String s) {
    Map<String, Integer> map = new HashMap<>();
    map.put("I", 1);
    map.put("V", 5);
    map.put("X", 10);
    map.put("L", 50);
    map.put("C", 100);
    map.put("D", 500);
    map.put("M", 1000);
    map.put("IV", 4);
    map.put("IX", 9);
    map.put("XL", 40);
    map.put("XC", 90);
    map.put("CD", 400);
    map.put("CM", 900);
    int i = 0, result = 0;
    while (i < s.length()) {
        if (i < s.length() - 1) {
            String doubleCharacter = s.substring(i, i + 2);
            if (map.containsKey(doubleCharacter)) {
                result += map.get(doubleCharacter);
                i += 2;
                continue;
            }
        }
        String singleCharacter = s.substring(i, i + 1);
        result += map.get(singleCharacter);
        i++;
    }
    return result;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

### Solution 2

```java
public int romanToInt(String s) {
    Map<Character, Integer> map = new HashMap<>();
    map.put('I', 1);
    map.put('V', 5);
    map.put('X', 10);
    map.put('L', 50);
    map.put('C', 100);
    map.put('D', 500);
    map.put('M', 1000);
    int result = 0, last = 1000;
    for (int i = 0; i < s.length(); i++) {
        int value = map.get(s.charAt(i));
        result += value;
        if (value > last) {
            result -= last * 2;
        }
        last = value;
    }
    return result;
}
```

Time Complexity: O(length)

Space Complexity: O(1)