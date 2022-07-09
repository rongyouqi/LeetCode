# Blind 75 Day 7 (Tree)

## 1. [LeetCode 104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) Maximum Depth of Binary Tree (easy)

- Given the `root` of a binary tree, return _its maximum depth_.
- A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [3,9,20,null,null,15,7]
    - **Output:** 3
- **Example 2:**
    - **Input:** root = [1,null,2]
    - **Output:** 2
- **Constraints:**
    -   The number of nodes in the tree is in the range `[0, 10^4]`.
    -   `-100 <= Node.val <= 100`

### Solution

```java
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 2. [LeetCode 100](https://leetcode.com/problems/same-tree/) Same Tree (easy)

- Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.
- Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg" style="zoom:67%;" />
    - **Input:** p = [1,2,3], q = [1,2,3]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg" style="zoom:67%;" />
    - **Input:** p = [1,2], q = [1,null,2]
    - **Output:** false
- **Example 3:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg" style="zoom:67%;" />
    - **Input:** p = [1,2,1], q = [1,1,2]
    - **Output:** false
- **Constraints:**
    -   The number of nodes in both trees is in the range `[0, 100]`.
    -   `-10^4 <= Node.val <= 10^4`

### Solution

```java
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    }
    if (p == null || q == null) {
        return false;
    }
    if (p.val != q.val) {
        return false;
    }
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 3. [LeetCode 226](https://leetcode.com/problems/invert-binary-tree/) Invert/Flip Binary Tree (easy)

- Given the `root` of a binary tree, invert the tree, and return _its root_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg" style="zoom: 67%;" />
    - **Input:** root = [4,2,7,1,3,6,9]
    - **Output:** [4,7,2,9,6,3,1]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [2,1,3]
    - **Output:** [2,3,1]
- **Example 3:**
    - **Input:** root = []
    - **Output:** []
- **Constraints:**
    -   The number of nodes in the tree is in the range `[0, 100]`.
    -   `-100 <= Node.val <= 100`

### Solution

```java
public TreeNode invertTree(TreeNode root) {
    if (root == null) {
        return null;
    }
    TreeNode left = invertTree(root.left);
    TreeNode right = invertTree(root.right);
    root.left = right;
    root.right = left;
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 4. [LeetCode 124](https://leetcode.com/problems/binary-tree-maximum-path-sum/) Binary Tree Maximum Path Sum (hard)

- A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.
- The **path sum** of a path is the sum of the node's values in the path.
- Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg" style="zoom:67%;" />
    - **Input:** root = [1,2,3]
    - **Output:** 6
    - **Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg" style="zoom:67%;" />
    - **Input:** root = [-10,9,20,null,null,15,7]
    - **Output:** 42
    - **Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.
- **Constraints:**
    -   The number of nodes in the tree is in the range `[1, 3 * 10^4]`.
    -   `-1000 <= Node.val <= 1000`

### Solution

1. what do you expect from your left-child or right-child?
     * left = max sum of half path in left subtree
     * right = max sum of half path in right subtree
2. what do you want to do in the current layer?
     * check and update result
3. what do you want to report to your parent?
     * return max positive sum of half path or prune

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

## 5. [LeetCode 102](https://leetcode.com/problems/binary-tree-level-order-traversal/) Binary Tree Level Order Traversal (medium)

- Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg" style="zoom:67%;" />
    - **Input:** root = [3,9,20,null,null,15,7]
    - **Output:** `[[3],[9,20],[15,7]]`
- **Example 2:**
    - **Input:** root = [1]
    - **Output:** `[[1]]`
- **Example 3:**
    - **Input:** root = []
    - **Output:** `[]`
- **Constraints:**
    -   The number of nodes in the tree is in the range `[0, 2000]`.
    -   `-1000 <= Node.val <= 1000`

### Solution 1: bfs

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }
    Queue<TreeNode> q = new ArrayDeque<>();
    q.offer(root);
    int level = 0;
    while (!q.isEmpty()) {
        result.add(new ArrayList<Integer>());
        int len = q.size();
        for (int i = 0; i < len; i++) {
            TreeNode node = q.poll();
            result.get(level).add(node.val);
            if (node.left != null) {
                q.offer(node.left);
            }
            if (node.right != null) {
                q.offer(node.right);
            }
        }
        level++;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: dfs

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) {
        return result;
    }
    helper(root, 0, result);
    return result;
}

private void helper(TreeNode root, int level, List<List<Integer>> result) {
    if (result.size() == level) {
        result.add(new ArrayList<Integer>());
    }
    result.get(level).add(root.val);
    if (root.left != null) {
        helper(root.left, level + 1, result);
    }
    if (root.right != null) {
        helper(root.right, level + 1, result);
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 6. [LeetCode 297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) Serialize and Deserialize Binary Tree (hard)

- Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.
- Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.
- **Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://leetcode.com/faq/#binary-tree). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg" style="zoom:67%;" />
    - **Input:** root = [1,2,3,null,null,4,5]
    - **Output:** [1,2,3,null,null,4,5]
- **Example 2:**
    - **Input:** root = []
    - **Output:** []
- **Constraints:**
    -   The number of nodes in the tree is in the range `[0, 10^4]`.
    -   `-1000 <= Node.val <= 1000`

### Solution 1: level-order iterative

```java
public String serialize(TreeNode root) {
    if (root == null) {
        return "#";
    }
    StringBuilder sb = new StringBuilder();
    Queue<TreeNode> q = new LinkedList<>(); // LinkedList to store null;
    q.offer(root);
    while (!q.isEmpty()) {
        TreeNode node = q.poll();
        if (node == null) {
            sb.append("# ");
            continue;
        }
        sb.append(node.val + " ");
        q.offer(node.left);
        q.offer(node.right);
    }
    return sb.toString();
}

public TreeNode deserialize(String data) {
    if (data == "#") {
        return null;
    }
    String[] array = data.split(" ");
    TreeNode root = new TreeNode(Integer.parseInt(array[0]));
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    for (int i = 1; i < array.length; i++) {
        TreeNode parent = q.poll();
        if (!array[i].equals("#")) {
            TreeNode left = new TreeNode(Integer.parseInt(array[i]));
            parent.left = left;
            q.offer(left);
        }
        i++;
        if (!array[i].equals("#")) {
            TreeNode right = new TreeNode(Integer.parseInt(array[i]));
            parent.right = right;
            q.offer(right);
        }
    }
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: pre-order recursive

```java
public String serialize(TreeNode root) {
    if (root == null) {
        return "#";
    }
    return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

public TreeNode deserialize(String data) {
    Queue<String> q = new LinkedList<>(Arrays.asList(data.split(",")));
    return helper(q);
}

private TreeNode helper(Queue<String> q) {
    String s = q.poll();
    if (s.equals("#")) {
        return null;
    }
    TreeNode root = new TreeNode(Integer.parseInt(s));
    root.left = helper(q);
    root.right = helper(q);
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 7. [LeetCode 572](https://leetcode.com/problems/subtree-of-another-tree/) Subtree of Another Tree (easy)

- Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.
- A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [3,4,5,1,2], subRoot = [4,1,2]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
    - **Output:** false
- **Constraints:**
    -   The number of nodes in the `root` tree is in the range `[1, 2000]`.
    -   The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
    -   `-10^4 <= root.val <= 10^4`
    -   `-10^4 <= subRoot.val <= 10^4`

### Solution

```java
public boolean isSubtree(TreeNode root, TreeNode subRoot) {
    if (root == null) {
        return subRoot == null;
    }
    return isSameTree(root, subRoot) || isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}

private boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    }
    if (p == null || q == null) {
        return false;
    }
    if (p.val != q.val) {
        return false;
    }
    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 8. [LeetCode 105](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) Construct Binary Tree from Preorder and Inorder Traversal (medium)

- Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/tree.jpg" style="zoom:67%;" />
    - **Input:** preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
    - **Output:** [3,9,20,null,null,15,7]
- **Example 2:**
    - **Input:** preorder = [-1], inorder = [-1]
    - **Output:** [-1]
- **Constraints:**
    -   `1 <= preorder.length <= 3000`
    -   `inorder.length == preorder.length`
    -   `-3000 <= preorder[i], inorder[i] <= 3000`
    -   `preorder` and `inorder` consist of **unique** values.
    -   Each value of `inorder` also appears in `preorder`.
    -   `preorder` is **guaranteed** to be the preorder traversal of the tree.
    -   `inorder` is **guaranteed** to be the inorder traversal of the tree.

### Solution

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

## 9. [LeetCode 98](https://leetcode.com/problems/validate-binary-search-tree/) Validate Binary Search Tree (medium)

- Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.
- A **valid BST** is defined as follows:
    -   The left subtree of a node contains only nodes with keys **less than** the node's key.
    -   The right subtree of a node contains only nodes with keys **greater than** the node's key.
    -   Both the left and right subtrees must also be binary search trees.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg" style="zoom:67%;" />
    - **Input:** root = [2,1,3]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg" style="zoom:67%;" />
    - **Input:** root = [5,1,4,null,null,3,6]
    - **Output:** false
    - **Explanation:** The root node's value is 5 but its right child's value is 4.
- **Constraints:**
    -   The number of nodes in the tree is in the range `[1, 10^4]`.
    -   `-2^31 <= Node.val <= 2^31 - 1`

### Solution 1: in-order recursive

```java
public boolean isValidBST(TreeNode root) {
    Integer[] previous = {null};
    return helper(root, previous);
}

private boolean helper(TreeNode root, Integer[] previous) {
    if (root == null) {
        return true;
    }
    if (!helper(root.left, previous)) {
        return false;
    }
    if (previous[0] != null && previous[0] >= root.val) {
        return false;
    }
    previous[0] = root.val;
    return helper(root.right, previous);
}
```

Time Complexity: O(n)

Space Complexity: O(n)

### Solution 2: in-order iterative

```java
public boolean isValidBST(TreeNode root) {
    if (root == null) {
        return true;
    }
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode helper = root;
    Integer previous = null;
    while (helper != null || !stack.isEmpty()) {
        if (helper != null) {
            stack.offerFirst(helper);
            helper = helper.left;
        } else {
            helper = stack.pollFirst();
            if (previous != null && previous >= helper.val) {
                return false;
            }
            previous = helper.val;
            helper = helper.right;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 10. [LeetCode 230](https://leetcode.com/problems/kth-smallest-element-in-a-bst/) Kth Smallest Element in a BST (medium)

- Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg" style="zoom:67%;" />
    - **Input:** root = [3,1,4,null,2], k = 1
    - **Output:** 1
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg" style="zoom:67%;" />
    - **Input:** root = [5,3,6,2,4,null,null,1], k = 3
    - **Output:** 3
- **Constraints:**
    -   The number of nodes in the tree is `n`.
    -   `1 <= k <= n <= 10^4`
    -   `0 <= Node.val <= 10^4`
- **Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

### Solution

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

## 11. [LeetCode 235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) Lowest Common Ancestor of BST (easy)

- Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.
- According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="zoom:150%;" />
    - **Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
    - **Output:** 6
    - **Explanation:** The LCA of nodes 2 and 8 is 6.
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="zoom:150%;" />
    - **Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
    - **Output:** 2
    - **Explanation:** The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
- **Example 3:**
    - **Input:** root = [2,1], p = 2, q = 1
    - **Output:** 2
- **Constraints:**
    -   The number of nodes in the tree is in the range `[2, 10^5]`.
    -   `-10^9 <= Node.val <= 10^9`
    -   All `Node.val` are **unique**.
    -   `p != q`
    -   `p` and `q` will exist in the BST.

### Solution 1

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

### Soltuion 2

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    int small = Math.min(p.val, q.val);
    int large = Math.max(p.val, q.val);
    while (root != null) {
        if (root.val < small) {
            root = root.right;
        } else if (root.val > large) {
            root = root.left;
        } else {
            return root;
        }
    }
    return null;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 12. [LeetCode 208](https://leetcode.com/problems/implement-trie-prefix-tree/) Implement Trie (Prefix Tree) (medium)

- A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.
- Implement the Trie class:
    -   `Trie()` Initializes the trie object.
    -   `void insert(String word)` Inserts the string `word` into the trie.
    -   `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
    -   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
- **Example 1:**
    - **Input**
        - ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
        - `[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]`
    - **Output**
        - [null, null, true, false, true, null, true]
    - **Explanation**
        ```
        Trie trie = new Trie();
        trie.insert("apple");
        trie.search("apple");   // return True
        trie.search("app");     // return False
        trie.startsWith("app"); // return True
        trie.insert("app");
        trie.search("app");     // return True
        ```
- **Constraints:**
    -   `1 <= word.length, prefix.length <= 2000`
    -   `word` and `prefix` consist only of lowercase English letters.
    -   At most `3 * 10^4` calls **in total** will be made to `insert`, `search`, and `startsWith`.

### Solution

```java
class Trie {
    static class TrieNode {
        Map<Character, TrieNode> children;
        boolean isWord;
        int count;
        
        public TrieNode() {
            children = new HashMap<>();
        }
    }
    
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        if (search(word)) {
            return;
        }
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode next = current.children.get(word.charAt(i));
            if (next == null) {
                next = new TrieNode();
                current.children.put(word.charAt(i), next);
            }
            current = next;
            current.count++;
        }
        current.isWord = true;
    }
    
    public boolean search(String word) {
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode next = current.children.get(word.charAt(i));
            if (next == null) {
                return false;
            }
            current = next;
        }
        return current.isWord;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        for (int i = 0; i < prefix.length(); i++) {
            TrieNode next = current.children.get(prefix.charAt(i));
            if (next == null) {
                return false;
            }
            current = next;
        }
        return true;
    }
}
```

Time Complexity: O(length)

## 13. [LeetCode 211](https://leetcode.com/problems/design-add-and-search-words-data-structure/) Add and Search Word (medium)

- Design a data structure that supports adding new words and finding if a string matches any previously added string.
- Implement the `WordDictionary` class:
    -   `WordDictionary()` Initializes the object.
    -   `void addWord(word)` Adds `word`to the data structure, it can be matched later.
    -   `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.
- **Example:**
    - **Input**
        - ["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
        - `[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]`
    - **Output**
        - [null,null,null,null,false,true,true,true]
    - **Explanation**
        ```
        WordDictionary wordDictionary = new WordDictionary();
        wordDictionary.addWord("bad");
        wordDictionary.addWord("dad");
        wordDictionary.addWord("mad");
        wordDictionary.search("pad"); // return False
        wordDictionary.search("bad"); // return True
        wordDictionary.search(".ad"); // return True
        wordDictionary.search("b.."); // return True
        ```
- **Constraints:**
    -   `1 <= word.length <= 25`
    -   `word` in `addWord` consists of lowercase English letters.
    -   `word` in `search` consist of `'.'` or lowercase English letters.
    -   There will be at most `3` dots in `word` for `search` queries.
    -   At most `10^4` calls will be made to `addWord` and `search`.

### Solution

```java
class WordDictionary {
    static class TrieNode {
        Map<Character, TrieNode> children;
        boolean isWord;
        int count;
        
        public TrieNode() {
            children = new HashMap<>();
        }
    }
    
    private TrieNode root;
    
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        if (search(word)) {
            return;
        }
        TrieNode current = root;
        for (int i = 0; i < word.length(); i++) {
            TrieNode next = current.children.get(word.charAt(i));
            if (next == null) {
                next = new TrieNode();
                current.children.put(word.charAt(i), next);
            }
            current = next;
            current.count++;
        }
        current.isWord = true;
    }
    
    public boolean search(String word) {
        return search(word, root);
    }
    
    private boolean search(String word, TrieNode current) {
        for (int i = 0; i < word.length(); i++) {
            if (!current.children.containsKey(word.charAt(i))) {
                if (word.charAt(i) == '.') {
                    for (char c : current.children.keySet()) {
                        TrieNode next = current.children.get(c);
                        if (search(word.substring(i + 1), next)) {
                            return true;
                        }
                    }
                }
                return false;
            } else {
                current = current.children.get(word.charAt(i));
            }
        }
        return current.isWord;
    }
}
```

Time Complexity: O(length)

## 14. [LeetCode 212](https://leetcode.com/problems/word-search-ii/) Word Search II (hard)

- Given an `m x n` `board` of characters and a list of strings `words`, return _all words on the board_.
- Each word must be constructed from letters of sequentially adjacent cells, where **adjacent cells** are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/07/search1.jpg" style="zoom:67%;" />
    - **Input:** board = `[["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]]`, words = ["oath","pea","eat","rain"]
    - **Output:** ["eat","oath"]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/11/07/search2.jpg" style="zoom:67%;" />
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
    // assumption: words[i] consists of lowercase English letters
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