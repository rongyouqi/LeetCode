# Blind 75 Day 5 (Matrix)

## 1. Set Matrix Zeros (medium)

[LeetCode 73](https://leetcode.com/problems/set-matrix-zeroes/)

```java
public void setZeroes(int[][] matrix) {
  if (matrix == null || matrix.length == 0) {
    return;
  }
  int k = 0;
  while (k < matrix[0].length && matrix[0][k] != 0) {
    k++;
  }
  for (int i = 1; i < matrix.length; i++) {
    for (int j = 0; j < matrix[0].length; j++) {
      if (matrix[i][j] == 0) {
        matrix[i][0] = 0;
        matrix[0][j] = 0;
      }
    }
  }
  for (int i = 1; i < matrix.length; i++) {
    for (int j = matrix[0].length - 1; j >= 0; j--) {
      if (matrix[i][0] == 0 || matrix[0][j] == 0) {
        matrix[i][j] = 0;
      }
    }
  }
  if (k < matrix[0].length) {
    Arrays.fill(matrix[0], 0);
  }
  return;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 2. Spiral Matrix (medium)

[LeetCode 54](https://leetcode.com/problems/spiral-matrix/)

```java
public List<Integer> spiralOrder(int[][] matrix) {
  List<Integer> result = new ArrayList<>();
  if (matrix == null || matrix.length == 0) {
    return result;
  }
  int m = matrix.length;
  int n = matrix[0].length;
  int left = 0, right = n - 1;
  int up = 0, down = m - 1;
  while (left < right && up < down) {
    for (int i = left; i <= right; i++) {
      result.add(matrix[up][i]);
    }
    for (int i = up + 1; i <= down - 1; i++) {
      result.add(matrix[i][right]);
    }
    for (int i = right; i >= left; i--) {
      result.add(matrix[down][i]);
    }
    for (int i = down - 1; i >= up + 1; i--) {
      result.add(matrix[i][left]);
    }
    left++;
    right--;
    up++;
    down--;
  }
  if (left == right) {
    for (int i = up; i <= down; i++) {
      result.add(matrix[i][left]);
    }
  } else if (up == down) {
    for (int i = left; i <= right; i++) {
      result.add(matrix[up][i]);
    }
  }
  return result;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 3. Rotate Image (medium)

[LeetCode 48](https://leetcode.com/problems/rotate-image/)

```java
public void rotate(int[][] matrix) {
  if (matrix == null || matrix.length == 0) {
    return;
  }
  for (int i = 0; i < matrix.length / 2; i++) {
    int[] temp = matrix[i];
    matrix[i] = matrix[matrix.length - 1 - i];
    matrix[matrix.length - 1 - i] = temp;
  }
  for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < i; j++) {
      int temp = matrix[i][j];
      matrix[i][j] = matrix[j][i];
      matrix[j][i] = temp;
    }
  }
  return;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

## 4. Word Search (medium)

[LeetCode 79](https://leetcode.com/problems/word-search/)

### Solution: backtracking

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