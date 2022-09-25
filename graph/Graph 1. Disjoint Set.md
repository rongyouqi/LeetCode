# Graph 1: Disjoint Set

[https://leetcode.com/explore/featured/card/graph/618/disjoint-set/](https://leetcode.com/explore/featured/card/graph/618/disjoint-set/)

## 1. [Overview of Disjoint Set](https://leetcode.com/explore/featured/card/graph/618/disjoint-set/3881/)

Given the vertices and edges between them, how could we quickly check whether two vertices are connected? For example, Figure 5 shows the edges between vertices, so how can we efficiently check if 0 is connected to 3, 1 is connected to 5, or 7 is connected to 8? We can do so by using the “disjoint set” data structure, also known as the “union-find” data structure. Note that others might refer to it as an algorithm. In this Explore Card, the term “disjoint set” refers to a data structure.

<img src="https://leetcode.com/explore/featured/card/Figures/Graph_Explore/Disjoint_Set_1_edited.png" style="zoom:50%;" />

_Figure 5. Each graph consists of vertices and edges. The root vertices are in blue._

The primary use of disjoint sets is to address the connectivity between the components of a network. The “network“ here can be a computer network or a social network. For instance, we can use a disjoint set to determine if two people share a common ancestor.

### Terminologies

-   Parent node: the direct parent node of a vertex. For example, in Figure 5, the parent node of vertex 3 is 1, the parent node of vertex 2 is 0, and the parent node of vertex 9 is 9.
-   Root node: a node without a parent node; it can be viewed as the parent node of itself. For example, in Figure 5, the root node of vertices 3 and 2 is 0. As for 0, it is its own root node and parent node. Likewise, the root node and parent node of vertex 9 is 9 itself. Sometimes the root node is referred to as the head node.

### Introduction to Disjoint Sets
- Summary of video content:
    1.  How do “disjoint sets” work.
    2.  Solving the connectivity question in Figure 5.

### Implementing “disjoint sets”
- Summary of video content:
    1.  How to implement a “disjoint set”.
    2.  The `find` function of a disjoint set.
    3.  The `union` function of a disjoint set.

### The two important functions of a “disjoint set.”

In the introduction videos above, we discussed the two important functions in a “disjoint set”.

-   **The `find` function** finds the root node of a given vertex. For example, in Figure 5, the output of the find function for vertex 3 is 0.
-   **The `union` function** unions two vertices and makes their root nodes the same. In Figure 5, if we union vertex 4 and vertex 5, their root node will become the same, which means the union function will modify the root node of vertex 4 or vertex 5 to the same root node. 

### There are two ways to implement a “disjoint set”.

-   Implementation with **Quick Find**: in this case, the time complexity of the `find` function will be O(1). However, the `union` function will take more time with the time complexity of O(N).
-   Implementation with **Quick Union**: compared with the Quick Find implementation, the time complexity of the `union` function is better. Meanwhile, the `find` function will take more time in this case.

## 2. Quick Find

```java
class UnionFind {
    private int[] root;

    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }
    // Time Complexity: O(n)

    public int find(int x) {
        return root[x];
    }
    // Time Complexity: O(1)
		
    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            for (int i = 0; i < root.length; i++) {
                if (root[i] == rootY) {
                    root[i] = rootX;
                }
            }
        }
    }
    // Time Complexity: O(n)

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    // Time Complexity: O(1)
}
```

Space Complexity: O(n)

## 3. Quick Union

==Generally speaking, **Quick Union** is more efficient than Quick Find.==

```java
class UnionFind {
    private int[] root;

    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }
    // Time Complexity: O(n)

    public int find(int x) {
        while (x != root[x]) {
            x = root[x];
        }
        return x;
    }
    // Time Complexity: O(n) worst case like linked list

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            root[rootY] = rootX;
        }
    }
    // Time Complexity: O(n)

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    // Time Complexity: O(n)
}
```

Space Complexity: O(n)

## 4. Union by Rank

We have implemented two kinds of “disjoint sets” so far, and they both have a concerning inefficiency. Specifically, the quick find implementation will always spend O(n) time on the union operation and in the quick union implementation, as shown in Figure 6, it is possible for all the vertices to form a line after connecting them using `union`, which results in the worst-case scenario for the `find` function. Is there any way to optimize these implementations?

<img src="https://leetcode.com/explore/learn/card/Figures/Graph_Explore/A_Line_Graph.png" style="zoom:50%;" />

_Figure 6. A line graph_

Of course, there is; it is to union by rank. The word “rank” means ordering by specific criteria. Previously, for the `union` function, we always chose the root node of `x` and set it as the new root node for the other vertex. However, by choosing the parent node based on certain criteria (by rank), we can limit the maximum height of each vertex.

To be specific, the “rank” refers to the height of each vertex. When we `union` two vertices, instead of always picking the root of `x` (or `y`, it doesn't matter as long as we're consistent) as the new root node, we choose the root node of the vertex with a larger “rank”. We will merge the shorter tree under the taller tree and assign the root node of the taller tree as the root node for both vertices. In this way, we effectively avoid the possibility of connecting all vertices into a straight line. This optimization is called the “disjoint set” with union by rank.

```java
class UnionFind {
    private int[] root;
    private int[] rank;

    public UnionFind(int size) {
        root = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
            rank[i] = 1; 
        }
    }
    // Time Complexity: O(n)

    public int find(int x) {
        while (x != root[x]) {
            x = root[x];
        }
        return x;
    }
    // Time Complexity: O(logn)

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    }
    // Time Complexity: O(logn)

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    // Time Complexity: O(logn)
}
```

Space Complexity: O(n)

## 5. Path Compression Optimization

In the previous implementation of the “disjoint set”, notice that to find the root node, we need to traverse the parent nodes sequentially until we reach the root node. If we search the root node of the same element again, we repeat the same operations. Is there any way to optimize this process?

The answer is yes! After finding the root node, we can update the parent node of all traversed elements to their root node. When we search for the root node of the same element again, we only need to traverse two elements to find its root node, which is highly efficient. So, how could we efficiently update the parent nodes of all traversed elements to the root node? The answer is to use “recursion”. This optimization is called “path compression”, which optimizes the `find` function.

```java
class UnionFind {
    private int[] root;

    public UnionFind(int size) {
        root = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
        }
    }
    // Time Complexity: O(n)

    public int find(int x) {
        if (x == root[x]) {
            return x;
        }
        return root[x] = find(root[x]);
    }
    // Time Complexity: O(logn)

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            root[rootY] = rootX;
        }
    }
    // Time Complexity: O(logn)

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    // Time Complexity: O(logn)
}
```

Space Complexity: O(n)

## 6. Optimized “disjoint set” with Path Compression and Union by Rank

```java
class UnionFind {
    private int[] root;
    private int[] rank;

    public UnionFind(int size) {
        root = new int[size];
        rank = new int[size];
        for (int i = 0; i < size; i++) {
            root[i] = i;
            rank[i] = 1;
        }
    }
    // Time Complexity: O(n)

    public int find(int x) {
        if (x == root[x]) {
            return x;
        }
        return root[x] = find(root[x]);
    }
    // Time Complexity: O(α(N))

    public void union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        if (rootX != rootY) {
            if (rank[rootX] > rank[rootY]) {
                root[rootY] = rootX;
            } else if (rank[rootX] < rank[rootY]) {
                root[rootX] = rootY;
            } else {
                root[rootY] = rootX;
                rank[rootX] += 1;
            }
        }
    }
    // Time Complexity: O(α(N))

    public boolean connected(int x, int y) {
        return find(x) == find(y);
    }
    // Time Complexity: O(α(N))
}
```

Space Complexity: O(n)

n is the number of vertices in the graph. α refers to the Inverse Ackermann function. In practice, we assume it's a constant. In other words, O(α(N)) is regarded as O(1) on average.

## 7. Summary

The main idea of a “disjoint set” is to have all connected vertices have the same parent node or root node, whether directly or indirectly connected. To check if two vertices are connected, we only need to check if they have the same root node.

The two most important functions for the “disjoint set” data structure are the `find` function and the `union` function. The `find` function locates the root node of a given vertex. The `union` function connects two previously unconnected vertices by giving them the same root node. There is another important function named `connected`, which checks the “connectivity” of two vertices. The `find` and `union` functions are essential for any question that uses the “disjoint set” data structure.

## 8.1 [LeetCode 547](https://leetcode.com/problems/number-of-provinces/) Number of Provinces (medium)

- There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.
- A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.
- You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.
- Return _the total number of **provinces**_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg" style="zoom: 67%;" />
    - **Input:** isConnected = `[[1,1,0],[1,1,0],[0,0,1]]`
    - **Output:** 2
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg" style="zoom:67%;" />
    - **Input:** isConnected = `[[1,0,0],[0,1,0],[0,0,1]]`
    - **Output:** 3
- **Constraints:**
    -   `1 <= n <= 200`
    -   `n == isConnected.length`
    -   `n == isConnected[i].length`
    -   `isConnected[i][j]` is `1` or `0`.
    -   `isConnected[i][i] == 1`
    -   `isConnected[i][j] == isConnected[j][i]`

### Solution

```java
public int findCircleNum(int[][] isConnected) {
    UnionFind uf = new UnionFind(isConnected.length);
    for (int i = 0; i < isConnected.length; i++) {
        for (int j = 0; j < isConnected.length; j++) {
            if (isConnected[i][j] == 1 && i != j) {
                uf.union(i, j);
            }
        }
    }
    int result = 0;
    for (int i = 0; i < isConnected.length; i++) {
        if (uf.find(i) == i) {
            result++;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.2 [LeetCode 261](https://leetcode.com/problems/graph-valid-tree/) Graph Valid Tree (medium)

- You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer n and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.
- Return `true` _if the edges of the given graph make up a valid tree, and_ `false` _otherwise_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg" style="zoom:67%;" />
    - **Input:** n = 5, edges = `[[0,1],[0,2],[0,3],[1,4]]`
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg" style="zoom:67%;" />
    - **Input:** n = 5, edges = `[[0,1],[1,2],[2,3],[1,3],[1,4]]`
    - **Output:** false
- **Constraints:**
    -   `1 <= n <= 2000`
    -   `0 <= edges.length <= 5000`
    -   `edges[i].length == 2`
    -   `0 <= ai, bi < n`
    -   `ai != bi`
    -   There are no self-loops or repeated edges.

### Solution

```java
public boolean validTree(int n, int[][] edges) {
    if (edges.length != n - 1) {
        return false;
    }
    UnionFind uf = new UnionFind(n);
    for (int[] edge : edges) {
        if (!uf.union(edge[0], edge[1])) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.3 [LeetCode 323](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/) Number of Connected Components in an Undirected Graph (medium)

- You have a graph of `n` nodes. You are given an integer `n` and an array `edges` where `edges[i] = [ai, bi]` indicates that there is an edge between `ai` and `bi` in the graph.
- Return _the number of connected components in the graph_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/14/conn1-graph.jpg" style="zoom:67%;" />
    - **Input:** n = 5, edges = `[[0,1],[1,2],[3,4]]`
    - **Output:** 2
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/14/conn2-graph.jpg" style="zoom:67%;" />
    - **Input:** n = 5, edges = `[[0,1],[1,2],[2,3],[3,4]]`
    - **Output:** 1
- **Constraints:**
    -   `1 <= n <= 2000`
    -   `1 <= edges.length <= 5000`
    -   `edges[i].length == 2`
    -   `0 <= ai <= bi < n`
    -   `ai != bi`
    -   There are no repeated edges.

### Solution

```java
public int countComponents(int n, int[][] edges) {
    UnionFind uf = new UnionFind(n);
    int result = n;
    for (int[] edge : edges) {
        if (uf.union(edge[0], edge[1])) {
            result--;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.4 [LeetCode 1101](https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/) The Earliest Moment When Everyone Become Friends (medium)

- There are n people in a social group labeled from `0` to `n - 1`. You are given an array `logs` where `logs[i] = [timestampi, xi, yi]` indicates that `xi` and `yi` will be friends at the time `timestampi`.
- Friendship is **symmetric**. That means if `a` is friends with `b`, then `b` is friends with `a`. Also, person `a` is acquainted with a person `b` if `a` is friends with `b`, or `a` is a friend of someone acquainted with `b`.
- Return _the earliest time for which every person became acquainted with every other person_. If there is no such earliest time, return `-1`.
- **Example 1:**
    - **Input:** logs = `[[20190101,0,1],[20190104,3,4],[20190107,2,3],[20190211,1,5],[20190224,2,4],[20190301,0,3],[20190312,1,2],[20190322,4,5]]`, n = 6
    - **Output:** 20190301
    - **Explanation:** 
        - The first event occurs at timestamp = 20190101 and after 0 and 1 become friends we have the following friendship groups [0,1], [2], [3], [4], [5].
        - The second event occurs at timestamp = 20190104 and after 3 and 4 become friends we have the following friendship groups [0,1], [2], [3,4], [5].
        - The third event occurs at timestamp = 20190107 and after 2 and 3 become friends we have the following friendship groups [0,1], [2,3,4], [5].
        - The fourth event occurs at timestamp = 20190211 and after 1 and 5 become friends we have the following friendship groups [0,1,5], [2,3,4].
        - The fifth event occurs at timestamp = 20190224 and as 2 and 4 are already friends anything happens.
        - The sixth event occurs at timestamp = 20190301 and after 0 and 3 become friends we have that all become friends.
- **Example 2:**
    - **Input:** logs = `[[0,2,0],[1,0,1],[3,0,3],[4,1,2],[7,3,1]]`, n = 4
    - **Output:** 3
- **Constraints:**
    -   `2 <= n <= 100`
    -   `1 <= logs.length <= 10^4`
    -   `logs[i].length == 3`
    -   `0 <= timestampi <= 10^9`
    -   `0 <= xi, yi <= n - 1`
    -   `xi != yi`
    -   All the values `timestampi` are **unique**.
    -   All the pairs `(xi, yi)` occur at most one time in the input.

### Solution

```java
public int earliestAcq(int[][] logs, int n) {
    Arrays.sort(logs, (a, b) -> a[0] - b[0]);
    UnionFind uf = new UnionFind(n);
    int groupCount = n;
    for (int[] log : logs) {
        if (uf.union(log[1], log[2])) {
            if (--groupCount == 1) {
                return log[0];
            }
        }
    }
    return -1;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.5 [LeetCode 1202](https://leetcode.com/problems/smallest-string-with-swaps/) Smallest String with Swaps (medium)

- You are given a string `s`, and an array of pairs of indices in the string `pairs` where `pairs[i] = [a, b]` indicates 2 indices(0-indexed) of the string.
- You can swap the characters at any pair of indices in the given `pairs` **any number of times**.
- Return the lexicographically smallest string that `s` can be changed to after using the swaps.
- **Example 1:**
    - **Input:** s = "dcab", pairs = `[[0,3],[1,2]]`
    - **Output:** "bacd"
    - **Explaination:** 
        - Swap s[0] and s[3], s = "bcad"
        - Swap s[1] and s[2], s = "bacd"
- **Example 2:**
    - **Input:** s = "dcab", pairs = `[[0,3],[1,2],[0,2]]`
    - **Output:** "abcd"
    - **Explaination:** 
        - Swap s[0] and s[3], s = "bcad"
        - Swap s[0] and s[2], s = "acbd"
        - Swap s[1] and s[2], s = "abcd"
- **Example 3:**
    - **Input:** s = "cba", pairs = `[[0,1],[1,2]]`
    - **Output:** "abc"
    - **Explaination:** 
        - Swap s[0] and s[1], s = "bca"
        - Swap s[1] and s[2], s = "bac"
        - Swap s[0] and s[1], s = "abc"
- **Constraints:**
    -   `1 <= s.length <= 10^5`
    -   `0 <= pairs.length <= 10^5`
    -   `0 <= pairs[i][0], pairs[i][1] < s.length`
    -   `s` only contains lower case English letters.

### Solution

```java
public String smallestStringWithSwaps(String s, List<List<Integer>> pairs) {
    UnionFind uf = new UnionFind(s.length());
    for (List<Integer> pair : pairs) {
        uf.union(pair.get(0), pair.get(1));
    }
    Map<Integer, List<Integer>> map = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        int root = uf.find(i);
        map.computeIfAbsent(root, k -> new ArrayList<>()).add(i);
    }
    char[] result = new char[s.length()];
    for (List<Integer> list : map.values()) {
        List<Character> chars = new ArrayList<>();
        for (int index : list) {
            chars.add(s.charAt(index));
        }
        Collections.sort(chars);
        for (int i = 0; i < list.size(); i++) {
            result[list.get(i)] = chars.get(i);
        }
    }
    return new String(result);
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.6 [LeetCode 399](https://leetcode.com/problems/evaluate-division/) Evaluate Division (medium)

- You are given an array of variable pairs `equations` and an array of real numbers `values`, where `equations[i] = [Ai, Bi]` and `values[i]` represent the equation `Ai / Bi = values[i]`. Each `Ai` or `Bi` is a string that represents a single variable.
- You are also given some `queries`, where `queries[j] = [Cj, Dj]` represents the `jth` query where you must find the answer for `Cj / Dj = ?`.
- Return _the answers to all queries_. If a single answer cannot be determined, return `-1.0`.
- **Note:** The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.
- **Example 1:**
    - **Input:** equations = `[["a","b"],["b","c"]]`, values = [2.0,3.0], queries = `[["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]`
    - **Output:** [6.00000,0.50000,-1.00000,1.00000,-1.00000]
    - **Explanation:** 
        - Given: _a / b = 2.0_, _b / c = 3.0_
        - queries are: _a / c = ?_, _b / a = ?_, _a / e = ?_, _a / a = ?_, _x / x = ?_
        - return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
- **Example 2:**
    - **Input:** equations = `[["a","b"],["b","c"],["bc","cd"]]`, values = [1.5,2.5,5.0], queries = `[["a","c"],["c","b"],["bc","cd"],["cd","bc"]]`
    - **Output:** [3.75000,0.40000,5.00000,0.20000]
- **Example 3:**
    - **Input:** equations = `[["a","b"]]`, values = [0.5], queries = `[["a","b"],["b","a"],["a","c"],["x","y"]]`
    - **Output:** [0.50000,2.00000,-1.00000,-1.00000]
- **Constraints:**
    -   `1 <= equations.length <= 20`
    -   `equations[i].length == 2`
    -   `1 <= Ai.length, Bi.length <= 5`
    -   `values.length == equations.length`
    -   `0.0 < values[i] <= 20.0`
    -   `1 <= queries.length <= 20`
    -   `queries[i].length == 2`
    -   `1 <= Cj.length, Dj.length <= 5`
    -   `Ai, Bi, Cj, Dj` consist of lower case English letters and digits.

### Solution

```java
public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
    
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8.7 [LeetCode 1168](https://leetcode.com/problems/optimize-water-distribution-in-a-village/) Optimize Water Distribution in a Village (hard)

- There are `n` houses in a village. We want to supply water for all the houses by building wells and laying pipes.
- For each house `i`, we can either build a well inside it directly with cost `wells[i - 1]` (note the `-1` due to **0-indexing**), or pipe in water from another well to it. The costs to lay pipes between houses are given by the array `pipes` where each `pipes[j] = [house1j, house2j, costj]` represents the cost to connect `house1j` and `house2j` together using a pipe. Connections are bidirectional, and there could be multiple valid connections between the same two houses with different costs.
- Return _the minimum total cost to supply water to all houses_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2019/05/22/1359_ex1.png"  />
    - **Input:** n = 3, wells = [1,2,2], pipes = `[[1,2,1],[2,3,1]]`
    - **Output:** 3
    - **Explanation:** The image shows the costs of connecting houses using pipes.
        - The best strategy is to build a well in the first house with cost 1 and connect the other houses to it with cost 2 so the total cost is 3.
- **Example 2:**
    - **Input:** n = 2, wells = [1,1], pipes = `[[1,2,1],[1,2,2]]`
    - **Output:** 2
    - **Explanation:** We can supply water with cost two using one of the three options:
        - Option 1:
            - Build a well inside house 1 with cost 1.
            - Build a well inside house 2 with cost 1.
            - The total cost will be 2.
        - Option 2:
            - Build a well inside house 1 with cost 1.
            - Connect house 2 with house 1 with cost 1.
            - The total cost will be 2.
        - Option 3:
            - Build a well inside house 2 with cost 1.
            - Connect house 1 with house 2 with cost 1.
            - The total cost will be 2.
        - Note that we can connect houses 1 and 2 with cost 1 or with cost 2 but we will always choose **the cheapest option**. 
- **Constraints:**
    -   `2 <= n <= 10^4`
    -   `wells.length == n`
    -   `0 <= wells[i] <= 10^5`
    -   `1 <= pipes.length <= 10^4`
    -   `pipes[j].length == 3`
    -   `1 <= house1j, house2j <= n`
    -   `0 <= costj <= 10^5`
    -   `house1j != house2j`

### Solution

```java
public int minCostToSupplyWater(int n, int[] wells, int[][] pipes) {
    
}
```

Time Complexity: O(n)

Space Complexity: O(n)
