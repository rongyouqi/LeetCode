# Blind 75 Day 6 (String)

* char -> int
    1. all unique characters: `int index = c - 'a';`
    2. parse a string representation of positive integer: `int i = c - '0';`
* int -> char
    1. convert a digit into its string representation: `char c = (char) (digit + '0');`
    2. interpret an integer as ASCII code: `char c = (char) i;`

## 1. Longest Substring Without Repeating Characters (medium)

[LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int result = 0;
    int[] cache = new int[256]; // assumption: all characters are from ACSII
    for (int slow = 0, fast = 0; fast < s.length(); fast++) {
        slow = cache[s.charAt(fast)] > 0 ? Math.max(slow, cache[s.charAt(fast)]) : slow;
        cache[s.charAt(fast)] = fast + 1;
        result = Math.max(result, fast - slow + 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Longest Repeating Character Replacement (medium)

[LeetCode 424](https://leetcode.com/problems/longest-repeating-character-replacement/)

```java
public int characterReplacement(String s, int k) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int[] cache = new int[26]; // assumption: s consists of only uppercase English letters
    int result = 0, maxCount = 0;
    for (int slow = 0, fast = 0; fast < s.length(); fast++) {
        maxCount = Math.max(maxCount, ++cache[s.charAt(fast) - 'A']);
        while (maxCount + k < fast - slow + 1) {
            cache[s.charAt(slow) - 'A']--;
            slow++;
        }
        result = Math.max(result, fast - slow + 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 3. Minimum Window Substring (hard)

[LeetCode 76](https://leetcode.com/problems/minimum-window-substring/)

```java
public String minWindow(String s, String t) {
    if (s == null || t == null) {
        return null;
    }
    if (s.length() == 0 || t.length() == 0) {
        return "";
    }
    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < t.length(); i++) {
        map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) + 1);
    }
    int[] result = {-1, 0, 0};
    int left = 0, right = 0, count = 0;
    Map<Character, Integer> window = new HashMap<>();
    while (right < s.length()) {
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);
        if (map.containsKey(c) && map.get(c).equals(window.get(c))) {
            count++;
        }
        while (left <= right && count == map.size()) {
            c = s.charAt(left);
            if (result[0] == -1 || right - left + 1 < result[0]) {
                result[0] = right - left + 1;
                result[1] = left;
                result[2] = right;
            }
            window.put(c, window.get(c) - 1);
            if (map.containsKey(c) && window.get(c).intValue() < map.get(c).intValue()) {
                count--;
            }
            left++;
        }
        right++;
    }
    return result[0] == -1 ? "" : s.substring(result[1], result[2] + 1);
}
```

Time Complexity: O(m + n)

Space Complexity: O(m + n)

## 4. Valid Anagram (easy)

[LeetCode 242](https://leetcode.com/problems/valid-anagram/)

```java
public boolean isAnagram(String s, String t) {
    // assumption: s and t are not null
    if (s.length() != t.length()) {
        return false;
    }
    int[] cache = new int[256]; // assumption: all characters are from ACSII
    for (int i = 0; i < s.length(); i++) {
        cache[s.charAt(i)]++;
    }
    for (int i = 0; i < t.length(); i++) {
        cache[t.charAt(i)]--;
        if (cache[t.charAt(i)] < 0) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Group Anagrams (medium)

[LeetCode 49](https://leetcode.com/problems/group-anagrams/)

### Solution 1

```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    Map<String, List<String>> result = new HashMap<>();
    for (String s : strs) {
        char[] array = s.toCharArray();
        Arrays.sort(array);
        String key = new String(array);
        if (!result.containsKey(key)) {
            result.put(key, new ArrayList<>());
        }
        result.get(key).add(s);
    }
    return new ArrayList<>(result.values());
}
```

Time Complexity: O(nLlogL)

Space Complexity: O(nL)

### Solution 2

```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    Map<String, List> result = new HashMap<>();
    int[] count = new int[26]; // assumption: strs[i] consists of lowercase English letters
    for (String s : strs) {
        Arrays.fill(count, 0);
        for (char c : s.toCharArray()) {
            count[c - 'a']++;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < 26; i++) {
            sb.append('#');
            sb.append(count[i]);
        }
        String key = sb.toString();
        if (!result.containsKey(key)) {
            result.put(key, new ArrayList());
        }
        result.get(key).add(s);
    }
    return new ArrayList(result.values());
}
```

Time Complexity: O(nL)

Space Complexity: O(nL)

## 6. Valid Parentheses (easy)

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

## 7. Valid Palindrome (easy)

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

## 8. Longest Palindromic Substring (medium)

[LeetCode 5](https://leetcode.com/problems/longest-palindromic-substring/)

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 2) {
        return s;
    }
    char[] array = s.toCharArray();
    int start = 0, end = 0;
    for (int i = 0; i < array.length; i++) {
        int maxLength = Math.max(expand(array, i, i), expand(array, i, i + 1));
        if (end - start < maxLength) {
            start = i - (maxLength - 1) / 2;
            end = i + maxLength / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expand(char[] array, int i, int j) {
    while (i >= 0 && j < array.length && array[i] == array[j]) {
        i--;
        j++;
    }
    return j - i - 1;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

## 9. Palindromic Substrings (medium)

[LeetCode 647](https://leetcode.com/problems/palindromic-substrings/)

```java
public int countSubstrings(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int result = 0;
    for (int i = 0; i < s.length(); i++) {
        int left = i - 1, right = i;
        while (right < s.length() - 1 && s.charAt(right) == s.charAt(right + 1)) {
            right++;
        }
        result += (right - left) * (right - left + 1) / 2;
        i = right++;
        while (left >= 0 && right < s.length() && s.charAt(left--) == s.charAt(right++)) {
            result++;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 10. Encode and Decode Strings (medium)

[LeetCode 271](https://leetcode.com/problems/encode-and-decode-strings/)

```java
public String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for (String s : strs) {
        sb.append(s.replace("#", "##")).append(" # ");
    }
    sb.deleteCharAt(sb.length() - 1);
    return sb.toString();
}

public List<String> decode(String s) {
    List<String> result = new ArrayList<>();
    String[] array = s.split(" # ", -1);
    for (String str : array) {
        result.add(str.replace("##", "#"));
    }
    return result;
}
```

### Solution 1: non-ASCII delimiter

```java
public String encode(List<String> strs) {
    if (strs.size() == 0) {
        return Character.toString((char)258);
    }
    String delimiter = Character.toString((char)257);
    StringBuilder sb = new StringBuilder();
    for (String str : strs) {
        sb.append(str).append(delimiter);
    }
    sb.deleteCharAt(sb.length() - 1);
    return sb.toString();
}

public List<String> decode(String s) {
    if (s.equals(Character.toString((char)258))) {
        return new ArrayList();
    }
    String delimiter = Character.toString((char)257);
    return Arrays.asList(s.split(delimiter, -1));
}
```

### Solution 2: chunked transfer encoding

```java
public String encode(List<String> strs) {
    StringBuilder sb = new StringBuilder();
    for (String str : strs) {
        sb.append(lenToString(str)).append(str);
    }
    return sb.toString();
}

private String lenToString(String s) {
    int len = s.length();
    char[] result = new char[4];
    for (int i = 3; i >= 0; i--) {
        result[3 - i] = (char) (len >> (i * 8) & 0xff); // ???
    }
    return new String(result);
}

public List<String> decode(String s) {
    List<String> result = new ArrayList<>();
    int i = 0;
    while (i < s.length()) {
        int len = stringToInt(s.substring(i, i + 4));
        i += 4;
        result.add(s.substring(i, i + len));
        i += len;
    }
    return result;
}

private int stringToInt(String s) {
    int result = 0;
    for (char c : s.toCharArray()) {
        result = (result << 8) + (int)c; // ???
    }
    return result;
}
```

