# Blind 75 Day 4 (Graph)

## 1. [LeetCode 133](https://leetcode.com/problems/clone-graph/) Clone Graph (medium)

- Given a reference of a node in a **[connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph)** undirected graph.
- Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.
- Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.
```
class Node {
    public int val;
    public List<Node> neighbors;
}
```
- **Test case format:**
- For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with `val == 1`, the second node with `val == 2`, and so on. The graph is represented in the test case using an adjacency list.
- **An adjacency list** is a collection of unordered **lists** used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.
- The given node will always be the first node with `val = 1`. You must return the **copy of the given node** as a reference to the cloned graph.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2019/11/04/133_clone_graph_question.png" style="zoom: 25%;" />
    - **Input:** adjList = `[[2,4],[1,3],[2,4],[1,3]]`
    - **Output:** `[[2,4],[1,3],[2,4],[1,3]]`
    - **Explanation:** There are 4 nodes in the graph.
        - 1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
        - 2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
        - 3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
        - 4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2020/01/07/graph.png)
    - **Input:** adjList = `[[]]`
    - **Output:** `[[]]`
    - **Explanation:** Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.
- **Example 3:**
    - **Input:** adjList = `[]`
    - **Output:** `[]`
    - **Explanation:** This an empty graph, it does not have any nodes.
- **Constraints:**
    -   The number of nodes in the graph is in the range `[0, 100]`.
    -   `1 <= Node.val <= 100`
    -   `Node.val` is unique for each node.
    -   There are no repeated edges and no self-loops in the graph.
    -   The Graph is connected and all nodes can be visited starting from the given node.

### Solution 1: dfs

```java
public Node cloneGraph(Node node) {
    if (node == null) {
        return null;
    }
    Map<Node, Node> map = new HashMap<>();
    Node cloneNode = new Node(node.val);
    map.put(node, cloneNode);
    for (Node neighbor : node.neighbors) {
        if (!map.containsKey(neighbor)) {
            map.put(neighbor, new Node(neighbor.val));
            DFS(neighbor, map);
        }
        cloneNode.neighbors.add(map.get(neighbor));
    }
    return cloneNode;
}

private void DFS(Node node, Map<Node, Node> map) {
    Node copy = map.get(node);
    for (Node neighbor : node.neighbors) {
        if (!map.containsKey(neighbor)) {
            map.put(neighbor, new Node(neighbor.val));
            DFS(neighbor, map);
        }
        copy.neighbors.add(map.get(neighbor));
    }
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node)

### Solution 2: bfs

```java
public Node cloneGraph(Node node) {
    if (node == null) {
        return null;
    }
    Map<Node, Node> map = new HashMap<>();
    Queue<Node> queue = new ArrayDeque<>();
    Node cloneNode = new Node(node.val);
    map.put(node, cloneNode);
    queue.offer(node);
    while (!queue.isEmpty()) {
        Node old = queue.poll();
        for (Node neighbor : old.neighbors) {
            if (!map.containsKey(neighbor)) {
                map.put(neighbor, new Node(neighbor.val));
                queue.offer(neighbor);
            }
            map.get(old).neighbors.add(map.get(neighbor));
        }
    }
    return cloneNode;
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node)

## 2. [LeetCode 207](https://leetcode.com/problems/course-schedule/) Course Schedule (medium)

- There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.
    -   For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
- Return `true` if you can finish all courses. Otherwise, return `false`.
- **Example 1:**
    - **Input:** numCourses = 2, prerequisites = `[[1,0]]`
    - **Output:** true
    - **Explanation:** There are a total of 2 courses to take. 
        - To take course 1 you should have finished course 0. So it is possible.
- **Example 2:**
    - **Input:** numCourses = 2, prerequisites = `[[1,0],[0,1]]`
    - **Output:** false
    - **Explanation:** There are a total of 2 courses to take. 
        - To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
- **Constraints:**
    -   `1 <= numCourses <= 2000`
    -   `0 <= prerequisites.length <= 5000`
    -   `prerequisites[i].length == 2`
    -   `0 <= ai, bi < numCourses`
    -   All the pairs prerequisites[i] are **unique**.

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

## 3. [LeetCode 417](https://leetcode.com/problems/pacific-atlantic-water-flow/) Pacific Atlantic Water Flow (medium)

- There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.
- The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.
- The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.
- Return _a **2D list** of grid coordinates_ `result` _where_ `result[i] = [ri, ci]` _denotes that rain water can flow from cell_ `(ri, ci)` _to **both** the Pacific and Atlantic oceans_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/06/08/waterflow-grid.jpg" style="zoom:50%;" />
    - **Input:** heights = `[[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]`
    - **Output:** `[[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]`
- **Example 2:**
    - **Input:** heights = `[[2,1],[1,2]]`
    - **Output:** `[[0,0],[0,1],[1,0],[1,1]]`
- **Constraints:**
    -   `m == heights.length`
    -   `n == heights[r].length`
    -   `1 <= m, n <= 200`
    -   `0 <= heights[r][c] <= 10^5`

### Solution: backtracking

```java
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    List<List<Integer>> result = new ArrayList<>();
    if (heights == null || heights.length == 0) {
        return result;
    }
    int m = heights.length, n = heights[0].length;
    byte[][] dp = new byte[m][n];
    for (int i = 0; i < m; i++) {
        helper(i, 0, 1, heights[i][0], heights, dp, result);
        helper(i, n - 1, 2, heights[i][n - 1], heights, dp, result);
    }
    for (int j = 0; j < n; j++) {
        helper(0, j, 1, heights[0][j], heights, dp, result);
        helper(m - 1, j, 2, heights[m - 1][j], heights, dp, result);
    }
    return result;
}

private void helper(int i, int j, int ocean, int h, int[][] heights, byte[][] dp, List<List<Integer>> result) {
    if (i < 0 || i >= heights.length || j < 0 || j >= heights[0].length || (dp[i][j] & ocean) > 0 || heights[i][j] < h) {
        return;
    }
    dp[i][j] += ocean;
    if (dp[i][j] == 3) {
        result.add(Arrays.asList(i, j));
    }
    helper(i - 1, j, ocean, heights[i][j], heights, dp, result);
    helper(i + 1, j, ocean, heights[i][j], heights, dp, result);
    helper(i, j - 1, ocean, heights[i][j], heights, dp, result);
    helper(i, j + 1, ocean, heights[i][j], heights, dp, result);
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

## 4. [LeetCode 200](https://leetcode.com/problems/number-of-islands/) Number of Islands (medium)

- Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.
- An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
- **Example 1:**
    - **Input:** 
    ```
    grid = [
      ["1","1","1","1","0"],
      ["1","1","0","1","0"],
      ["1","1","0","0","0"],
      ["0","0","0","0","0"]
    ]
    ```
    - **Output:** 1
- **Example 2:**
    - **Input:** 
    ```
    grid = [
      ["1","1","0","0","0"],
      ["1","1","0","0","0"],
      ["0","0","1","0","0"],
      ["0","0","0","1","1"]
    ]
    ```
    **Output:** 3
- **Constraints:**
    -   `m == grid.length`
    -   `n == grid[i].length`
    -   `1 <= m, n <= 300`
    -   `grid[i][j]` is `'0'` or `'1'`.

### Solution 1: dfs

```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    int result = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                result++;
                dfs(grid, i, j);
            }
        }
    }
    return result;
}

private void dfs(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] == '0') {
        return;
    }
    grid[i][j] = '0';
    dfs(grid, i - 1, j);
    dfs(grid, i + 1, j);
    dfs(grid, i, j - 1);
    dfs(grid, i, j + 1);
}
```

Time Complexity: O(mn)

Space Complexity: O(mn)

### Solution 2: bfs

```java
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) {
        return 0;
    }
    int result = 0, len = grid[0].length;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                result++;
                grid[i][j] = '0';
                Queue<Integer> q = new ArrayDeque<>();
                q.offer(i * len + j);
                while (!q.isEmpty()) {
                    int id = q.poll();
                    int row = id / len;
                    int col = id % len;
                    if (row > 0 && grid[row - 1][col] == '1') {
                        q.offer((row - 1) * len + col);
                        grid[row - 1][col] = '0';
                    }
                    if (row + 1 < grid.length && grid[row + 1][col] == '1') {
                        q.offer((row + 1) * len + col);
                        grid[row + 1][col] = '0';
                    }
                    if (col > 0 && grid[row][col - 1] == '1') {
                        q.offer(row * len + col - 1);
                        grid[row][col - 1] = '0';
                    }
                    if (col + 1 < grid[0].length && grid[row][col + 1] == '1') {
                        q.offer(row * len + col + 1);
                        grid[row][col + 1] = '0';
                    }
                }
            }
        }
    }
    return result;
}
```

Time Complexity: O(mn)

Space Complexity: O(min(m, n))

## 5. [LeetCode 128](https://leetcode.com/problems/longest-consecutive-sequence/) Longest Consecutive Sequence (medium)

- Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._
- You must write an algorithm that runs in `O(n)` time.
- **Example 1:**
    - **Input:** nums = [100,4,200,1,3,2]
    - **Output:** 4
    - **Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.
- **Example 2:**
    - **Input:** nums = [0,3,7,2,5,8,4,6,0,1]
    - **Output:** 9
- **Constraints:**
    -   `0 <= nums.length <= 10^5`
    -   `-10^9 <= nums[i] <= 10^9`

### Solution

```java
public int longestConsecutive(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        set.add(num);
    }
    int result = 0;
    for (int num : set) {
        if (!set.contains(num - 1)) {
            int current = num, temp = 1;
            while (set.contains(current + 1)) {
                temp++;
                current++;
            }
            result = Math.max(result, temp);
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 6. [LeetCode 269](https://leetcode.com/problems/alien-dictionary/) Alien Dictionary (hard)

- There is a new alien language that uses the English alphabet. However, the order among the letters is unknown to you.
- You are given a list of strings `words` from the alien language's dictionary, where the strings in `words` are **sorted lexicographically** by the rules of this new language.
- Return _a string of the unique letters in the new alien language sorted in **lexicographically increasing order** by the new language's rules. If there is no solution, return_ `""`_. If there are multiple solutions, return **any of them**_.
- A string `s` is **lexicographically smaller** than a string `t` if at the first letter where they differ, the letter in `s` comes before the letter in `t` in the alien language. If the first `min(s.length, t.length)` letters are the same, then `s` is smaller if and only if `s.length < t.length`.
- **Example 1:**
    - **Input:** words = ["wrt","wrf","er","ett","rftt"]
    - **Output:** "wertf"
- **Example 2:**
    - **Input:** words = ["z","x"]
    - **Output:** "zx"
- **Example 3:**
    - **Input:** words = ["z","x","z"]
    - **Output:** ""
    - **Explanation:** The order is invalid, so return `""`.
- **Constraints:**
    -   `1 <= words.length <= 100`
    -   `1 <= words[i].length <= 100`
    -   `words[i]` consists of only lowercase English letters.

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

## 7. [LeetCode 261](https://leetcode.com/problems/graph-valid-tree/) Graph Valid Tree (medium)

- You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer n and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.
- Return `true` _if the edges of the given graph make up a valid tree, and_ `false` _otherwise_.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg)
    - **Input:** n = 5, edges = `[[0,1],[0,2],[0,3],[1,4]]`
    - **Output:** true
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg)
    - **Input:** n = 5, edges = `[[0,1],[1,2],[2,3],[1,3],[1,4]]`
    - **Output:** false
- **Constraints:**
    -   `1 <= n <= 2000`
    -   `0 <= edges.length <= 5000`
    -   `edges[i].length == 2`
    -   `0 <= ai, bi < n`
    -   `ai != bi`
    -   There are no self-loops or repeated edges.

### Solution 1: dfs

```java
public boolean validTree(int n, int[][] edges) {
    if (edges == null || edges.length != n - 1) {
        return false;
    }
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for (int i = 0; i < n; i++) {
        graph.put(i, new ArrayList<>());
    }
    for (int[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
        graph.get(edge[1]).add(edge[0]);
    }
    Set<Integer> visited = new HashSet<>();
    dfs(0, graph, visited);
    return visited.size() == n;
}

private void dfs(int node, Map<Integer, List<Integer>> graph, Set<Integer> visited) {
    if (visited.contains(node)) {
        return;
    }
    visited.add(node);
    for (int neighbor : graph.get(node)) {
        dfs(neighbor, graph, visited);
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: bfs

```java
public boolean validTree(int n, int[][] edges) {
    if (edges == null || edges.length != n - 1) {
        return false;
    }
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for (int i = 0; i < n; i++) {
        graph.put(i, new ArrayList<>());
    }
    for (int[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
        graph.get(edge[1]).add(edge[0]);
    }
    Queue<Integer> queue = new ArrayDeque<>();
    Set<Integer> visited = new HashSet<>();
    queue.offer(0);
    visited.add(0);
    while (!queue.isEmpty()) {
        int node = queue.poll();
        for (int neighbor : graph.get(node)) {
            if (visited.contains(neighbor)) {
                continue;
            }
            queue.offer(neighbor);
            visited.add(neighbor);
        }
    }
    return visited.size() == n;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8. [LeetCode 323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) Number of Connected Components in an Undirected Graph (medium)

- You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.
- Return _the number of connected components in the graph_.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg)
    - **Input:** n = 5, edges = `[[0,1],[1,2],[3,4]]`
    - **Output:** 2
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg)
    - **Input:** n = 5, edges = `[[0,1],[1,2],[2,3],[3,4]]`
    - **Output:** 1
- **Constraints:**
    -   `1 <= n <= 2000`
    -   `1 <= edges.length <= 5000`
    -   `edges[i].length == 2`
    -   `0 <= ai <= bi < n`
    -   `ai != bi`
    -   There are no repeated edges.

### Solution: dfs

```java
public int countComponents(int n, int[][] edges) {
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for (int i = 0; i < n; i++) {
        graph.put(i, new ArrayList<>());
    }
    for (int[] edge : edges) {
        graph.get(edge[0]).add(edge[1]);
        graph.get(edge[1]).add(edge[0]);
    }
    Set<Integer> visited = new HashSet<>();
    int result = 0;
    for (int i = 0; i < n; i++) {
        if (!visited.contains(i)) {
            result++;
            dfs(i, graph, visited);
        }
    }
    return result;
}

private void dfs(int node, Map<Integer, List<Integer>> graph, Set<Integer> visited) {
    visited.add(node);
    for (int neighbor : graph.get(node)) {
        if (!visited.contains(neighbor)) {
            dfs(neighbor, graph, visited);
        }
    }
}
```

Time Complexity: O(node + edge)

Space Complexity: O(node + edge)
