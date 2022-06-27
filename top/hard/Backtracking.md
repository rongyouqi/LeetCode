# Hard Collection Day 4 (Backtracking)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/119/backtracking/

## 1. [LeetCode 131](https://leetcode.com/problems/palindrome-partitioning/) Palindrome Partitioning (medium)

- Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.
- A **palindrome** string is a string that reads the same backward as forward.
- **Example 1:**
    - **Input:** s = "aab"
    - **Output:** `[["a","a","b"],["aa","b"]]`
- **Example 2:**
    - **Input:** s = "a"
    - **Output:** `[["a"]]`
- **Constraints:**
    - `1 <= s.length <= 16`
    - `s` contains only lowercase English letters.

### Solution 1

```java
public List<List<String>> partition(String s) {
    List<List<String>> result = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return result;
    }
    boolean[][] memo = new boolean[s.length()][s.length()];
    List<String> solution = new ArrayList<>();
    helper(s, 0, solution, memo, result);
    return result;
}

private void helper(String s, int index, List<String> solution, boolean[][] memo, List<List<String>> result) {
    if (index == s.length()) {
        result.add(new ArrayList<>(solution));
    }
    for (int i = index; i < s.length(); i++) {
        if (s.charAt(index) == s.charAt(i) && (i - index <= 2 || memo[index + 1][i - 1])) {
            memo[index][i] = true;
            solution.add(s.substring(index, i + 1));
            helper(s, i + 1, solution, memo, result);
            solution.remove(solution.size() - 1);
        }
    }
}
```

Time Complexity: O(n * 2^n) // substring takes O(n) time!

Space Complexity: O(n^2)

### Solution 2

```java
public List<List<String>> partition(String s) {
    List<List<String>> result = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return result;
    }
    boolean[][] memo = new boolean[s.length()][s.length()];
    for (int i = 0; i < s.length(); i++) {
        for (int j = 0; j <= i; j++) {
            if (s.charAt(i) == s.charAt(j) && (i - j <= 2 || memo[j + 1][i - 1])) {
                memo[j][i] = true;
            }
        }
    }
    List<String> solution = new ArrayList<>();
    helper(s, 0, solution, memo, result);
    return result;
}

private void helper(String s, int index, List<String> solution, boolean[][] memo, List<List<String>> result) {
    if (index == s.length()) {
        result.add(new ArrayList<>(solution));
    }
    for (int i = index; i < s.length(); i++) {
        if (memo[index][i]) {
            solution.add(s.substring(index, i + 1));
            helper(s, i + 1, solution, memo, result);
            solution.remove(solution.size() - 1);
        }
    }
}
```

Time Complexity: O(n * 2^n) // substring takes O(n) time!

Space Complexity: O(n^2)

## 2. [LeetCode 212](https://leetcode.com/problems/word-search-ii/) Word Search II (hard)

- Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.
- Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)
    - **Input:** board = `[["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]]`, words = ["oath","pea","eat","rain"]
    - **Output:** ["eat","oath"]
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2020/11/07/search2.jpg)
    - **Input:** board = `[["a","b"],["c","d"]]`, words = ["abcb"]
    - **Output:** []
- **Constraints:**
    -   `m == board.length`
    -   `n == board[i].length`
    -   `1 <= m, n <= 12`
    -   `board[i][j]` is a lowercase English letter.
    -   `1 <= words.length <= 3 * 10^4`
    -   `1 <= words[i].length <= 10`
    -   `words[i]` consists of lowercase English letters.
    -   All the strings of `words` are unique.

### Solution: trie

```java
class Solution {
    static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        String word;
    }
    
    public List<String> findWords(char[][] board, String[] words) {
        List<String> result = new ArrayList<>();
        TrieNode root = new TrieNode();
        for (String word : words) {
            TrieNode current = root;
            for (char c : word.toCharArray()) {
                int index = c - 'a';
                if (current.children[index] == null) {
                    current.children[index] = new TrieNode();
                }
                current = current.children[index];
            }
            current.word = word;
        }
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                helper(board, i, j, root, result);
            }
        }
        return result;
    }
    
    private void helper(char[][] board, int i, int j, TrieNode node, List<String> result) {
        char c = board[i][j];
        if (c == '#' || node.children[c - 'a'] == null) {
            return;
        }
        node = node.children[c - 'a'];
        if (node.word != null) {
            result.add(node.word);
            node.word = null;
        }
        board[i][j] = '#';
        if (i > 0) {
            helper(board, i - 1, j, node, result);
        }
        if (i < board.length - 1) {
            helper(board, i + 1, j, node, result);
        }
        if (j > 0) {
            helper(board, i, j - 1, node, result);
        }
        if (j < board[0].length - 1) {
            helper(board, i, j + 1, node, result);
        }
        board[i][j] = c;
    }
}
```

Time Complexity: O(mn * 4^L)

Space Complexity: O(wL)

## 3. [LeetCode 301](https://leetcode.com/problems/remove-invalid-parentheses/) Remove Invalid Parentheses (hard)

- Given a string `s` that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.
- Return _all the possible results_. You may return the answer in **any order**.
- **Example 1:**
    - **Input:** s = "()())()"
    - **Output:** ["(())()","()()()"]
- **Example 2:**
    - **Input:** s = "(a)())()"
    - **Output:** ["(a())()","(a)()()"]
- **Example 3:**
    - **Input:** s = ")("
    - **Output:** [""]
- **Constraints:**
    -   `1 <= s.length <= 25`
    -   `s` consists of lowercase English letters and parentheses `'('` and `')'`.
    -   There will be at most `20` parentheses in `s`.

### Solution

```java
public List<String> removeInvalidParentheses(String s) {
    List<String> result = new ArrayList<>();
    if (s == null || s.length() == 0) {
        return result;
    }
    helper(s, 0, 0, result, new char[]{'(', ')'});
    return result;
}

private void helper(String s, int last_i, int last_j, List<String> result, char[] par) {
    int i = last_i, count = 0;
    while (i < s.length() && count >= 0) {
        if (s.charAt(i) == par[0]) {
            count++;
        }
        if (s.charAt(i) == par[1]) {
            count--;
        }
        i++;
    }
    if (i == s.length() && count >= 0) {
        String reversed = new StringBuilder(s).reverse().toString();
        if (par[0] == '(') {
            helper(reversed, 0, 0, result, new char[]{')', '('});
        } else {
            result.add(reversed);
        }
        return;
    }
    i--;
    for (int j = last_j; j <= i; j++) {
        if (s.charAt(j) == par[1] && (j == 0 || s.charAt(j - 1) != par[1])) {
            helper(s.substring(0, j) + s.substring(j + 1, s.length()), i, j, result, par);
        }
    }
}
```

Time Complexity: O(nk) // n == s.length(), k == number of result

Space Complexity: O(k)

## 4. [LeetCode 44](https://leetcode.com/problems/wildcard-matching/) Wildcard Matching (hard)

- Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:
    -   `'?'` Matches any single character.
    -   `'*'` Matches any sequence of characters (including the empty sequence).
- The matching should cover the **entire** input string (not partial).
- **Example 1:**
    - **Input:** s = "aa", p = "a"
    - **Output:** false
    - **Explanation:** "a" does not match the entire string "aa".
- **Example 2:**
    - **Input:** s = "aa", p = "*"
    - **Output:** true
    - **Explanation:** '*' matches any sequence.
- **Example 3:**
    - **Input:** s = "cb", p = "?a"
    - **Output:** false
    - **Explanation:** '?' matches 'c', but the second letter is 'a', which does not match 'b'.
- **Constraints:**
    -   `0 <= s.length, p.length <= 2000`
    -   `s` contains only lowercase English letters.
    -   `p` contains only lowercase English letters, `'?'` or `'*'`.

### Solution 1: dynamic programming

```java
public boolean isMatch(String s, String p) {
    boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
    dp[0][0] = true;
    for (int j = 1; j <= p.length(); j++) {
        if (p.charAt(j - 1) == '*') {
            dp[0][j] = dp[0][j - 1];
        }
    }
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 1; j <= p.length(); j++) {
            if (p.charAt(j - 1) == '*') {
                dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
            } else if (p.charAt(j - 1) == '?' || s.charAt(i - 1) == p.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = false;
            }
        }
    }
    return dp[s.length()][p.length()];
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

### Solution 2

```java
public boolean isMatch(String s, String p) {
    int i = 0, j = 0, index1 = -1, index2 = -1;
    while (i < s.length()) {
        if (j < p.length() && (p.charAt(j) == '?' || s.charAt(i) == p.charAt(j))) {
            i++;
            j++;
        } else if (j < p.length() && p.charAt(j) == '*') {
            index1 = i;
            index2 = j;
            j++;
        } else if (index2 == -1) {
            return false;
        } else {
            i = index1 + 1;
            j = index2 + 1;
            index1 = i;
        }
    }
    for (; j < p.length(); j++) {
        if (p.charAt(j) != '*') {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(max(m, n))

Space Complexity: O(1)

## 5. [LeetCode 10](https://leetcode.com/problems/regular-expression-matching/) Regular Expression Matching (hard)

- Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:
    -   `'.'` Matches any single character.​​​​
    -   `'*'` Matches zero or more of the preceding element.
- The matching should cover the **entire** input string (not partial).
- **Example 1:**
    - **Input:** s = "aa", p = "a"
    - **Output:** false
    - **Explanation:** "a" does not match the entire string "aa".
- **Example 2:**
    - **Input:** s = "aa", p = "a*"
    - **Output:** true
    - **Explanation:** '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
- **Example 3:**
    - **Input:** s = "ab", p = ".*"
    - **Output:** true
    - **Explanation:** ".*" means "zero or more (*) of any character (.)".
- **Constraints:**
    -   `1 <= s.length <= 20`
    -   `1 <= p.length <= 30`
    -   `s` contains only lowercase English letters.
    -   `p` contains only lowercase English letters, `'.'`, and `'*'`.
    -   It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

### Solution: dynamic programming

```java
public boolean isMatch(String s, String p) {
    boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
    dp[0][0] = true;
    for (int j = 0; j < p.length(); j++) {
        if (p.charAt(j) == '*' && dp[0][j - 1]) {
            dp[0][j + 1] = true;
        }
    }
    for (int i = 0; i < s.length(); i++) {
        for (int j = 0; j < p.length(); j++) {
            if (p.charAt(j) == '.' || s.charAt(i) == p.charAt(j)) {
                dp[i + 1][j + 1] = dp[i][j];
            } else if (p.charAt(j) == '*') {
                if (p.charAt(j - 1) != s.charAt(i) && p.charAt(j - 1) != '.') {
                    dp[i + 1][j + 1] = dp[i + 1][j - 1];
                } else {
                    dp[i + 1][j + 1] = dp[i + 1][j] || dp[i][j + 1] || dp[i + 1][j - 1];
                }
            }
        }
    }
    return dp[s.length()][p.length()];
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

