# Hard Collection Day 3 (Trees and Graphs)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/118/trees-and-graphs/

## 1. Word Ladder (hard)

[LeetCode 127](https://leetcode.com/problems/word-ladder/)

```java
public int ladderLength(String beginWord, String endWord, List<String> wordList) {
    int len = beginWord.length();
    Map<String, List<String>> map = new HashMap<>();
    for (String word : wordList) {
        for (int i = 0; i < len; i++) {
            String newWord = word.substring(0, i) + '*' + word.substring(i + 1, len);
            List<String> list = map.getOrDefault(newWord, new ArrayList<>());
            list.add(word);
            map.put(newWord, list);
        }
    }
    Queue<Pair<String, Integer>> queue = new ArrayDeque<>();
    queue.offer(new Pair(beginWord, 1));
    Set<String> visited = new HashSet<>();
    visited.add(beginWord);
    while (!queue.isEmpty()) {
        Pair<String, Integer> node = queue.poll();
        String word = node.getKey();
        int level = node.getValue();
        for (int i = 0; i < len; i++) {
            String newWord = word.substring(0, i) + '*' + word.substring(i + 1, len);
            for (String neighbor : map.getOrDefault(newWord, new ArrayList<>())) {
                if (neighbor.equals(endWord)) {
                    return level + 1;
                }
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(new Pair(neighbor, level + 1));
                }
            }
        }
    }
    return 0;
}
```

Time Complexity: O(n * length<sup>2</sup>)

Space Complexity: O(n * length<sup>2</sup>)

## 2. Surrounded Regions (medium)

[LeetCode 130](https://leetcode.com/problems/surrounded-regions/)

```java
public void solve(char[][] board) {
    if (board == null || board.length < 3 || board[0].length < 3) {
        return;
    }
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++) {
        if (board[i][0] == 'O') {
            helper(board, i, 0);
        }
        if (board[i][n - 1] == 'O') {
            helper(board, i, n - 1);
        }
    }
    for (int i = 0; i < n; i++) {
        if (board[0][i] == 'O') {
            helper(board, 0, i);
        }
        if (board[m - 1][i] == 'O') {
            helper(board, m - 1, i);
        }
    }
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 'O') {
                board[i][j] = 'X';
            } else if (board[i][j] == '*') {
                board[i][j] = 'O';
            }
        }
    }
}

private void helper(char[][] board, int i, int j) {
    if (i < 0 || i > board.length - 1 || j < 0 || j > board[0].length - 1 || board[i][j] != 'O') {
        return;
    }
    board[i][j] = '*';
    helper(board, i - 1, j);
    helper(board, i + 1, j);
    helper(board, i, j - 1);
    helper(board, i, j + 1);
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

## 3. Lowest Common Ancestor of a Binary Tree (medium)

[LeetCode 236](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
        return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) {
        return root;
    }
    return left == null ? right : left;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 4. Binary Tree Maximum Path Sum (hard)

[LeetCode 124](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

```java
public int maxPathSum(TreeNode root) {
    int[] result = {Integer.MIN_VALUE};
    helper(root, result);
    return result[0];
}

private int helper(TreeNode root, int[] result) {
    if (root == null) {
        return 0;
    }
    int left = helper(root.left, result);
    int right = helper(root.right, result);
    result[0] = Math.max(result[0], left + right + root.val);
    return Math.max(left, right) + root.val < 0 ? 0 : Math.max(left, right) + root.val;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 5. Number of Provinces (medium)

[LeetCode 547](https://leetcode.com/problems/number-of-provinces/)

### Solution 1: dfs

```java
public int findCircleNum(int[][] isConnected) {
    if (isConnected == null || isConnected.length == 0) {
        return 0;
    }
    boolean[] visited = new boolean[isConnected.length];
    int count = 0;
    for (int i = 0; i < isConnected.length; i++) {
        if (!visited[i]) {
            helper(isConnected, i, visited);
            count++;
        }
    }
    return count;
}

private void helper(int[][] isConnected, int i, boolean[] visited) {
    for (int j = 0; j < isConnected.length; j++) {
        if (isConnected[i][j] == 1 && !visited[j]) {
            visited[j] = true;
            helper(isConnected, j, visited);
        }
    }
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(n)

### Solution 2: bfs

```java
public int findCircleNum(int[][] isConnected) {
    if (isConnected == null || isConnected.length == 0) {
        return 0;
    }
    boolean[] visited = new boolean[isConnected.length];
    int count = 0;
    Queue<Integer> queue = new ArrayDeque<>();
    for (int i = 0; i < isConnected.length; i++) {
        if (!visited[i]) {
            queue.offer(i);
            while (!queue.isEmpty()) {
                int num = queue.poll();
                visited[num] = true;
                for (int j = 0; j < isConnected.length; j++) {
                    if (!visited[j] && isConnected[num][j] == 1) {
                        queue.offer(j);
                    }
                }
            }
            count++;
        }
    }
    return count;
}
```

## 5. Course Schedule (medium)

[LeetCode 207](https://leetcode.com/problems/course-schedule/)

DAG (directed acyclic graph) 有向无环图 -> topological ordering 拓扑排序

### Solution 1: dfs + memo (otherwise time limit exceeded)

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    if (numCourses <= 0) {
        return false;
    }
    if (prerequisites == null || prerequisites.length == 0) {
        return true;
    }
    List[] graph = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new ArrayList<Integer>();
    }
    for (int i = 0; i < prerequisites.length; i++) {
        graph[prerequisites[i][1]].add(prerequisites[i][0]);
    }
    boolean[] visited = new boolean[numCourses];
    boolean[] memo = new boolean[numCourses];
    for (int i = 0; i < numCourses; i++) {
        if (!DFS(graph, visited, i, memo)) {
            return false;
        }
    }
    return true;
}

private boolean DFS(List[] graph, boolean[] visited, int course, boolean[] memo) {
    if (visited[course]) {
        // cycle
        return false;
    }
    if (memo[course]) {
        return true;
    }
    visited[course] = true;
    for (int i = 0; i < graph[course].size(); i++) {
        if (!DFS(graph, visited, (int)graph[course].get(i), memo)) {
            return false;
        }
    }
    visited[course] = false;
    memo[course] = true;
    return true;
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node + edge)

### Solution 2: bfs

```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    if (numCourses <= 0) {
        return false;
    }
    if (prerequisites == null || prerequisites.length == 0) {
        return true;
    }
    List[] graph = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new ArrayList<Integer>();
    }
    int[] degree = new int[numCourses];
    for (int i = 0; i < prerequisites.length; i++) {
        graph[prerequisites[i][0]].add(prerequisites[i][1]);
        degree[prerequisites[i][1]]++;
    }
    Queue<Integer> queue = new ArrayDeque<>();
    for (int i = 0; i < numCourses; i++) {
        if (degree[i] == 0) {
            queue.offer(i);
        }
    }
    int count = 0;
    while (!queue.isEmpty()) {
        int course = (int)queue.poll();
        count++;
        for (int i = 0; i < graph[course].size(); i++) {
            int next = (int)graph[course].get(i);
            degree[next]--;
            if (degree[next] == 0) {
                queue.offer(next);
            }
        }
    }
    return count == numCourses;
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node + edge)

## 6. Course Schedule II (medium)

[LeetCode 210](https://leetcode.com/problems/course-schedule-ii/)

### Solution 1: dfs

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    if (numCourses <= 0) {
        return new int[0];
    }
    if (prerequisites == null || prerequisites.length == 0) {
        return IntStream.rangeClosed(0, numCourses - 1).toArray();
    }
    List[] graph = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new ArrayList<Integer>();
    }
    for (int i = 0; i < prerequisites.length; i++) {
        graph[prerequisites[i][1]].add(prerequisites[i][0]);
    }
    boolean[] visited = new boolean[numCourses];
    boolean[] memo = new boolean[numCourses];
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        if (!DFS(graph, visited, i, memo, list)) {
            return new int[0];
        }
    }
    int[] result = new int[numCourse];
    for (int i = 0; i < numCourses; i++) {
        result[i] = list.get(numCourses - 1 - i);
    }
    return result;
}

private boolean DFS(List[] graph, boolean[] visited, int course, boolean[] memo, List<Integer> list) {
    if (visited[course]) {
        // cycle
        return false;
    }
    if (memo[course]) {
        return true;
    }
    visited[course] = true;
    for (int i = 0; i < graph[course].size(); i++) {
        if (!DFS(graph, visited, (int)graph[course].get(i), memo, list)) {
            return false;
        }
    }
    visited[course] = false;
    memo[course] = true;
    list.add(course);
    return true;
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node + edge)

### Solution 2: bfs

```java
public int[] findOrder(int numCourses, int[][] prerequisites) {
    if (numCourses <= 0) {
        return new int[0];
    }
    if (prerequisites == null || prerequisites.length == 0) {
        return IntStream.rangeClosed(0, numCourses - 1).toArray();
    }
    List[] graph = new List[numCourses];
    for (int i = 0; i < numCourses; i++) {
        graph[i] = new ArrayList<Integer>();
    }
    int[] degree = new int[numCourses];
    for (int i = 0; i < prerequisites.length; i++) {
        graph[prerequisites[i][0]].add(prerequisites[i][1]);
        degree[prerequisites[i][1]]++;
    }
    Queue<Integer> queue = new ArrayDeque<>();
    List<Integer> list = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        if (degree[i] == 0) {
            queue.offer(i);
            list.add(i);
        }
    }
    int count = 0;
    while (!queue.isEmpty()) {
        int course = (int)queue.poll();
        count++;
        for (int i = 0; i < graph[course].size(); i++) {
            int next = (int)graph[course].get(i);
            degree[next]--;
            if (degree[next] == 0) {
                queue.offer(next);
                list.add(next);
            }
        }
    }
    if (count != numCourses) {
        return new int[0];
    }
    int[] result = new int[numCourses];
    for (int i = 0; i < numCourses; i++) {
        result[i] = list.get(numCourses - 1 - i);
    }
    return result;
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node + edge)

## 7. Longest Increasing Path in a Matrix (hard)

[LeetCode 329](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

```java
public int longestIncreasingPath(int[][] matrix) {
    if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
        return 0;
    }
    int m = matrix.length, n = matrix[0].length;
    int[][] memo = new int[m][n];
    int result = 0;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            result = Math.max(result, dfs(matrix, i, j, memo, Integer.MIN_VALUE));
        }
    }
    return result;
}

private int dfs(int[][] matrix, int i, int j, int[][] memo, int previous) {
    if (i < 0 || i >= matrix.length || j < 0 || j >= matrix[0].length || matrix[i][j] <= previous) {
        return 0;
    }
    if (memo[i][j] > 0) {
        return memo[i][j];
    }
    int current = matrix[i][j];
    int tempMax = Integer.MIN_VALUE;
    tempMax = Math.max(tempMax, dfs(matrix, i - 1, j, memo, current));
    tempMax = Math.max(tempMax, dfs(matrix, i + 1, j, memo, current));
    tempMax = Math.max(tempMax, dfs(matrix, i, j - 1, memo, current));
    tempMax = Math.max(tempMax, dfs(matrix, i, j + 1, memo, current));
    memo[i][j] = tempMax + 1;
    return memo[i][j];
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

## 8. Alien Dictionary (hard)

[LeetCode 269](https://leetcode.com/problems/alien-dictionary/)

### Solution 1: dfs

```java
public String alienOrder(String[] words) {
    if (words == null || words.length == 0) {
        return "";
    }
    Map<Character, List<Character>> graph = new HashMap<>();
    for (String word : words) {
        for (char c : word.toCharArray()) {
            graph.put(c, new ArrayList<>());
        }
    }
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];
        if (word1.length() > word2.length() && word1.startsWith(word2)) {
            return "";
        }
        for (int j = 0; j < Math.min(word1.length(), word2.length()); j++) {
            if (word1.charAt(j) != word2.charAt(j)) {
                graph.get(word2.charAt(j)).add(word1.charAt(j));
                break;
            }
        }
    }
    Map<Character, Boolean> visited = new HashMap<>();
    StringBuilder sb = new StringBuilder();
    for (Character c : graph.keySet()) {
        if (!dfs(graph, c, visited, sb)) {
            return "";
        }
    }
    return sb.toString();
}

private boolean dfs(Map<Character, List<Character>> graph, Character c, Map<Character, Boolean> visited, StringBuilder sb) {
    if (visited.containsKey(c)) {
        return visited.get(c);
    }
    visited.put(c, false);
    for (Character next : graph.get(c)) {
        if (!dfs(graph, next, visited, sb)) {
            return false;
        }
    }
    visited.put(c, true);
    sb.append(c);
    return true;
}
```

Time Complexity: O(total length of all words)

Space Complexity: O(1)

### Solution 2: bfs

```java
public String alienOrder(String[] words) {
    if (words == null || words.length == 0) {
        return "";
    }
    Map<Character, List<Character>> graph = new HashMap<>();
    Map<Character, Integer> degree = new HashMap<>();
    for (String word : words) {
        for (char c : word.toCharArray()) {
            graph.put(c, new ArrayList<>());
            degree.put(c, 0);
        }
    }
    for (int i = 0; i < words.length - 1; i++) {
        String word1 = words[i];
        String word2 = words[i + 1];
        if (word1.length() > word2.length() && word1.startsWith(word2)) {
            return "";
        }
        for (int j = 0; j < Math.min(word1.length(), word2.length()); j++) {
            if (word1.charAt(j) != word2.charAt(j)) {
                graph.get(word1.charAt(j)).add(word2.charAt(j));
                degree.put(word2.charAt(j), degree.get(word2.charAt(j)) + 1);
                break;
            }
        }
    }
    Queue<Character> queue = new ArrayDeque<>();
    for (Character c : degree.keySet()) {
        if (degree.get(c).equals(0)) {
            queue.offer(c);
        }
    }
    StringBuilder sb = new StringBuilder();
    while (!queue.isEmpty()) {
        Character c = queue.poll();
        sb.append(c);
        for (Character next : graph.get(c)) {
            degree.put(next, degree.get(next) - 1);
            if (degree.get(next).equals(0)) {
                queue.offer(next);
            }
        }
    }
    if (sb.length() != degree.size()) {
        return "";
    }
    return sb.toString();
}
```

Time Complexity: O(total length of all words)

Space Complexity: O(1)

## 9. Count of Smaller Numbers After Self (hard)

[LeetCode 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

```java
public List<Integer> countSmaller(int[] nums) {
    Integer[] result = new Integer[nums.length];
    Arrays.fill(result, 0);
    int[] index = IntStream.rangeClosed(0, nums.length - 1).toArray();
    mergesort(nums, index, 0, nums.length, result);
    return Arrays.asList(result);
}

private void mergesort(int[] nums, int[] index, int left, int right, Integer[] result) {
    if (right - left <= 1) {
        return;
    }
    int mid = left + (right - left) / 2;
    mergesort(nums, index, left, mid, result);
    mergesort(nums, index, mid, right, result);
    merge(nums, index, left, right, mid, result);
}

private void merge(int[] nums, int[] index, int left, int right, int mid, Integer[] result) {
    int i = left, j = mid;
    List<Integer> list = new ArrayList<>();
    while (i < mid && j < right) {
        if (nums[index[i]] <= nums[index[j]]) {
            result[index[i]] += j - mid;
            list.add(index[i]);
            i++;
        } else {
            list.add(index[j]);
            j++;
        }
    }
    while (i < mid) {
        result[index[i]] += j - mid;
        list.add(index[i]);
        i++;
    }
    while (j < right) {
        list.add(index[j]);
        j++;
    }
    for (int k = left; k < right; k++) {
        index[k] = list.get(k - left);
    }
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)