# Medium Collection Day 2 (Backtracking)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/109/backtracking/

## 1. Letter Combinations of a Phone Number (medium)

[LeetCode 17](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```java
public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    if (digits == null || digits.length() == 0) {
        return result;
    }
    StringBuilder sb = new StringBuilder();
    Map<Character, String> map = new HashMap<>();
    map.put('2', "abc");
    map.put('3', "def");
    map.put('4', "ghi");
    map.put('5', "jkl");
    map.put('6', "mno");
    map.put('7', "pqrs");
    map.put('8', "tuv");
    map.put('9', "wxyz");
    helper(map, digits, 0, sb, result);
    return result;
}

private void helper(Map<Character, String> map, String digits, int index, StringBuilder sb, List<String> result) {
    if (index == digits.length()) {
        result.add(sb.toString());
        return;
    }
    char c = digits.charAt(index);
    for (int i = 0; i < map.get(c).length(); i++) {
        sb.append(map.get(c).charAt(i));
        helper(map, digits, index + 1, sb, result);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

Time Complexity: O(4<sup>n</sup>)

Space Complexity: O(n)

## 2. Generate Parentheses (medium)

[LeetCode 22](https://leetcode.com/problems/generate-parentheses/)

```java
public List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    StringBuilder sb = new StringBuilder();
    helper(n, 0, 0, sb, result);
    return result;
}

private void helper(int n, int left, int right, StringBuilder sb, List<String> result) {
    if (left + right == 2 * n) {
        result.add(sb.toString());
        return;
    }
    if (left < n) {
        sb.append('(');
        helper(n, left + 1, right, sb, result);
        sb.deleteCharAt(sb.length() - 1);
    }
    if (right < left) {
        sb.append(')');
        helper(n, left, right + 1, sb, result);
        sb.deleteCharAt(sb.length() - 1);
    }
}
```

Time Complexity: O(2<sup>n</sup>)

Space Complexity: O(n)

## 3. Permutations (medium)

[LeetCode 46](https://leetcode.com/problems/permutations/)

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }
    helper(nums, 0, result);
    return result;
}

private void helper(int[] nums, int index, List<List<Integer>> result) {
    if (index == nums.length) {
        List<Integer> solution = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            solution.add(nums[i]);
        }
        result.add(solution);
        return;
    }
    for (int i = index; i < nums.length; i++) {
        swap(nums, index, i);
        helper(nums, index + 1, result);
        swap(nums, index, i);
    }
}

private void swap(int[] nums, int x, int y) {
    int temp = nums[x];
    nums[x] = nums[y];
    nums[y] = temp;
}
```

Time Complexity: O(n! * n)

Space Complexity: O(n)

## 4. Subsets (medium)

[LeetCode 78](https://leetcode.com/problems/subsets/)

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    if (nums == null || nums.length == 0) {
        return result;
    }
    List<Integer> solution = new ArrayList<>();
    helper(nums, 0, solution, result);
    return result;
}

private void helper(int[] nums, int index, List<Integer> solution, List<List<Integer>> result) {
    if (index == nums.length) {
        result.add(new ArrayList<>(solution));
        return;
    }
    // case 1
    helper(nums, index + 1, solution, result);
    // case 2
    solution.add(nums[index]);
    helper(nums, index + 1, solution, result);
    solution.remove(solution.size() - 1);
}
```

Time Complexity: O(2<sup>n</sup>)

Space Complexity: O(n)

## 5. Word Search (medium)

[LeetCode 79](https://leetcode.com/problems/word-search/)

```java
public boolean exist(char[][] board, String word) {
    if (board == null) {
        return false;
    }
    if (word == null || word.length() == 0) {
        return true;
    }
    boolean[][] visited = new boolean[board.length][board[0].length];
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            if (board[i][j] == word.charAt(0) && helper(board, word, i, j, 0, visited)) {
                return true;
            }
        }
    }
    return false;
}

private boolean helper(char[][] board, String word, int i, int j, int index, boolean[][] visited) {
    if (index == word.length()) {
        return true;
    }
    if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(index) || visited[i][j]) {
        return false;
    }
    visited[i][j] = true;
    if (helper(board, word, i - 1, j, index + 1, visited) || helper(board, word, i + 1, j, index + 1, visited) || helper(board, word, i, j - 1, index + 1, visited) || helper(board, word, i, j + 1, index + 1, visited)) {
        return true;
    }
    visited[i][j] = false;
    return false;
}
```

Time Complexity: O(mn*3<sup>L</sup>)

Space Complexity: O(L)