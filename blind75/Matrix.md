# Blind 75 Day 5 (Matrix)

## 1. [LeetCode 73](https://leetcode.com/problems/set-matrix-zeroes/) Set Matrix Zeros (medium)

- Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.
- You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
    - **Output:** `[[1,0,1],[0,0,0],[1,0,1]]`
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
    - **Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`
- **Constraints:**
    -   `m == matrix.length`
    -   `n == matrix[0].length`
    -   `1 <= m, n <= 200`
    -   `-2^31 <= matrix[i][j] <= 2^31 - 1`
- **Follow up:**
    -   A straightforward solution using `O(mn)` space is probably a bad idea.
    -   A simple improvement uses `O(m + n)` space, but still not the best solution.
    -   Could you devise a constant space solution?

### Solution

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

## 2. [LeetCode 54](https://leetcode.com/problems/spiral-matrix/) Spiral Matrix (medium)

- Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[1,2,3],[4,5,6],[7,8,9]]`
    - **Output:** [1,2,3,6,9,8,7,4,5]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[1,2,3,4],[5,6,7,8],[9,10,11,12]]`
    - **Output:** [1,2,3,4,8,12,11,10,9,5,6,7]
- **Constraints:**
    -   `m == matrix.length`
    -   `n == matrix[i].length`
    -   `1 <= m, n <= 10`
    -   `-100 <= matrix[i][j] <= 100`

### Solution

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

## 3. [LeetCode 48](https://leetcode.com/problems/rotate-image/) Rotate Image (medium)

- You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).
- You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[1,2,3],[4,5,6],[7,8,9]]`
    - **Output:** `[[7,4,1],[8,5,2],[9,6,3]]`
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg" style="zoom:67%;" />
    - **Input:** matrix = `[[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]`
    - **Output:** `[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]`
- **Constraints:**
    -   `n == matrix.length == matrix[i].length`
    -   `1 <= n <= 20`
    -   `-1000 <= matrix[i][j] <= 1000`

### Solution

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

Time Complexity: O(n^2)

Space Complexity: O(1)

## 4. [LeetCode 79](https://leetcode.com/problems/word-search/) Word Search (medium)

- Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.
- The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/04/word2.jpg" style="zoom:67%;" />
    - **Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "ABCCED"
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg" style="zoom:67%;" />
    - **Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "SEE"
    - **Output:** true
- **Example 3:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/15/word3.jpg" style="zoom:67%;" />
    - **Input:** board = `[["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, word = "ABCB"
    - **Output:** false
- **Constraints:**
    -   `m == board.length`
    -   `n = board[i].length`
    -   `1 <= m, n <= 6`
    -   `1 <= word.length <= 15`
    -   `board` and `word` consists of only lowercase and uppercase English letters.
- **Follow up:** Could you use search pruning to make your solution faster with a larger `board`?

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

Time Complexity: O(mn * 3^L)

Space Complexity: O(L)