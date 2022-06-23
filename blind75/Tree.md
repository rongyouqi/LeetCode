# Blind 75 Day 7 (Tree)

## 1. Maximum Depth of Binary Tree (easy)

[LeetCode 104](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

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

## 2. Same Tree (easy)

[LeetCode 100](https://leetcode.com/problems/same-tree/)

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

## 3. Invert/Flip Binary Tree (easy)

[LeetCode 226](https://leetcode.com/problems/invert-binary-tree/)

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

## 4. Binary Tree Maximum Path Sum (hard)

[LeetCode 124](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

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

## 5. Binary Tree Level Order Traversal (medium)

[LeetCode 102](https://leetcode.com/problems/binary-tree-level-order-traversal/)

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

## 6. Serialize and Deserialize Binary Tree (hard)

[LeetCode 297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

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

## 7. Subtree of Another Tree (easy)

[LeetCode 572](https://leetcode.com/problems/subtree-of-another-tree/)

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

## 8. Construct Binary Tree from Preorder and Inorder Traversal (medium)

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

## 9. Validate Binary Search Tree (medium)

[LeetCode 98](https://leetcode.com/problems/validate-binary-search-tree/)

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

## 10. Kth Smallest Element in a BST (medium)

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

## 11. Lowest Common Ancestor of BST (easy)

[LeetCode 235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

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

## 12. Implement Trie (Prefix Tree) (medium)

[LeetCode 208](https://leetcode.com/problems/implement-trie-prefix-tree/)

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

## 13. Add and Search Word (medium)

[LeetCode 211](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

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

## 14. Word Search II (hard)

[LeetCode 212](https://leetcode.com/problems/word-search-ii/)

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

Time Complexity: O(mn*4<sup>L</sup>)

Space Complexity: O(wL)