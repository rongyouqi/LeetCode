# Easy Collection Day 2 (Strings)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/127/strings/

## 1. Reverse String (easy)

[LeetCode 344](https://leetcode.com/problems/reverse-string/)

```java
public void reverseString(char[] s) {
    if (s == null || s.length <= 1) {
        return;
    }
    int left = 0, right = s.length - 1;
    while (left < right) {
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        left++;
        right--;
    }
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Reverse Integer (medium)

[LeetCode 7](https://leetcode.com/problems/reverse-integer/)

```java
// Integer.MAX_VALUE = 2147483647
// Integer.MIN_VALUE = -2147483648

public int reverse(int x) {
    int result = 0;
    while (x != 0) {
        int digit = x % 10;
        x /= 10;
        if (result > Integer.MAX_VALUE / 10 || (result == Integer.MAX_VALUE / 10 && digit > 7)) {
            return 0;
        }
        if (result < Integer.MIN_VALUE / 10 || (result == Integer.MIN_VALUE / 10 && digit < -8)) {
            return 0;
        }
        result = result * 10 + digit;
    }
    return result;
}
```

Time Complexity: O(log<sub>10</sub>x)

Space Complexity: O(1)

## 3. First Unique Character in String (easy)

[LeetCode 387](https://leetcode.com/problems/first-unique-character-in-a-string/)

```java
public int firstUniqChar(String s) {
    // assumption: s consists of only lowercase English letters
    if (s == null || s.length() == 0) {
        return -1;
    }
    int[] count = new int[26];
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
    }
    for (int i = 0; i < s.length(); i++) {
        if (count[s.charAt(i) - 'a'] == 1) {
            return i;
        }
    }
    return -1;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 4. Valid Anagram (easy)

[LeetCode 242](https://leetcode.com/problems/valid-anagram/)

```java
public boolean isAnagram(String s, String t) {
    // assumption 1: s and t are not null
    // assumption 2: consist of only lowercase English letters
    if (s.length() != t.length()) {
        return false;
    }
    int[] count = new int[26];
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
        count[t.charAt(i) - 'a']--;
    }
    for (int num : count) {
        if (num != 0) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Valid Palindrome (easy)

[LeetCode 125](https://leetcode.com/problems/valid-palindrome/)

```java
public boolean isPalindrome(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }
    for (int left = 0, right = s.length() - 1; left < right; left++, right--) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. String to Integer (atoi) (medium)

[LeetCode 8](https://leetcode.com/problems/string-to-integer-atoi/)

```java
public int myAtoi(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int sign = 1, base = 0, i = 0;
    while (i < s.length() && s.charAt(i) == ' ') {
        i++;
    }
    if (i < s.length() && (s.charAt(i) == '-' || s.charAt(i) == '+')) {
        sign = s.charAt(i++) == '-' ? -1 : 1;
    }
    while (i < s.length() && s.charAt(i) >= '0' && s.charAt(i) <= '9') {
        if (base > Integer.MAX_VALUE / 10 || (base == Integer.MAX_VALUE / 10 && s.charAt(i) - '0' > 7)) {
            return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
        }
        base = base * 10 + (s.charAt(i++) - '0');
    }
    return sign * base;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 7. Implement `strStr()` (easy)

[LeetCode 28](https://leetcode.com/problems/implement-strstr/)

```java
public int strStr(String haystack, String needle) {
    // assumption: haystack and needle are not null
    if (needle.length() == 0) {
        return 0;
    }
    for (int i = 0; i <= haystack.length() - needle.length(); i++) {
        for (int j = 0; j < needle.length() && hayStack.charAt(i + j) == needle.charAt(j); j++) {
            if (j == needle.length() - 1) {
                return i;
            }
        }
    }
    return -1;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 8. Longest Common Prefix (easy)

[LeetCode 14](https://leetcode.com/problems/longest-common-prefix/)

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    for (int i = 0; i < strs[0].length(); i++) {
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

Time Complexity: O(sum of string length)

Space Complexity: O(1)