# Grind 75 (169)

https://www.techinterviewhandbook.org/grind75?hours=40&weeks=4

https://www.techinterviewhandbook.org

# Easy (42)

## 1. [LeetCode 1](https://leetcode.com/problems/two-sum/) Two Sum #Array

- Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.
- You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.
- You can return the answer in any order.
- **Example 1:**
    - **Input:** nums = [2,7,11,15], target = 9
    - **Output:** [0,1]
    - **Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].
- **Example 2:**
    - **Input:** nums = [3,2,4], target = 6
    - **Output:** [1,2]
- **Example 3:**
    - **Input:** nums = [3,3], target = 6
    - **Output:** [0,1]
- **Constraints:**
    -   `2 <= nums.length <= 10^4`
    -   `-10^9 <= nums[i] <= 10^9`
    -   `-10^9 <= target <= 10^9`
    -   **Only one valid answer exists.**
- **Follow-up:** Can you come up with an algorithm that is less than `O(n^2)` time complexity?

### Solution: hashmap

```java
public int[] twoSum(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return null;
    }
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        map.put(nums[i], i);
    }
    return null;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 2. [LeetCode 20](https://leetcode.com/problems/valid-parentheses/) Valid Parentheses #Stack

- Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
- An input string is valid if:
    1.  Open brackets must be closed by the same type of brackets.
    2.  Open brackets must be closed in the correct order.
- **Example 1:**
    - **Input:** s = "()"
    - **Output:** true
- **Example 2:**
    - **Input:** s = "()[]{}"
    - **Output:** true
- **Example 3:**
    - **Input:** s = "(]"
    - **Output:** false
- **Constraints:**
    -   `1 <= s.length <= 10^4`
    -   `s` consists of parentheses only `'()[]{}'`.

### Solution: stack

```java
public boolean isValid(String s) {
    if (s == null || s.length() == 0) {
        return true;
    }
    Map<Character, Character> map = new HashMap<>();
    map.put(')', '(');
    map.put(']', '[');
    map.put('}', '{');
    Deque<Character> stack = new ArrayDeque<>();
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (map.containsKey(c)) {
            if (stack.isEmpty()) {
                return false;
            }
            if (stack.pollFirst() != map.get(c)) {
                return false;
            }
        } else {
            stack.offerFirst(c);
        }
    }
    return stack.isEmpty();
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 3. [LeetCode 21](https://leetcode.com/problems/merge-two-sorted-lists/) Merge Two Sorted Lists #LinkedList

- You are given the heads of two sorted linked lists `list1` and `list2`.
- Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.
- Return _the head of the merged linked list_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg" style="zoom:67%;" />
    - **Input:** list1 = [1,2,4], list2 = [1,3,4]
    - **Output:** [1,1,2,3,4,4]
- **Example 2:**
    - **Input:** list1 = [], list2 = []
    - **Output:** []
- **Example 3:**
    - **Input:** list1 = [], list2 = [0]
    - **Output:** [0]
- **Constraints:**
    -   The number of nodes in both lists is in the range `[0, 50]`.
    -   `-100 <= Node.val <= 100`
    -   Both `list1` and `list2` are sorted in **non-decreasing** order.

### Solution

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    if (list1 == null && list2 == null) {
        return null;
    }
    ListNode dummy = new ListNode(0);
    ListNode tail = dummy;
    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            tail.next = list1;
            list1 = list1.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
        }
        tail = tail.next;
    }
    if (list1 == null) {
        tail.next = list2;
    }
    if (list2 == null) {
        tail.next = list1;
    }
    return dummy.next;
}
```

Time Complexity: O(m + n)

Space Complexity: O(1)

## 4. [LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) Best Time to Buy and Sell Stock #Array 

- You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
- You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
- Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.
- **Example 1:**
    - **Input:** prices = [7,1,5,3,6,4]
    - **Output:** 5
    - **Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
        - Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
- **Example 2:**
    - **Input:** prices = [7,6,4,3,1]
    - **Output:** 0
    - **Explanation:** In this case, no transactions are done and the max profit = 0.
- **Constraints:**
    -   `1 <= prices.length <= 10^5`
    -   `0 <= prices[i] <= 10^4`

### Solution

```java
public int maxProfit(int[] prices) {
    if (prices == null || prices.length <= 1) {
        return 0;
    }
    int minPriceSoFar = Integer.MAX_VALUE, result = 0;
    for (int price : prices) {
        minPriceSoFar = Math.min(minPriceSoFar, price);
        result = Math.max(result, price - minPriceSoFar);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. [LeetCode 126](https://leetcode.com/problems/valid-palindrome/) Valid Palindrome #String

- A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.
- Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.
- **Example 1:**
    - **Input:** s = "A man, a plan, a canal: Panama"
    - **Output:** true
    - **Explanation:** "amanaplanacanalpanama" is a palindrome.
- **Example 2:**
    - **Input:** s = "race a car"
    - **Output:** false
    - **Explanation:** "raceacar" is not a palindrome.
- **Example 3:**
    - **Input:** s = " "
    - **Output:** true
    - **Explanation:** s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome.
- **Constraints:**
    -   `1 <= s.length <= 2 * 10^5`
    -   `s` consists only of printable ASCII characters.

### Solution

```java
public boolean isPalindrome(String s) {
    if (s == null || s.length() <= 1) {
        return true;
    }
    for (int left = 0, right = s.length() - 1; left < right; left++, right--) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right))) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. [LeetCode 226](https://leetcode.com/problems/invert-binary-tree/) Invert Binary Tree #BinaryTree

- Given the `root` of a binary tree, invert the tree, and return _its root_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg" style="zoom:67%;" />
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

## 7. [LeetCode 242](https://leetcode.com/problems/valid-anagram/) Valid Anagram #String 

- Given two strings `s` and `t`, return `true` _if_ `t` _is an anagram of_ `s`_, and_ `false` _otherwise_.
- An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
- **Example 1:**
    - **Input:** s = "anagram", t = "nagaram"
    - **Output:** true
- **Example 2:**
    - **Input:** s = "rat", t = "car"
    - **Output:** false
- **Constraints:**
    -   `1 <= s.length, t.length <= 5 * 10^4`
    -   `s` and `t` consist of lowercase English letters.
- **Follow up:** What if the inputs contain Unicode characters? How would you adapt your solution to such a case?

### Solution 1: sorting

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    char[] str1 = s.toCharArray();
    char[] str2 = t.toCharArray();
    Arrays.sort(str1);
    Arrays.sort(str2);
    return Arrays.equals(str1, str2);
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

### Solution 2: counter

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    int[] counter = new int[26];
    for (int i = 0; i < s.length(); i++) {
        counter[s.charAt(i) - 'a']++;
        counter[t.charAt(i) - 'a']--;
    }
    for (int count : counter) {
        if (count != 0) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

#### Follow up: hashmap as counter

```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) {
        return false;
    }
    Map<Character, Integer> map = new HashMap<>();
    for (int i = 0; i < s.length(); i++) {
        map.put(s.charAt(i), map.getOrDefault(s.charAt(i), 0) + 1);
        map.put(t.charAt(i), map.getOrDefault(t.charAt(i), 0) - 1);
    }
    return Collections.max(map.values()) == 0;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 8. [LeetCode 704](https://leetcode.com/problems/binary-search/) Binary Search #BinarySearch

- Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.
- You must write an algorithm with `O(logn)` runtime complexity.
- **Example 1:**
    - **Input:** nums = [-1,0,3,5,9,12], target = 9
    - **Output:** 4
    - **Explanation:** 9 exists in nums and its index is 4
- **Example 2:**
    - **Input:** nums = [-1,0,3,5,9,12], target = 2
    - **Output:** -1
    - **Explanation:** 2 does not exist in nums so return -1
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `-10^4 < nums[i], target < 10^4`
    -   All the integers in `nums` are **unique**.
    -   `nums` is sorted in ascending order.

### Solution

```java
public int search(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 9. [LeetCode 733](https://leetcode.com/problems/flood-fill/) Flood Fill #Graph

- An image is represented by an `m x n` integer grid `image` where `image[i][j]` represents the pixel value of the image.
- You are also given three integers `sr`, `sc`, and `color`. You should perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.
- To perform a **flood fill**, consider the starting pixel, plus any pixels connected **4-directionally** to the starting pixel of the same color as the starting pixel, plus any pixels connected **4-directionally** to those pixels (also with the same color), and so on. Replace the color of all of the aforementioned pixels with `color`.
- Return _the modified image after performing the flood fill_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/06/01/flood1-grid.jpg" style="zoom:67%;" />
    - **Input:** image = `[[1,1,1],[1,1,0],[1,0,1]]`, sr = 1, sc = 1, color = 2
    - **Output:** `[[2,2,2],[2,2,0],[2,0,1]]`
    - **Explanation:** From the center of the image with position (sr, sc) = (1, 1) (i.e., the red pixel), all pixels connected by a path of the same color as the starting pixel (i.e., the blue pixels) are colored with the new color.
        - Note the bottom corner is not colored 2, because it is not 4-directionally connected to the starting pixel.
- **Example 2:**
    - **Input:** image = `[[0,0,0],[0,0,0]]`, sr = 0, sc = 0, color = 0
    - **Output:** `[[0,0,0],[0,0,0]]`
    - **Explanation:** The starting pixel is already colored 0, so no changes are made to the image.
- **Constraints:**
    -   `m == image.length`
    -   `n == image[i].length`
    -   `1 <= m, n <= 50`
    -   `0 <= image[i][j], color < 2^16`
    -   `0 <= sr < m`
    -   `0 <= sc < n`

### Solution: dfs

```java
public int[][] floodFill(int[][] image, int sr, int sc, int color) {
    if (image[sr][sc] != color) {
        dfs(image, sr, sc, image[sr][sc], color);
    }
    return image;
}

private void dfs(int[][] image, int r, int c, int color, int newColor) {
    if (image[r][c] == color) {
        image[r][c] = newColor;
        if (r > 0) {
            dfs(image, r - 1, c, color, newColor);
        }
        if (c > 0) {
            dfs(image, r, c - 1, color, newColor);
        }
        if (r + 1 < image.length) {
            dfs(image, r + 1, c, color, newColor);
        }
        if (c + 1 < image[0].length) {
            dfs(image, r, c + 1, color, newColor);
        }
    }
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 10. [LeetCode 53](https://leetcode.com/problems/maximum-subarray/) Maximum Subarray #DynamicProgramming

- Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return _its sum_.
- A **subarray** is a **contiguous** part of an array.
- **Example 1:**
    - **Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
    - **Output:** 6
    - **Explanation:** [4,-1,2,1] has the largest sum = 6.
- **Example 2:**
    - **Input:** nums = [1]
    - **Output:** 1
- **Example 3:**
    - **Input:** nums = [5,4,-1,7,8]
    - **Output:** 23
- **Constraints:**
    -   `1 <= nums.length <= 10^5`
    -   `-10^4 <= nums[i] <= 10^4`
- **Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

### Solution 1: dynamic programming

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int max = nums[0], result = nums[0];
    for (int i = 1; i < nums.length; i++) {
        max = Math.max(max + nums[i], nums[i]);
        result = Math.max(result, max);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 2: divide and conquer

```java
class Solution {
    private int[] numsArray;
    
    public int maxSubArray(int[] nums) {
        numsArray = nums;
        
        // Our helper function is designed to solve this problem for
        // any array - so just call it using the entire input!
        return findBestSubarray(0, numsArray.length - 1);
    }
    
    private int findBestSubarray(int left, int right) {
        // Base case - empty array.
        if (left > right) {
            return Integer.MIN_VALUE;
        }
        
        int mid = Math.floorDiv(left + right, 2);
        int curr = 0;
        int bestLeftSum = 0;
        int bestRightSum = 0;
        
        // Iterate from the middle to the beginning.
        for (int i = mid - 1; i >= left; i--) {
            curr += numsArray[i];
            bestLeftSum = Math.max(bestLeftSum, curr);
        }
        
        // Reset curr and iterate from the middle to the end.
        curr = 0;
        for (int i = mid + 1; i <= right; i++) {
            curr += numsArray[i];
            bestRightSum = Math.max(bestRightSum, curr);
        }
        
        // The bestCombinedSum uses the middle element and the best
        // possible sum from each half.
        int bestCombinedSum = numsArray[mid] + bestLeftSum + bestRightSum;
        
        // Find the best subarray possible from both halves.
        int leftHalf = findBestSubarray(left, mid - 1);
        int rightHalf = findBestSubarray(mid + 1, right);
        
        // The largest of the 3 is the answer for any given input array.
        return Math.max(bestCombinedSum, Math.max(leftHalf, rightHalf));
    }
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

## 11. [LeetCode 235](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) Lowest Common Ancestor of a Binary Search Tree #BinarySearchTree

- Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.
- According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png"  />
    - **Input:** root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
    - **Output:** 6
    - **Explanation:** The LCA of nodes 2 and 8 is 6.
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png"  />
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

### Solution

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

Time Complexity: O(height)

Space Complexity: O(1)

## 12. [LeetCode 110](https://leetcode.com/problems/balanced-binary-tree/) Balanced Binary Tree #BinaryTree 

- Given a binary tree, determine if it is height-balanced.
- For this problem, a height-balanced binary tree is defined as:
    - a binary tree in which the left and right subtrees of _every_ node differ in height by no more than 1.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg" style="zoom:67%;" />
    - **Input:** root = [3,9,20,null,null,15,7]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg" style="zoom:67%;" />
    - **Input:** root = [1,2,2,3,3,null,null,4,4]
    - **Output:** false
- **Example 3:**
    - **Input:** root = []
    - **Output:** true
- **Constraints:**
    -   The number of nodes in the tree is in the range `[0, 5000]`.
    -   `-10^4 <= Node.val <= 10^4`

### Solution 1

```java
public boolean isBalanced(TreeNode root) {
    if (root == null) {
        return true;
    }
    int left = getHeight(root.left);
    int right = getHeight(root.right);
    if (Math.abs(left - right) > 1) {
        return false;
    }
    return isBalanced(root.left) && isBalanced(root.right);
}

private int getHeight(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return Math.max(getHeight(root.left), getHeight(root.right)) + 1;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(height)

### Solution 2

```java
public boolean isBalanced(TreeNode root) {
    if (root == null) {
        return true;
    }
    return helper(root) != -1;
}

private int helper(TreeNode root) {
    if (root == null) {
        return 0;
    }
    int left = helper(root.left);
    if (left == -1) {
        return -1;
    }
    int right = helper(root.right);
    if (right == -1) {
        return -1;
    }
    if (Math.abs(left - right) > 1) {
        return -1;
    }
    return Math.max(left, right) + 1;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 13. [LeetCode 141](https://leetcode.com/problems/linked-list-cycle/) Linked List Cycle #LinkedList 

- Given `head`, the head of a linked list, determine if the linked list has a cycle in it.
- There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.
- Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png" style="zoom:67%;" />
    - **Input:** head = [3,2,0,-4], pos = 1
    - **Output:** true
    - **Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png" style="zoom:67%;" />
    - **Input:** head = [1,2], pos = 0
    - **Output:** true
    - **Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.
- **Example 3:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png" style="zoom:67%;" />
    - **Input:** head = [1], pos = -1
    - **Output:** false
    - **Explanation:** There is no cycle in the linked list.
- **Constraints:**
    -   The number of the nodes in the list is in the range `[0, 10^4]`.
    -   `-10^5 <= Node.val <= 10^5`
    -   `pos` is `-1` or a **valid index** in the linked-list.
- **Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

### Solution

```java
public boolean hasCycle(ListNode head) {
    if (head == null) {
        return false;
    }
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true;
        }
    }
    return false;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 13.* [LeetCode 142](https://leetcode.com/problems/linked-list-cycle-ii/) Linked List Cycle II (medium) #LinkedList 

- Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.
- There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.
- **Do not modify** the linked list.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png" style="zoom:67%;" />
    - **Input:** head = [3,2,0,-4], pos = 1
    - **Output:** tail connects to node index 1
    - **Explanation:** There is a cycle in the linked list, where tail connects to the second node.
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png" style="zoom:67%;" />
    - **Input:** head = [1,2], pos = 0
    - **Output:** tail connects to node index 0
    - **Explanation:** There is a cycle in the linked list, where tail connects to the first node.
- **Example 3:**
    - <img src="https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png" style="zoom:67%;" />
    - **Input:** head = [1], pos = -1
    - **Output:** no cycle
    - **Explanation:** There is no cycle in the linked list.
- **Constraints:**
    -   The number of the nodes in the list is in the range `[0, 10^4]`.
    -   `-10^5 <= Node.val <= 10^5`
    -   `pos` is `-1` or a **valid index** in the linked-list.
- **Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

### Solution

```java
public ListNode detectCycle(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            while (head != fast) {
                fast = fast.next;
                head = head.next;
            }
            return head;
        }
    }
    return null;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 14. [LeetCode 232](https://leetcode.com/problems/implement-queue-using-stacks/) Implement Queue using Stacks #Stack 

- Implement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).
- Implement the `MyQueue` class:
    -   `void push(int x)` Pushes element x to the back of the queue.
    -   `int pop()` Removes the element from the front of the queue and returns it.
    -   `int peek()` Returns the element at the front of the queue.
    -   `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.
- **Notes:**
    -   You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
    -   Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.
- **Example 1:**
    - **Input**
        - ["MyQueue", "push", "push", "peek", "pop", "empty"]
        - `[[], [1], [2], [], [], []]`
    - **Output**
        - [null, null, null, 1, 1, false]
    - **Explanation**
        ```
        MyQueue myQueue = new MyQueue();
        myQueue.push(1); // queue is: [1]
        myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
        myQueue.peek(); // return 1
        myQueue.pop(); // return 1, queue is [2]
        myQueue.empty(); // return false
        ```
- **Constraints:**
    -   `1 <= x <= 9`
    -   At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
    -   All the calls to `pop` and `peek` are valid.
- **Follow-up:** Can you implement the queue such that each operation is **[amortized](https://en.wikipedia.org/wiki/Amortized_analysis)** `O(1)` time complexity? In other words, performing `n` operations will take overall `O(n)` time even if one of those operations may take longer.

### Solution

```java
class MyQueue {
    private Deque<Integer> input;
    private Deque<Integer> output;

    public MyQueue() {
        input = new ArrayDeque<>();
        output = new ArrayDeque<>();
    }
    
    public void push(int x) {
        input.offerFirst(x);
    }
    
    public int pop() {
        peek();
        return output.pollFirst();
    }
    
    public int peek() {
        if (output.isEmpty()) {
            while (!input.isEmpty()) {
                output.offerFirst(input.pollFirst());
            }
        }
        return output.peekFirst();
    }
    
    public boolean empty() {
        return input.isEmpty() && output.isEmpty();
    }
}
```

## 15. [LeetCode 278](https://leetcode.com/problems/first-bad-version/) First Bad Version #BinarySearch

- You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.
- Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.
- You are given an API `bool isBadVersion(version)` which returns whether `version` is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.
- **Example 1:**
    - **Input:** n = 5, bad = 4
    - **Output:** 4
    - **Explanation:**
        - call isBadVersion(3) -> false
        - call isBadVersion(5) -> true
        - call isBadVersion(4) -> true
        - Then 4 is the first bad version.
- **Example 2:**
    - **Input:** n = 1, bad = 1
    - **Output:** 1
- **Constraints:**
    -   `1 <= bad <= n <= 2^31 - 1`

### Solution: binary search

```java
public int firstBadVersion(int n) {
    int left = 1, right = n;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (isBadVersion(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 16. [LeetCode 383](https://leetcode.com/problems/ransom-note/) Ransom Note #HashTable

- Given two strings `ransomNote` and `magazine`, return `true` _if_ `ransomNote` _can be constructed by using the letters from_ `magazine` _and_ `false` _otherwise_.
- Each letter in `magazine` can only be used once in `ransomNote`.
- **Example 1:**
    - **Input:** ransomNote = "a", magazine = "b"
    - **Output:** false
- **Example 2:**
    - **Input:** ransomNote = "aa", magazine = "ab"
    - **Output:** false
- **Example 3:**
    - **Input:** ransomNote = "aa", magazine = "aab"
    - **Output:** true
- **Constraints:**
    -   `1 <= ransomNote.length, magazine.length <= 10^5`
    -   `ransomNote` and `magazine` consist of lowercase English letters.

### Solution

```java
public boolean canConstruct(String ransomNote, String magazine) {
    int[] counter = new int[26];
    for (char c : magazine.toCharArray()) {
        counter[c - 'a']++;
    }
    for (char c : ransomNote.toCharArray()) {
        if (counter[c - 'a'] == 0) {
            return false;
        }
        counter[c - 'a']--;
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 17. [LeetCode 70](https://leetcode.com/problems/climbing-stairs/) Climbing Stairs #DynamicProgramming

You are climbing a staircase. It takes `n` steps to reach the top.
- Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?
- **Example 1:**
    - **Input:** n = 2
    - **Output:** 2
    - **Explanation:** There are two ways to climb to the top.
        1. 1 step + 1 step
        2. 2 steps
- **Example 2:**
    - **Input:** n = 3
    - **Output:** 3
    - **Explanation:** There are three ways to climb to the top.
        1. 1 step + 1 step + 1 step
        2. 1 step + 2 steps
        3. 2 steps + 1 step
- **Constraints:**
    -   `1 <= n <= 45`

### Solution: dynamic programming

```java
public int climbStairs(int n) {
    if (n == 0 || n == 1 || n == 2) {
        return n;
    }
    int twoStep = 1, oneStep = 2, result = oneStep;
    for (int i = 3; i <= n; i++) {
        result = oneStep + twoStep;
        twoStep = oneStep;
        oneStep = result;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 18. [LeetCode 409](https://leetcode.com/problems/longest-palindrome/) Longest Palindrome #String

- Given a string `s` which consists of lowercase or uppercase letters, return _the length of the **longest palindrome**_ that can be built with those letters.
- Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome here.
- **Example 1:**
    - **Input:** s = "abccccdd"
    - **Output:** 7
    - **Explanation:** One longest palindrome that can be built is "dccaccd", whose length is 7.
- **Example 2:**
    - **Input:** s = "a"
    - **Output:** 1
    - **Explanation:** The longest palindrome that can be built is "a", whose length is 1.
- **Constraints:**
    -   `1 <= s.length <= 2000`
    -   `s` consists of lowercase **and/or** uppercase English letters only.

### Solution

```java
public int longestPalindrome(String s) {
    int[] counter = new int[128];
    for (char c : s.toCharArray()) {
        counter[c]++;
    }
    int result = 0;
    for (int num : counter) {
        result += num / 2 * 2;
        if (result % 2 == 0 && num % 2 == 1) {
            result++;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 19. [LeetCode 206](https://leetcode.com/problems/reverse-linked-list/) Reverse Linked List #LinkedList

- Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4,5]
    - **Output:** [5,4,3,2,1]
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2]
    - **Output:** [2,1]
- **Example 3:**
    - **Input:** head = []
    - **Output:** []
- **Constraints:**
    -   The number of nodes in the list is the range `[0, 5000]`.
    -   `-5000 <= Node.val <= 5000`
- **Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

### Solution 1: iterative

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode pre = null;
    ListNode cur = head;
    ListNode nxt = null;
    while (cur != null) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    return pre;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

### Solution 2: recursive

```java
public ListNode reverseList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    ListNode newHead = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 20. [LeetCode 169](https://leetcode.com/problems/majority-element/) Majority Element #Array

- Given an array `nums` of size `n`, return _the majority element_.
- The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.
- **Example 1:**
    - **Input:** nums = [3,2,3]
    - **Output:** 3
- **Example 2:**
    - **Input:** nums = [2,2,1,1,1,2,2]
    - **Output:** 2
- **Constraints:**
    -   `n == nums.length`
    -   `1 <= n <= 5 * 10^4`
    -   `-10^9 <= nums[i] <= 10^9`
- **Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

### Solution

```java
public int majorityElement(int[] nums) {
    int count = 0;
    Integer result = null;
    for (int num : nums) {
        if (count == 0) {
            result = num;
        }
        count += num == result ? 1 : -1;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 21. [LeetCode 67](https://leetcode.com/problems/add-binary/) Add Binary #Binary

- Given two binary strings `a` and `b`, return _their sum as a binary string_.
- **Example 1:**
    - **Input:** a = "11", b = "1"
    - **Output:** "100"
- **Example 2:**
    - **Input:** a = "1010", b = "1011"
    - **Output:** "10101"
- **Constraints:**
    -   `1 <= a.length, b.length <= 10^4`
    -   `a` and `b` consist only of `'0'` or `'1'` characters.
    -   Each string does not contain leading zeros except for the zero itself.

### Intuition (Runtime Error)

1. convert a and b into integers
2. compute the sum
3. convert the sum back into binary form

```java
public String addBinary(String a, String b) {
    return Integer.toBinaryString(Integer.parseInt(a, 2) + Integer.parseInt(b, 2));
}
```

Time Complexity: O(m + n)

Space Complexity: O(m + n)

### Solution 1: bit-by-bit computation

```java
public String addBinary(String a, String b) {
    if (a.length() < b.length()) {
        return addBinary(b, a);
    }
    StringBuilder sb = new StringBuilder();
    int carry = 0, j = b.length() - 1;
    for (int i = a.length() - 1; i >= 0; i--) {
        if (a.charAt(i) == '1') {
            carry++;
        }
        if (j >= 0 && b.charAt(j--) == '1') {
            carry++;
        }
        if (carry % 2 == 0) {
            sb.append('0');
        } else {
            sb.append('1');
        }
        carry /= 2;
    }
    if (carry == 1) {
        sb.append('1');
    }
    sb.reverse();
    return sb.toString();
}
```

Time Complexity: O(max(m, n))

Space Complexity: O(max(m, n))

### Solution 2: bit manipulation

```java
import java.math.BigInteger;
class Solution {
    public String addBinary(String a, String b) {
        BigInteger x = new BigInteger(a, 2);
        BigInteger y = new BigInteger(b, 2);
        BigInteger zero = new BigInteger("0", 2);
        BigInteger carry, answer;
        while (y.compareTo(zero) != 0) {
            answer = x.xor(y);
            carry = x.and(y).shiftLeft(1);
            x = answer;
            y = carry;
        }
        return x.toString(2);
    }
}
```

```python
def addBinary(self, a: str, b: str) -> str:
    x, y = int(a, 2), int(b, 2)
    while y:
        x, y = x ^ y, (x & y) << 1
    return bin(x)[2:]
```

Time Complexity: O(m + n)

Space Complexity: O(max(m, n))

## 22. [LeetCode 543](https://leetcode.com/problems/diameter-of-binary-tree/) Diameter of Binary Tree #BinaryTree

- Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.
- The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.
- The **length** of a path between two nodes is represented by the number of edges between them.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg" style="zoom:67%;" />
    - **Input:** root = [1,2,3,4,5]
    - **Output:** 3
    - **Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].
- **Example 2:**
    - **Input:** root = [1,2]
    - **Output:** 1
- **Constraints:**
    -   The number of nodes in the tree is in the range `[1, 10^4]`.
    -   `-100 <= Node.val <= 100`

### Solution

```java
public int diameterOfBinaryTree(TreeNode root) {
    int[] result = new int[]{Integer.MIN_VALUE};
    helper(root, result);
    return result[0];
}

private int helper(TreeNode root, int[] result) {
    if (root == null) {
        return 0;
    }
    int left = helper(root.left, result);
    int right = helper(root.right, result);
    result[0] = Math.max(result[0], left + right);
    return Math.max(left, right) + 1;
}
```

Time Complexity: O(n)

Space Complexity: O(height)

## 23. [LeetCode 876](https://leetcode.com/problems/middle-of-the-linked-list/) Middle of the Linked List #LinkedList

- Given the `head` of a singly linked list, return _the middle node of the linked list_.
- If there are two middle nodes, return **the second middle** node.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4,5]
    - **Output:** [3,4,5]
    - **Explanation:** The middle node of the list is node 3.
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,3,4,5,6]
    - **Output:** [4,5,6]
    - **Explanation:** Since the list has two middle nodes with values 3 and 4, we return the second one.
- **Constraints:**
    -   The number of nodes in the list is in the range `[1, 100]`.
    -   `1 <= Node.val <= 100`

### Solution

```java
public ListNode middleNode(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 24. [LeetCode 104](https://leetcode.com/problems/maximum-depth-of-binary-tree/) Maximum Depth of Binary Tree #BinaryTree

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

## 25. [LeetCode 217](https://leetcode.com/problems/contains-duplicate/) Contains Duplicate #Array

- Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false`if every element is distinct.
- **Example 1:**
    - **Input:** nums = [1,2,3,1]
    - **Output:** true
- **Example 2:**
    - **Input:** nums = [1,2,3,4]
    - **Output:** false
- **Example 3:**
    - **Input:** nums = [1,1,1,3,3,4,3,2,4,2]
    - **Output:** true
- **Constraints:**
    -   `1 <= nums.length <= 10^5`
    -   `-10^9 <= nums[i] <= 10^9`

### Solution

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>();
    for (int num : nums) {
        if (set.contains(num)) {
            return true;
        }
        set.add(num);
    }
    return false;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 25.* [LeetCode 219](https://leetcode.com/problems/contains-duplicate-ii/) Contains Duplicate II (easy) #Array

- Given an integer array `nums` and an integer `k`, return `true` if there are two **distinct indices** `i` and `j` in the array such that `nums[i] == nums[j]` and `abs(i - j) <= k`.
- **Example 1:**
    - **Input:** nums = [1,2,3,1], k = 3
    - **Output:** true
- **Example 2:**
    - **Input:** nums = [1,0,1,1], k = 1
    - **Output:** true
- **Example 3:**
    - **Input:** nums = [1,2,3,1,2,3], k = 2
    - **Output:** false
- **Constraints:**
    -   `1 <= nums.length <= 10^5`
    -   `-10^9 <= nums[i] <= 10^9`
    -   `0 <= k <= 10^5`

### Solution

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {
    Set<Integer> set = new HashSet<>();
    for (int i = 0; i < nums.length; i++) {
        if (set.contains(nums[i])) {
            return true;
        }
        set.add(nums[i]);
        if (set.size() > k) {
            set.remove(nums[i - k]);
        }
    }
    return false;
}
```

Time Complexity: O(n)

Space Complexity: O(min(n, k))

## 25.* [LeetCode 220](https://leetcode.com/problems/contains-duplicate-iii/) Contains Duplicate III (medium) #Array

- Given an integer array `nums` and two integers `k` and `t`, return `true` if there are **two distinct indices** `i`and `j` in the array such that `abs(nums[i] - nums[j]) <= t` and `abs(i - j) <= k`.
- **Example 1:**
    - **Input:** nums = [1,2,3,1], k = 3, t = 0
    - **Output:** true
- **Example 2:**
    - **Input:** nums = [1,0,1,1], k = 1, t = 2
    - **Output:** true
- **Example 3:**
    - **Input:** nums = [1,5,9,1,5,9], k = 2, t = 3
    - **Output:** false
- **Constraints:**
    -   `1 <= nums.length <= 2 * 10^4`
    -   `-2^31 <= nums[i] <= 2^31 - 1`
    -   `0 <= k <= 10^4`
    -   `0 <= t <= 2^31 - 1`

### Solution

```java
public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
    Map<Long, Long> map = new HashMap<>();
    long width = (long)t + 1;
    for (int i = 0; i < nums.length; i++) {
        long id = getID(nums[i], width);
        if (map.containsKey(id)) {
            return true;
        }
        if (map.containsKey(id - 1) && Math.abs(nums[i] - map.get(id - 1)) < width) {
            return true;
        }
        if (map.containsKey(id + 1) && Math.abs(nums[i] - map.get(id + 1)) < width) {
            return true;
        }
        map.put(id, (long)nums[i]);
        if (i >= k) {
            map.remove(getID(nums[i - k], width));
        }
    }
    return false;
}

private long getID(long i, long w) {
    return i < 0 ? (i + 1) / w - 1 : i / w;
}
```

Time Complexity: O(n)

Space Complexity: O(min(n, k))

## 26. [LeetCode 252](https://leetcode.com/problems/meeting-rooms/) Meeting Rooms #Array

- Given an array of meeting time `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.
- **Example 1:**
    - **Input:** intervals = `[[0,30],[5,10],[15,20]]`
    - **Output:** false
- **Example 2:**
    - **Input:** intervals = `[[7,10],[2,4]]`
    - **Output:** true
- **Constraints:**
    -   `0 <= intervals.length <= 10^4`
    -   `intervals[i].length == 2`
    -   `0 <= starti < endi <= 10^6`

### Solution

```java
public boolean canAttendMeetings(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    for (int i = 0; i < intervals.length - 1; i++) {
        if (intervals[i][1] > intervals[i + 1][0]) {
            return false;
        }
    }
    return true;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(logn)

```java
public boolean canAttendMeetings(int[][] intervals) {
    try {
        Arrays.sort(intervals, (a, b) -> {
            if (a[1] <= b[0]) {
                return -1;
            } else if (a[0] >= b[1]) {
                return 1;
            }
            throw new RuntimeException("overlap detected");
        });
        return true;
    }  catch (RuntimeException e) {
        return false;
    }
}
```

## 27. [LeetCode 13](https://leetcode.com/problems/roman-to-integer/) Roman to Integer #Math

- Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.
| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| V      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |
- For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.
- Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:
    -   `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
    -   `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
    -   `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.
- Given a roman numeral, convert it to an integer.
- **Example 1:**
    - **Input:** s = "III"
    - **Output:** 3
    - **Explanation:** III = 3.
- **Example 2:**
    - **Input:** s = "LVIII"
    - **Output:** 58
    - **Explanation:** L = 50, V= 5, III = 3.
- **Example 3:**
    - **Input:** s = "MCMXCIV"
    - **Output:** 1994
    - **Explanation:** M = 1000, CM = 900, XC = 90 and IV = 4.
- **Constraints:**
    -   `1 <= s.length <= 15`
    -   `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
    -   It is **guaranteed** that `s` is a valid roman numeral in the range `[1, 3999]`.

### Solution

```java
public int romanToInt(String s) {
    int result = 0, last = 1000;
    for (int i = 0; i < s.length(); i++) {
        int value = getValue(s.charAt(i));
        if (value > last) {
            result -= last * 2;
        }
        result += value;
        last = value;
    }
    return result;
}

private int getValue(char c) {
    switch(c) {
        case 'I' : return 1;
        case 'V' : return 5;
        case 'X' : return 10;
        case 'L' : return 50;
        case 'C' : return 100;
        case 'D' : return 500;
        case 'M' : return 1000;
        default : return 0;
    }
}
```

```java
public int romanToInt(String s) {
    Map<Character, Integer> map = new HashMap<>();
    map.put('I', 1);
    map.put('V', 5);
    map.put('X', 10);
    map.put('L', 50);
    map.put('C', 100);
    map.put('D', 500);
    map.put('M', 1000);
    int result = 0, last = 1000;
    for (int i = 0; i < s.length(); i++) {
        int value = map.get(s.charAt(i));
        result += value;
        if (value > last) {
            result -= last * 2;
        }
        last = value;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 28. [LeetCode 844](https://leetcode.com/problems/backspace-string-compare/) Backspace String Compare #Stack

- Given two strings `s` and `t`, return `true` _if they are equal when both are typed into empty text editors_. `'#'` means a backspace character.
- Note that after backspacing an empty text, the text will continue empty.
- **Example 1:**
    - **Input:** s = "ab#c", t = "ad#c"
    - **Output:** true
    - **Explanation:** Both s and t become "ac".
- **Example 2:**
    - **Input:** s = "ab##", t = "c#d#"
    - **Output:** true
    - **Explanation:** Both s and t become "".
- **Example 3:**
    - **Input:** s = "a#c", t = "b"
    - **Output:** false
    - **Explanation:** s becomes "c" while t becomes "b".
- **Constraints:**
    -   `1 <= s.length, t.length <= 200`
    -   `s` and `t` only contain lowercase letters and `'#'` characters.
- **Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

### Solution 1: stack

```java
public boolean backspaceCompare(String s, String t) {
    return build(s).equals(build(t));
}

private String build(String s) {
    Deque<Character> stack = new ArrayDeque<>();
    for (char c : s.toCharArray()) {
        if (c != '#') {
            stack.offerFirst(c);
        } else if (!stack.isEmpty()) {
            stack.pollFirst();
        }
    }
    return String.valueOf(stack);
}
```

Time Complexity: O(m + n)

Space Complexity: O(m + n)

### Solution 2: two pointers

```java
public boolean backspaceCompare(String s, String t) {
    int i = s.length() - 1, j = t.length() - 1;
    int skipS = 0, skipT = 0;
    while (i >= 0 || j >= 0) {
        while (i >= 0) {
            if (s.charAt(i) == '#') {
                skipS++;
                i--;
            } else if (skipS > 0) {
                skipS--;
                i--;
            } else {
                break;
            }
        }
        while (j >= 0) {
            if (t.charAt(j) == '#') {
                skipT++;
                j--;
            } else if (skipT > 0) {
                skipT--;
                j--;
            } else {
                break;
            }
        }
        if (i >= 0 && j >= 0 && s.charAt(i) != t.charAt(j)) {
            return false;
        }
        if ((i >= 0) != (j >= 0)) {
            return false;
        }
        i--;
        j--;
    }
    return true;
}
```

Time Complexity: O(m + n)

Space Complexity: O(1)

## 29. [LeetCode 338](https://leetcode.com/problems/counting-bits/) Counting Bits #Binary

- Given an integer `n`, return _an array_ `ans` _of length_ `n + 1` _such that for each_ `i` (`0 <= i <= n`)_,_ `ans[i]` _is the **number of**_ `1`_**'s** in the binary representation of_ `i`.
- **Example 1:**
    - **Input:** n = 2
    - **Output:** [0,1,1]
    - **Explanation:**
        - 0 --> 0
        - 1 --> 1
        - 2 --> 10
- **Example 2:**
    - **Input:** n = 5
    - **Output:** [0,1,1,2,1,2]
    - **Explanation:**
        - 0 --> 0
        - 1 --> 1
        - 2 --> 10
        - 3 --> 11
        - 4 --> 100
        - 5 --> 101
- **Constraints:**
    -   `0 <= n <= 10^5`
- **Follow up:**
    -   It is very easy to come up with a solution with a runtime of `O(nlogn)`. Can you do it in linear time `O(n)` and possibly in a single pass?
    -   Can you do it without using any built-in function (i.e., like `__builtin_popcount` in C++)?

### Solution

```java
public int[] countBits(int n) {
    int[] result = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        result[i] = result[i >> 1] + (i & 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 30. [LeetCode 100](https://leetcode.com/problems/same-tree/) Same Tree #BinaryTree

- Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.
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

## 31. [LeetCode 191](https://leetcode.com/problems/number-of-1-bits/) Number of 1 Bits #Binary

- Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the [Hamming weight](http://en.wikipedia.org/wiki/Hamming_weight)).
- **Note:**
    -   Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
    -   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 3**, the input represents the signed integer. `-3`.
- **Example 1:**
    - **Input:** n = 00000000000000000000000000001011
    - **Output:** 3
    - **Explanation:** The input binary string **00000000000000000000000000001011** has a total of three '1' bits.
- **Example 2:**
    - **Input:** n = 00000000000000000000000010000000
    - **Output:** 1
    - **Explanation:** The input binary string **00000000000000000000000010000000** has a total of one '1' bit.
- **Example 3:**
    - **Input:** n = 11111111111111111111111111111101
    - **Output:** 31
    - **Explanation:** The input binary string **11111111111111111111111111111101** has a total of thirty one '1' bits.
- **Constraints:**
    -   The input must be a **binary string** of length `32`.
- **Follow up:** If this function is called many times, how would you optimize it?

### Solution

```java
public int hammingWeight(int n) {
    int result = 0;
    while (n != 0) {
        result++;
        n &= (n - 1);
    }
    return result;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 32. [LeetCode 14](https://leetcode.com/problems/longest-common-prefix/) Longest Common Prefix #String

- Write a function to find the longest common prefix string amongst an array of strings.
- If there is no common prefix, return an empty string `""`.
- **Example 1:**
    - **Input:** strs = ["flower","flow","flight"]
    - **Output:** "fl"
- **Example 2:**
    - **Input:** strs = ["dog","racecar","car"]
    - **Output:** ""
    - **Explanation:** There is no common prefix among the input strings.
- **Constraints:**
    -   `1 <= strs.length <= 200`
    -   `0 <= strs[i].length <= 200`
    -   `strs[i]` consists of only lowercase English letters.

### Solution

```java
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) {
        return "";
    }
    for (int i = 0; i < strs[0].length() ; i++){
        char c = strs[0].charAt(i);
        for (int j = 1; j < strs.length; j ++) {
            if (i == strs[j].length() || strs[j].charAt(i) != c) {
                return strs[0].substring(0, i);
            }
        }
    }
    return strs[0];
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 33. [LeetCode 136](https://leetcode.com/problems/single-number/) Single Number #Binary

- Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.
- You must implement a solution with a linear runtime complexity and use only constant extra space.
- **Example 1:**
    - **Input:** nums = [2,2,1]
    - **Output:** 1
- **Example 2:**
    - **Input:** nums = [4,1,2,1,2]
    - **Output:** 4
- **Example 3:**
    - **Input:** nums = [1]
    - **Output:** 1
- **Constraints:**
    -   `1 <= nums.length <= 3 * 10^4`
    -   `-3 * 10^4 <= nums[i] <= 3 * 10^4`
    -   Each element in the array appears twice except for one element which appears only once.

### Solution

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 34. [LeetCode 234](https://leetcode.com/problems/palindrome-linked-list/) Palindrome Linked List #LinkedList

- Given the `head` of a singly linked list, return `true` if it is a palindrome.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2,2,1]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg" style="zoom:67%;" />
    - **Input:** head = [1,2]
    - **Output:** false
- **Constraints:**
    -   The number of nodes in the list is in the range `[1, 10^5]`.
    -   `0 <= Node.val <= 9`
- **Follow up:** Could you do it in `O(n)` time and `O(1)` space?

### Solution

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) {
        return true;
    }
    ListNode middle = findMiddle(head);
    ListNode newHead = reverse(middle.next);
    while (newHead != null) {
        if (head.val != newHead.val) {
            return false;
        }
        head = head.next;
        newHead = newHead.next;
    }
    return true;
}

private ListNode findMiddle(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

private ListNode reverse(ListNode head) {
    if (head == null) {
        return null;
    }
    ListNode pre = null, cur = head, nxt = null;
    while (cur != null) {
        nxt = cur.next;
        cur.next = pre;
        pre = cur;
        cur = nxt;
    }
    return pre;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 35. [LeetCode 283](https://leetcode.com/problems/move-zeroes/) Move Zeroes #Array

- Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.
- **Note** that you must do this in-place without making a copy of the array.
- **Example 1:**
    - **Input:** nums = [0,1,0,3,12]
    - **Output:** [1,3,12,0,0]
- **Example 2:**
    - **Input:** nums = [0]
    - **Output:** [0]
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `-2^31 <= nums[i] <= 2^31 - 1`
- **Follow up:** Could you minimize the total number of operations done?

### Solution

```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length <= 1) {
        return;
    }
    for (int i = 0, j = 0; j < nums.length; j++) {
        if (nums[j] != 0) {
            swap(nums, i++, j);
        }
    }
}

private void swap(int[] nums, int x, int y) {
    int temp = nums[x];
    nums[x] = nums[y];
    nums[y] = temp;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 36. [LeetCode 101](https://leetcode.com/problems/symmetric-tree/) Symmetric Tree #BinaryTree

- Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)
    - **Input:** root = [1,2,2,3,4,4,3]
    - **Output:** true
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)
    - **Input:** root = [1,2,2,null,3,null,3]
    - **Output:** false
- **Constraints:**
    -   The number of nodes in the tree is in the range `[1, 1000]`.
    -   `-100 <= Node.val <= 100`
- **Follow up:** Could you solve it both recursively and iteratively?

### Solution 1: recursive

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

### Solution 2: iterative

```java
public boolean isSymmetric(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    queue.offer(root);
    while (!queue.isEmpty()) {
        TreeNode root1 = queue.poll();
        TreeNode root2 = queue.poll();
        if (root1 == null && root2 == null) {
            continue;
        }
        if (root1 == null || root2 == null) {
            return false;
        }
        if (root1.val != root2.val) {
            return false;
        }
        queue.add(root1.left);
        queue.add(root2.right);
        queue.add(root1.right);
        queue.add(root2.left);
    }
    return true;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 37. [LeetCode 268](https://leetcode.com/problems/missing-number/) Missing Number #Binary

- Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._
- **Example 1:**
    - **Input:** nums = [3,0,1]
    - **Output:** 2
    - **Explanation:** n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
- **Example 2:**
    - **Input:** nums = [0,1]
    - **Output:** 2
    - **Explanation:** n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
- **Example 3:**
    - **Input:** nums = [9,6,4,2,3,5,7,0,1]
    - **Output:** 8
    - **Explanation:** n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
- **Constraints:**
    -   `n == nums.length`
    -   `1 <= n <= 10^4`
    -   `0 <= nums[i] <= n`
    -   All the numbers of `nums` are **unique**.
- **Follow up:** Could you implement a solution using only `O(1)` extra space complexity and `O(n)` runtime complexity?

### Solution

```java
public int missingNumber(int[] nums) {
    int xor = 0;
    for (int num : nums) {
        xor ^= num;
    }
    for (int i = 1; i <= nums.length; i++) {
        xor ^= i;
    }
    return xor;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 38. [LeetCode 9](https://leetcode.com/problems/palindrome-number/) Palindrome Number #Math

- Given an integer `x`, return `true` if `x` is palindrome integer.
- An integer is a **palindrome** when it reads the same backward as forward.
    -   For example, `121` is a palindrome while `123` is not.
- **Example 1:**
    - **Input:** x = 121
    - **Output:** true
    - **Explanation:** 121 reads as 121 from left to right and from right to left.
- **Example 2:**
    - **Input:** x = -121
    - **Output:** false
    - **Explanation:** From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
- **Example 3:**
    - **Input:** x = 10
    - **Output:** false
    - **Explanation:** Reads 01 from right to left. Therefore it is not a palindrome.
- **Constraints:**
    -   `-2^31 <= x <= 2^31 - 1`
- **Follow up:** Could you solve it without converting the integer to a string?

### Solution

```java
public boolean isPalindrome(int x) {
    if (x < 0 || (x % 10 == 0 && x != 0)) {
        return false;
    }
    int num = 0;
    while (x > num) {
        num = num * 10 + x % 10;
        x /= 10;
    }
    return x == num || x == num / 10;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 39. [LeetCode 108](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/) Convert Sorted Array to Binary Search Tree #BinarySearchTree

- Given an integer array `nums` where the elements are sorted in **ascending order**, convert _it to a **height-balanced** binary search tree_.
- A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg" style="zoom:67%;" />
    - **Input:** nums = [-10,-3,0,5,9]
    - **Output:** [0,-3,9,-10,null,5]
    - **Explanation:** [0,-10,5,null,-3,null,9] is also accepted:
        - <img src="https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg" style="zoom:67%;" />
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/18/btree.jpg" style="zoom:67%;" />
    - **Input:** nums = [1,3]
    - **Output:** [3,1]
    - **Explanation:** [1,null,3] and [3,1] are both height-balanced BSTs.
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `-10^4 <= nums[i] <= 10^4`
    -   `nums` is sorted in a **strictly increasing** order.

### Solution

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

## 40. [LeetCode 190](https://leetcode.com/problems/reverse-bits/) Reverse Bits #Binary

- Reverse bits of a given 32 bits unsigned integer.
- **Note:**
    -   Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
    -   In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two%27s_complement). Therefore, in **Example 2** above, the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.
- **Example 1:**
    - **Input:** n = 00000010100101000001111010011100
    - **Output: 964176192 (00111001011110000010100101000000)
    - **Explanation:** The input binary string **00000010100101000001111010011100** represents the unsigned integer 43261596, so return 964176192 which its binary representation is **00111001011110000010100101000000**.
- **Example 2:**
    - **Input:** n = 11111111111111111111111111111101
    - **Output: 3221225471 (10111111111111111111111111111111)
    - **Explanation:** The input binary string **11111111111111111111111111111101** represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is **10111111111111111111111111111111**.
- **Constraints:**
    -   The input must be a **binary string** of length `32`
- **Follow up:** If this function is called many times, how would you optimize it?

### Solution 1: bit by bit

```java
public int reverseBits(int n) {
    int left = 0, right = 31;
    while (left < right) {
        n = swap(n, left++, right--);
    }
    return n;
}

private int swap(int x, int i, int j) {
    int bit_i = (x >> i) & 1;
    int bit_j = (x >> j) & 1;
    if (bit_i == bit_j) {
        return x;
    }
    return x ^ ((1 << i) + (1 << j));
}
```

Time Complexity: O(1)

Space Complexity: O(1)

### Solution 2: divide and conquer

```java
public int reverseBits(int num) {
	num = ((num & 0xffff0000) >>> 16) | ((num & 0x0000ffff) << 16);
	num = ((num & 0xff00ff00) >>> 8) | ((num & 0x00ff00ff) << 8);
	num = ((num & 0xf0f0f0f0) >>> 4) | ((num & 0x0f0f0f0f) << 4);
	num = ((num & 0xcccccccc) >>> 2) | ((num & 0x33333333) << 2);
	num = ((num & 0xaaaaaaaa) >>> 1) | ((num & 0x55555555) << 1);
	return num;
}
```

Time Complexity: O(1)

Space Complexity: O(1)

## 41. [LeetCode 572](https://leetcode.com/problems/subtree-of-another-tree/) Subtree of Another Tree #BinaryTree

- Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.
- A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [3,4,5,1,2], subRoot = [4,1,2]
    - **Output:** true
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg" style="zoom:67%;" />
    - **Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
    - **Output:** false
- **Constraints:**
    -   The number of nodes in the `root` tree is in the range `[1, 2000]`.
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

## 42. [LeetCode 977](https://leetcode.com/problems/squares-of-a-sorted-array/) Squares of a Sorted Array #Array

- Given an integer array `nums` sorted in **non-decreasing**order, return _an array of **the squares of each number**sorted in non-decreasing order_.
- **Example 1:**
    - **Input:** nums = [-4,-1,0,3,10]
    - **Output:** [0,1,9,16,100]
    - **Explanation:** After squaring, the array becomes [16,1,0,9,100]. After sorting, it becomes [0,1,9,16,100].
- **Example 2:**
    - **Input:** nums = [-7,-3,2,3,11]
    - **Output:** [4,9,9,49,121]
- **Constraints:**
    -   `1 <= nums.length <= 10^4`
    -   `-10^4 <= nums[i] <= 10^4`
    -   `nums` is sorted in **non-decreasing** order.
- **Follow up:** Squaring each element and sorting the new array is very trivial, could you find an `O(n)` solution using a different approach?

### Solution

```java
public int[] sortedSquares(int[] nums) {
    int[] result = new int[nums.length];
    int left = 0, right = nums.length - 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        int num;
        if (Math.abs(nums[left]) < Math.abs(nums[right])) {
            num = nums[right];
            right--;
        } else {
            num = nums[left];
            left++;
        }
        result[i] = num * num;
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

# Medium (101)

## 43. [Leetcode 57](https://leetcode.com/problems/insert-interval/) Insert Interval #Array

- You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represent the start and the end of the `ith` interval and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.
- Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).
- Return `intervals` _after the insertion_.
- **Example 1:**
    - **Input:** intervals = `[[1,3],[6,9]]`, newInterval = [2,5]
    - **Output:** `[[1,5],[6,9]]`
- **Example 2:**
    - **Input:** intervals = `[[1,2],[3,5],[6,7],[8,10],[12,16]]`, newInterval = [4,8]
    - **Output:** `[[1,2],[3,10],[12,16]]`
    - **Explanation:** Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
- **Constraints:**
    -   `0 <= intervals.length <= 10^4`
    -   `intervals[i].length == 2`
    -   `0 <= starti <= endi <= 10^5`
    -   `intervals` is sorted by `starti` in **ascending** order.
    -   `newInterval.length == 2`
    -   `0 <= start <= end <= 10^5`

### Solution

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0;
    while (i < intervals.length && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i]);
        i++;
    }
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.add(newInterval);
    while (i < intervals.length) {
        result.add(intervals[i]);
        i++;
    }
    return result.toArray(new int[result.size()][]);
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 44. [LeetCode 542](https://leetcode.com/problems/01-matrix/) 01 Matrix #Graph

- Given an `m x n` binary matrix `mat`, return _the distance of the nearest_ `0` _for each cell_.
- The distance between two adjacent cells is `1`.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/24/01-1-grid.jpg" style="zoom:67%;" />
    - **Input:** mat = `[[0,0,0],[0,1,0],[0,0,0]]`
    - **Output:** `[[0,0,0],[0,1,0],[0,0,0]]`
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/04/24/01-2-grid.jpg" style="zoom:67%;" />
    - **Input:** mat = `[[0,0,0],[0,1,0],[1,1,1]]`
    - **Output:** `[[0,0,0],[0,1,0],[1,2,1]]`
- **Constraints:**
    -   `m == mat.length`
    -   `n == mat[i].length`
    -   `1 <= m, n <= 10^4`
    -   `1 <= m * n <= 10^4`
    -   `mat[i][j]` is either `0` or `1`.
    -   There is at least one `0` in `mat`.

### Solution

```java
public int[][] updateMatrix(int[][] mat) {
    if (mat == null || mat.length == 0 || mat[0].length == 0 || (mat.length == 1 && mat[0].length == 0)) {
        return mat;
    }
    int[][] result = new int[mat.length][mat[0].length];
    int max = mat.length + mat[0].length;
    for (int i = 0; i < mat.length; i++) {
        for (int j = 0; j < mat[0].length; j++) {
            if (mat[i][j] == 0) {
                continue;
            }
            result[i][j] = max;
            if (i > 0) {
                result[i][j] = Math.min(result[i][j], result[i - 1][j] + 1);
            }
            if (j > 0) {
                result[i][j] = Math.min(result[i][j], result[i][j - 1] + 1);
            }
        }
    }
    for (int i = mat.length - 1; i >= 0; i--) {
        for (int j = mat[0].length - 1; j >= 0; j--) {
            if (mat[i][j] == 0) {
                continue;
            }
            if (i < mat.length - 1) {
                result[i][j] = Math.min(result[i][j], result[i + 1][j] + 1);
            }
            if (j < mat[0].length - 1) {
                result[i][j] = Math.min(result[i][j], result[i][j + 1] + 1);
            }
        }
    }
    return result;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 45. [LeetCode 973](https://leetcode.com/problems/k-closest-points-to-origin/) K Closest Points to Origin #Heap

- Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.
- The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).
- You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg" style="zoom: 33%;" />
    - **Input:** points = `[[1,3],[-2,2]]`, k = 1
    - **Output:** `[[-2,2]]`
    - **Explanation:**
        - The distance between (1, 3) and the origin is sqrt(10).
        - The distance between (-2, 2) and the origin is sqrt(8).
        - Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
        - We only want the closest k = 1 points from the origin, so the answer is just `[[-2,2]]`.
- **Example 2:**
    - **Input:** points = `[[3,3],[5,-1],[-2,4]]`, k = 2
    - **Output:** `[[3,3],[-2,4]]`
    - **Explanation:** The answer `[[-2,4],[3,3]]` would also be accepted.
- **Constraints:**
    -   `1 <= k <= points.length <= 10^4`
    -   `-10^4 < xi, yi < 10^4`

### Solution 1: max heap

```java
public int[][] kClosest(int[][] points, int k) {
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(b[0] * b[0] + b[1] * b[1], a[0] * a[0] + a[1] * a[1]));
    for (int[] point : points) {
        pq.offer(point);
        if (pq.size() > k) {
            pq.poll();
        }
    }
    int[][] result = new int[k][2];
    for (int i = 0; i < k; i++) {
        int[] point = pq.poll();
        result[i][0] = point[0];
        result[i][1] = point[1];
    }
    return result;
}
```

Time Complexity: O(nlogk)

Space Complexity: O(k)

### Solution 2: quickselect

```java
public int[][] kClosest(int[][] points, int k) {
    quickselect(points, 0, points.length - 1, k);
    return Arrays.copyOfRange(points, 0, k);
}

private void quickselect(int[][] points, int left, int right, int k) {
    if (left >= right) {
        return;
    }
    int pivot = left + new Random().nextInt(right - left);
    pivot = partition(left, right, pivot, points);
    if (pivot == k) {
        return;
    } else if (pivot < k) {
        quickselect(points, pivot + 1, right, k);
    } else {
        quickselect(points, left, pivot - 1, k);
    }
}

private int partition(int left, int right, int pivot, int[][] points) {
    int distance = dist(points, pivot);
    swap(points, pivot, right);
    int index = left;
    for (int i = left; i < right; i++) {
        if (dist(points, i) < distance) {
            swap(points, index, i);
            index++;
        }
    }
    swap(points, index, right);
    return index;
}

private int dist(int[][] points, int index) {
    int[] point = points[index];
    return point[0] * point[0] + point[1] * point[1];
}

private void swap(int[][] points, int x, int y) {
    int temp0 = points[x][0], temp1 = points[x][1];
    points[x][0] = points[y][0];
    points[x][1] = points[y][1];
    points[y][0] = temp0;
    points[y][1] = temp1;
}
```

Time Complexity: O(n)

Space Complexity: O(n)

## 46. [LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) Longest Substring Without Repeating Characters #String 

- Given a string `s`, find the length of the **longest substring** without repeating characters.
- **Example 1:**
    - **Input:** s = "abcabcbb"
    - **Output:** 3
    - **Explanation:** The answer is "abc", with the length of 3.
- **Example 2:**
    - **Input:** s = "bbbbb"
    - **Output:** 1
    - **Explanation:** The answer is "b", with the length of 1.
- **Example 3:**
    - **Input:** s = "pwwkew"
    - **Output:** 3
    - **Explanation:** The answer is "wke", with the length of 3.
        - Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
- **Constraints:**
    -   `0 <= s.length <= 5 * 10^4`
    -   `s` consists of English letters, digits, symbols and spaces.

### Solution: sliding window

```java
public int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0) {
        return 0;
    }
    int result = 0;
    int[] cache = new int[256];
    for (int slow = 0, fast = 0; fast < s.length(); fast++) {
        slow = cache[s.charAt(fast)] > 0 ? Math.max(slow, cache[s.charAt(fast)]) : slow;
        cache[s.charAt(fast)] = fast + 1;
        result = Math.max(result, fast - slow + 1);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 47. [LeetCode 15](https://leetcode.com/problems/3sum/) 3 Sum #Array

- Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
- Notice that the solution set must not contain duplicate triplets.
- **Example 1:**
    - **Input:** nums = [-1,0,1,2,-1,-4]
    - **Output:** `[[-1,-1,2],[-1,0,1]]`
    - **Explanation:** 
        - nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
        - nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
        - nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
        - The distinct triplets are [-1,0,1] and [-1,-1,2].
        - Notice that the order of the output and the order of the triplets does not matter.
- **Example 2:**
    - **Input:** nums = [0,1,1]
    - **Output:** []
    - **Explanation:** The only possible triplet does not sum up to 0.
- **Example 3:**
    - **Input:** nums = [0,0,0]
    - **Output:** `[[0,0,0]]`
    - **Explanation:** The only possible triplet sums up to 0.
- **Constraints:**
    -   `3 <= nums.length <= 3000`
    -   `-10^5 <= nums[i] <= 10^5`

### Solution

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    if (nums == null || nums.length < 3) {
        return result;
    }
    return allTriples(nums, 0);
}

private List<List<Integer>> allTriples(int[] array, int target) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(array);
    for (int i = 0; i < array.length - 2; i++) {
        if (i > 0 && array[i] == array[i - 1]) {
            continue;
        }
        int left = i + 1;
        int right = array.length - 1;
        while (left < right) {
            int temp = array[left] + array[right];
            if (temp + array[i] == target) {
                result.add(Arrays.asList(array[i], array[left], array[right]));
                left++;
                while (left < right && array[left] == array[left - 1]) {
                    left++;
                }
            } else if (temp + array[i] < target) {
                left++;
            } else {
                right--;
            }
        }
    }
    return result;
}
```

Time Complexity: O(n^2)

Space Complexity: O(logn)

## 48. [LeetCode ]



### Solution

```java
```

Time Complexity: O()

Space Complexity: O()





# Hard (26)