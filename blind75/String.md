# Blind 75 Day 6 (String)

* char -> int
    1. all unique characters: `int index = c - 'a';`
    2. parse a string representation of positive integer: `int i = c - '0';`
* int -> char
    1. convert a digit into its string representation: `char c = (char) (digit + '0');`
    2. interpret an integer as ASCII code: `char c = (char) i;`

## 1. [LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) Longest Substring Without Repeating Characters (medium)

- Given a string `s`, find the length of the **longest substring** without repeating characters.
- **Example 1:**
    - **Input:** s = "abcabcbb"
    - **Output:** 3
    - **Explanation:** The answer is "abc", with the length of 3.
- **Example 2:**
    - **Input:** s = "bbbbb"
    - **Output:** 1
    - **Explanation:** The answer is "b", with the length of 1.
- **Example 3:**
    - **Input:** s = "pwwkew"
    - **Output:** 3
    - **Explanation:** The answer is "wke", with the length of 3.
        - Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
- **Constraints:**
    -   `0 <= s.length <= 5 * 10^4`
    -   `s` consists of English letters, digits, symbols and spaces.

### Solution

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

## 2. [LeetCode 424](https://leetcode.com/problems/longest-repeating-character-replacement/) Longest Repeating Character Replacement (medium)

- You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.
- Return _the length of the longest substring containing the same letter you can get after performing the above operations_.
- **Example 1:**
    - **Input:** s = "ABAB", k = 2
    - **Output:** 4
    - **Explanation:** Replace the two 'A's with two 'B's or vice versa.
- **Example 2:**
    - **Input:** s = "AABABBA", k = 1
    - **Output:** 4
    - **Explanation:** Replace the one 'A' in the middle with 'B' and form "AABBBBA". The substring "BBBB" has the longest repeating letters, which is 4.
- **Constraints:**
    -   `1 <= s.length <= 10^5`
    -   `s` consists of only uppercase English letters.
    -   `0 <= k <= s.length`

### Solution

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

## 3. [LeetCode 76](https://leetcode.com/problems/minimum-window-substring/) Minimum Window Substring (hard)

- Given two strings `s` and `t` of lengths `m` and `n` respectively, return _the **minimum window substring** of_ `s` _such that every character in_ `t` _(**including duplicates**) is included in the window. If there is no such substring__, return the empty string_ `""`_._
- The testcases will be generated such that the answer is **unique**.
- A **substring** is a contiguous sequence of characters within the string.
- **Example 1:**
    - **Input:** s = "ADOBECODEBANC", t = "ABC"
    - **Output:** "BANC"
    - **Explanation:** The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
- **Example 2:**
    - **Input:** s = "a", t = "a"
    - **Output:** "a"
    - **Explanation:** The entire string s is the minimum window.
- **Example 3:**
    - **Input:** s = "a", t = "aa"
    - **Output:** ""
    - **Explanation:** Both 'a's from t must be included in the window. Since the largest window of s only has one 'a', return empty string.
- **Constraints:**
    -   `m == s.length`
    -   `n == t.length`
    -   `1 <= m, n <= 10^5`
    -   `s` and `t` consist of uppercase and lowercase English letters.
- **Follow up:** Could you find an algorithm that runs in `O(m + n)` time?

### Solution

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

## 4. [LeetCode 242](https://leetcode.com/problems/valid-anagram/) Valid Anagram (easy)

- Given two strings `s` and `t`, return `true` _if_ `t` _is an anagram of_ `s`_, and_ `false` _otherwise_.
- An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
- **Example 1:**
    - **Input:** s = "anagram", t = "nagaram"
    - **Output:** true
- **Example 2:**
    - **Input:** s = "rat", t = "car"
    - **Output:** false
- **Constraints:**
    -   `1 <= s.length, t.length <= 5 * 10^4`
    -   `s` and `t` consist of lowercase English letters.
- **Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

### Solution

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

## 5. [LeetCode 49](https://leetcode.com/problems/group-anagrams/) Group Anagrams (medium)

- Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.
- An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
- **Example 1:**
    - **Input:** strs = ["eat","tea","tan","ate","nat","bat"]
    - **Output:** `[["bat"],["nat","tan"],["ate","eat","tea"]]`
- **Example 2:**
    - **Input:** strs = [""]
    - **Output:** `[[""]]`
- **Example 3:**
    - **Input:** strs = ["a"]
    - **Output:** `[["a"]]`
- **Constraints:**
    -   `1 <= strs.length <= 10^4`
    -   `0 <= strs[i].length <= 100`
    -   `strs[i]` consists of lowercase English letters.

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

## 6. [LeetCode 20](https://leetcode.com/problems/valid-parentheses/) Valid Parentheses (easy)

- Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['`and `']'`, determine if the input string is valid.
- An input string is valid if:
    1.  Open brackets must be closed by the same type of brackets.
    2.  Open brackets must be closed in the correct order.
- **Example 1:**
    - **Input:** s = `"()"`
    - **Output:** true
- **Example 2:**
    - **Input:** s = `"()[]{}"`
    - **Output:** true
- **Example 3:**
    - **Input:** s = `"(]"`
    - **Output:** false
- **Constraints:**
    -   `1 <= s.length <= 10^4`
    -   `s` consists of parentheses only `'()[]{}'`.

### Solution

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

## 7. [LeetCode 125](https://leetcode.com/problems/valid-palindrome/) Valid Palindrome (easy)

- A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.
- Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.
- **Example 1:**
    - **Input:** s = "A man, a plan, a canal: Panama"
    - **Output:** true
    - **Explanation:** "amanaplanacanalpanama" is a palindrome.
- **Example 2:**
    - **Input:** s = "race a car"
    - **Output:** false
    - **Explanation:** "raceacar" is not a palindrome.
- **Example 3:**
    - **Input:** s = " "
    - **Output:** true
    - **Explanation:** s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.
- **Constraints:**
    -   `1 <= s.length <= 2 * 10^5`
    -   `s` consists only of printable ASCII characters.

### Solution

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

## 8. [LeetCode 5](https://leetcode.com/problems/longest-palindromic-substring/) Longest Palindromic Substring (medium)

- Given a string `s`, return _the longest palindromic substring_ in `s`.
- **Example 1:**
    - **Input:** s = "babad"
    - **Output:** "bab"
    - **Explanation:** "aba" is also a valid answer.
- **Example 2:**
    - **Input:** s = "cbbd"
    - **Output:** "bb"
- **Constraints:**
    -   `1 <= s.length <= 1000`
    -   `s` consist of only digits and English letters.

### Solution: expand from center

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

Time Complexity: O(n^2)

Space Complexity: O(1)

## 9. [LeetCode 647](https://leetcode.com/problems/palindromic-substrings/) Palindromic Substrings (medium)

- Given a string `s`, return _the number of **palindromic substrings** in it_.
- A string is a **palindrome** when it reads the same backward as forward.
- A **substring** is a contiguous sequence of characters within the string.
- **Example 1:**
    - **Input:** s = "abc"
    - **Output:** 3
    - **Explanation:** Three palindromic strings: "a", "b", "c".
- **Example 2:**
    - **Input:** s = "aaa"
    - **Output:** 6
    - **Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
- **Constraints:**
    -   `1 <= s.length <= 1000`
    -   `s` consists of lowercase English letters.

### Solution

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

## 10. [LeetCode 271](https://leetcode.com/problems/encode-and-decode-strings/) Encode and Decode Strings (medium)

- Design an algorithm to encode **a list of strings** to **a string**. The encoded string is then sent over the network and is decoded back to the original list of strings.
- Machine 1 (sender) has the function:
    ```
    string encode(vector<string> strs) {
      // ... your code
      return encoded_string;
    }
    ```
- Machine 2 (receiver) has the function:
    ```
    vector<string> decode(string s) {
      //... your code
      return strs;
    }
    ```
- So Machine 1 does: `string encoded_string = encode(strs);` and Machine 2 does: `vector<string> strs2 = decode(encoded_string);`
- `strs2` in Machine 2 should be the same as `strs` in Machine 1.
- Implement the `encode` and `decode`methods.
- You are not allowed to solve the problem using any serialize methods (such as `eval`).
- **Example 1:**
    - **Input:** dummy_input = ["Hello","World"]
    - **Output:** ["Hello","World"]
    - **Explanation:**
        - Machine 1:
        ```
        Codec encoder = new Codec();
        String msg = encoder.encode(strs);
        ```
        - Machine 1 ---msg---> Machine 2
        - Machine 2:
        ```
        Codec decoder = new Codec();
        String[] strs = decoder.decode(msg);
        ```
- **Example 2:**
    - **Input:** dummy_input = [""]
    - **Output:** [""]
- **Constraints:**
    -   `1 <= strs.length <= 200`
    -   `0 <= strs[i].length <= 200`
    -   `strs[i]` contains any possible characters out of `256` valid ASCII characters.
- **Follow up:** Could you write a generalized algorithm to work on any possible set of characters?

### Solution

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

