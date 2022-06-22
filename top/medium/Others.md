# Medium Collection Day 4 (Others)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/114/others/

## 1. Sum of Two Integers (medium)

[LeetCode 371](https://leetcode.com/problems/sum-of-two-integers/)

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

## 2. Evaluate Reverse Polish Notation (medium)

[LeetCode 150](https://leetcode.com/problems/evaluate-reverse-polish-notation/)

```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String token : tokens) {
        if (!"+-*/".contains(token)) {
            stack.offerFirst(Integer.valueOf(token));
            continue;
        }
        int num2 = stack.pollFirst();
        int num1 = stack.pollFirst();
        int result = 0;
        switch (token) {
            case "+":
                result = num1 + num2;
                break;
            case "-":
                result = num1 - num2;
                break;
            case "*":
                result = num1 * num2;
                break;
            case "/":
                result = num1 / num2;
                break;
        }
        stack.offerFirst(result);
    }
    return stack.peekFirst();
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 3. Majority Element (easy)

[LeetCode 169](https://leetcode.com/problems/majority-element/)

```java
public int majorityElement(int[] nums) {
    int count = 0;
    Integer candidate = null;
    for (int num : nums) {
        if (count == 0) {
            candidate = num;
        }
        count += num == candidate ? 1 : -1;
    }
    return candidate;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. Find the Celebrity (medium)

[LeetCode 277](https://leetcode.com/problems/find-the-celebrity/)

```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        int candidate = 0;
        for (int i = 0; i < n; i++) {
            if (knows(candidate, i)) {
                candidate = i;
            }
        }
        if (isCelebrity(n, candidate)) {
            return candidate;
        }
        return -1;
    }
    
    private boolean isCelebrity(int n, int i) {
        for (int j = 0; j < n; j++) {
            if (i == j) {
                continue;
            }
            if (knows(i, j) || !knows(j, i)) {
                return false;
            }
        }
        return true;
    }
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Task Scheduler (medium)

[LeetCode 621](https://leetcode.com/problems/task-scheduler/)

```java
public int leastInterval(char[] tasks, int n) {
    int[] frequencies = new int[26];
    for (char task : tasks) {
        frequencies[task - 'A']++;
    }
    int max = 0;
    for (int freq : frequencies) {
        max = Math.max(max, freq);
    }
    int count = 0;
    for (int freq : frequencies) {
        if (freq == max) {
            count++;
        }
    }
    return Math.max(tasks.length, (max - 1) * (n + 1) + count);
}
```

Time Complexity: O(n)

Space Complexity: O(1)