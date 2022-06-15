# Medium Collection Day 2 (Trees and Graphs)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/108/trees-and-graphs/

## 1. Binary Tree Inorder Traversal (easy)

[LeetCode 94](https://leetcode.com/problems/binary-tree-inorder-traversal/)

### Solution 1: recursive

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    helper(root, result);
    return result;
}

private void helper(TreeNode root, List<Integer> result) {
    if (root != null) {
        helper(root.left, result);
        result.add(root.val);
        helper(root.right, result);
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: iterative

```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) {
        return result;
    }
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode helper = root;
    while (helper != null || !stack.isEmpty()) {
        if (helper != null) {
            stack.offerFirst(helper);
            helper = helper.left;
        } else {
            helper = stack.pollFirst();
            result.add(helper.val);
            helper = helper.right;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 2. Binary Tree Zigzag Level Order Traversal (medium)

[LeetCode 103](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

```java
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }
    Deque<TreeNode> dq = new ArrayDeque<>();
    dq.offerFirst(root);
    int level = 0;
    while (!dq.isEmpty()) {
        result.add(new ArrayList<Integer>());
        int len = dq.size();
        for (int i = 0; i < len; i++) {
            if (level % 2 == 0) {
                TreeNode node = dq.pollFirst();
                result.get(level).add(node.val);
                if (node.left != null) {
                    dq.offerLast(node.left);
                }
                if (node.right != null) {
                    dq.offerLast(node.right);
                }
            } else {
                TreeNode node = dq.pollLast();
                result.get(level).add(node.val);
                if (node.right != null) {
                    dq.offerFirst(node.right);
                }
                if (node.left != null) {
                    dq.offerFirst(node.left);
                }
            }
        }
        level++;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 3. Construct Binary Tree from Preorder and Inorder Traversal (medium)

[LeetCode 105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        map.put(inorder[i], i);
    }
    return helper(preorder, map, 0, inorder.length - 1, 0, preorder.length - 1);
}

private TreeNode helper(int[] preorder, Map<Integer, Integer> map, int inLeft, int inRight, int preLeft, int preRight) {
    if (inLeft > inRight || preLeft > preRight) {
        return null;
    }
    TreeNode root = new TreeNode(preorder[preLeft]);
    int mid = map.get(root.val);
    root.left = helper(preorder, map, inLeft, mid - 1, preLeft + 1, preLeft + mid - inLeft);
    root.right = helper(preorder, map, mid + 1, inRight, preRight + mid - inRight + 1, preRight);
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 4. Populating Next Right Pointers in Each Node (medium)

[LeetCode 116](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

### Solution 1: level order + queue

```java
public Node connect(Node root) {
    if (root == null) {
        return root;
    }
    Queue<Node> queue = new ArrayDeque<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int len = queue.size();
        for (int i = 0; i < len; i++) {
            Node node = queue.poll();
            if (i < len - 1) {
                node.next = queue.peek();
            }
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
    }
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 2

```java
public Node connect(Node root) {
    if (root == null) {
        return root;
    }
    Node leftmost = root;
    while (leftmost.left != null) {
        Node node = leftmost;
        while (node != null) {
            node.left.next = node.right;
            if (node.next != null) {
                node.right.next = node.next.left;
            }
            node = node.next;
        }
        leftmost = leftmost.left;
    }
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. Kth Smallest Element in a BST (medium)

[LeetCode 230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

```java
public int kthSmallest(TreeNode root, int k) {
    Deque<TreeNode> stack = new ArrayDeque<>();
    while (true) {
        while (root != null) {
            stack.offerFirst(root);
            root = root.left;
        }
        root = stack.pollFirst();
        if (--k == 0) {
            return root.val;
        }
        root = root.right;
    }
}
```

Time Complexity: O(height + k)

Space Complexity: O(height)

## 6. Inorder Successor in BST (medium)

[LeetCode 285](https://leetcode.com/problems/inorder-successor-in-bst/)

```java
public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
    TreeNode result = null;
    while (root != null) {
        if (p.val >= root.val) {
            root = root.right;
        } else {
            result = root;
            root = root.left;
        }
    }
    return result;
}
```

Time Complexity: O(height)

Space Complexity: O(1)

## 7. Number of Islands (medium)

[LeetCode 200](https://leetcode.com/problems/number-of-islands/)

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

