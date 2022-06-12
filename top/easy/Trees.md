# Easy Collection Day 3 (Trees)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/94/trees/

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

## 2. Validate Binary Search Tree (medium)

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

## 3. Symmetric Tree (easy)

[LeetCode 101](https://leetcode.com/problems/symmetric-tree/)

```java
public boolean isSymmetric(TreeNode root) {
    return helper(root, root);
}

private boolean helper(TreeNode root1, TreeNode root2) {
    if (root1 == null && root2 == null) {
        return true;
    }
    if (root1 == null || root2 == null) {
        return false;
    }
    if (root1.val != root2.val) {
        return false;
    }
    return helper(root1.left, root2.right) && helper(root1.right, root2.left);
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 4. Binary Tree Level Order Traversal (medium)

[LeetCode 102](https://leetcode.com/problems/binary-tree-level-order-traversal/)

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
        result.add(new ArrayList<>());
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

Space Complexity: O(height)

## 5. Convert Sorted Array to Binary Search Tree (easy)

[LeetCode 108](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

```java
public TreeNode sortedArrayToBST(int[] nums) {
    return helper(nums, 0, nums.length - 1);
}

private TreeNode helper(int[] nums, int left, int right) {
    if (left > right) {
        return null;
    }
    int mid = left + (right - left) / 2;
    TreeNode root = new TreeNode(nums[mid]);
    root.left = helper(nums, left, mid - 1);
    root.right = helper(nums, mid + 1, right);
    return root;
}
```

Time Complexity: O(n)

Space Complexity: O(logn)

