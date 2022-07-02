# Blind 75 Day 1 (Array)

## 1. [LeetCode 1](https://leetcode.com/problems/two-sum/) 2 Sum (easy)

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

### Solution 1: brute force

```java
public int[] twoSum(int[] nums, int target) {
    for (int i = 0; i < nums.length; i++) {
        for (int j = i + 1; j < nums.length; j++) {
            if (nums[i] + nums[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return null; // throw new IllegalArgumentException("No two sum solution");
}
```

Time Complexity: O(n^2)

Space Complexity: O(1)

### Solution 2: one pass hashmap

```java
public int[] twoSum(int[] nums, int target) {
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

## 2. [LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) Best Time to Buy and Sell Stock (easy)

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
    if (prices == null || prices.length == 0) {
        return 0;
    }
    int minPriceSoFar = Integer.MAX_VALUE;
    int maxProfit = 0;
    for (int price : prices) {
        minPriceSoFar = Math.min(minPriceSoFar, price);
        maxProfit = Math.max(maxProfit, price - minPriceSoFar);
    }
    return maxProfit;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. [LeetCode 217](https://leetcode.com/problems/contains-duplicate/) Contains Duplicate (easy)

- Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.
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

### Solution 1: sort

```java
public boolean containsDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 1; i++) {
        if (nums[i] == nums[i + 1]) {
            return true;
        }
    }
    return false;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(1)

### Solution 2: hashset

```java
public boolean containsDuplicate(int[] nums) {
    if (nums == null || nums.length == 0) {
        return false;
    }
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

## 4. [LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/) Product of Array Except Self (medium)

- Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.
- The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.
- You must write an algorithm that runs in `O(n)` time and without using the division operation.
- **Example 1:**
    - **Input:** nums = [1,2,3,4]
    - **Output:** [24,12,8,6]
- **Example 2:**
    - **Input:** nums = [-1,1,0,-3,3]
    - **Output:** [0,0,9,0,0]
- **Constraints:**
    -   `2 <= nums.length <= 10^5`
    -   `-30 <= nums[i] <= 30`
    -   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.
- **Follow up:** Can you solve the problem in `O(1)` extra space complexity? (The output array **does not** count as extra space for space complexity analysis.)

### Solution

```java
public int[] productExceptSelf(int[] nums) {
    if (nums == null || nums.length == 0) {
        throw new IllegalArgumentException("illegal input array");
    }
    int[] result = new int[nums.length];
    result[0] = 1;
    for (int i = 1; i < nums.length; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    int right = 1;
    for (int i = nums.length - 1; i >= 0; i--) {
        result[i] = result[i] * right;
        right *= nums[i];
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 5. [LeetCode 53](https://leetcode.com/problems/maximum-subarray/) Maximum Subarray (easy)

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
- **Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

### Solution: dynamic programming

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int lastMax = nums[0];
    int globalMax = nums[0];
    for (int i = 1; i < nums.length; i++) {
        lastMax = Math.max(lastMax + nums[i], nums[i]);
        // 继承遗产 or 另起炉灶
        globalMax = Math.max(globalMax, lastMax);
    }
    return globalMax;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 6. [LeetCode 152](https://leetcode.com/problems/maximum-product-subarray/) Maximum Product Subarray (medium)

- Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return _the product_.
- The test cases are generated so that the answer will fit in a **32-bit** integer.
- A **subarray** is a contiguous subsequence of the array.
- **Example 1:**
    - **Input:** nums = [2,3,-2,4]
    - **Output:** 6
    - **Explanation:** [2,3] has the largest product 6.
- **Example 2:**
    - **Input:** nums = [-2,0,-1]
    - **Output:** 0
    - **Explanation:** The result cannot be 2, because [-2,-1] is not a subarray.
- **Constraints:**
    -   `1 <= nums.length <= 2 * 10^4`
    -   `-10 <= nums[i] <= 10`
    -   The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

### Solution

```java
public int maxProduct(int[] nums) {
    if (nums == null || nums.length == 0) {
        return 0;
    }
    int lastMin = nums[0];
    int lastMax = nums[0];
    int globalMax = nums[0];
    for (int i = 1; i < nums.length; i++) {
        int temp = lastMin;
        lastMin = Math.min(Math.min(lastMin * nums[i], lastMax * nums[i]), nums[i]);
        lastMax = Math.max(Math.max(temp * nums[i], lastMax * nums[i]), nums[i]);
        globalMax = Math.max(globalMax, lastMax);
    }
    return globalMax;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 7. [LeetCode 153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) Find Minimum in Rotated Sorted Array (medium)

- Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:
    -   `[4,5,6,7,0,1,2]` if it was rotated `4` times.
    -   `[0,1,2,4,5,6,7]` if it was rotated `7` times.
- Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.
- Given the sorted rotated array `nums` of **unique** elements, return _the minimum element of this array_.
- You must write an algorithm that runs in `O(log n) time.`
- **Example 1:**
    - **Input:** nums = [3,4,5,1,2]
    - **Output:** 1
    - **Explanation:** The original array was [1,2,3,4,5] rotated 3 times.
- **Example 2:**
    - **Input:** nums = [4,5,6,7,0,1,2]
    - **Output:** 0
    - **Explanation:** The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
- **Example 3:**
    - **Input:** nums = [11,13,15,17]
    - **Output:** 11
    - **Explanation:** The original array was [11,13,15,17] and it was rotated 4 times. 
- **Constraints:**
    -   `n == nums.length`
    -   `1 <= n <= 5000`
    -   `-5000 <= nums[i] <= 5000`
    -   All the integers of `nums` are **unique**.
    -   `nums` is sorted and rotated between `1` and `n` times.

### Solution: binary search

1. while condition
     * right > left + 1
2. update left
     * left = mid
3. update right
     * right = mid

```java
public int findMin(int[] nums) {
    if (nums == null || nums.length == 0) {
        throw new IllegalArgumentException("illegal input array");
    }
    if (nums.length == 1 || nums[0] < nums[nums.length - 1]) {
        return nums[0];
    }
    int left = 0, right = nums.length - 1;
    while (right > left + 1) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < nums[right]) {
            right = mid;
        } else {
            left = mid;
        }
    }
    return Math.min(nums[left], nums[right]);
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 8. [LeetCode 33](https://leetcode.com/problems/search-in-rotated-sorted-array/) Search in Rotated Sorted Array (medium)

- There is an integer array `nums` sorted in ascending order (with **distinct** values).
- Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.
- Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.
- You must write an algorithm with `O(log n)` runtime complexity.
- **Example 1:**
    - **Input:** nums = [4,5,6,7,0,1,2], target = 0
    - **Output:** 4
- **Example 2:**
    - **Input:** nums = [4,5,6,7,0,1,2], target = 3
    - **Output:** -1
- **Example 3:**
    - **Input:** nums = [1], target = 0
    - **Output:** -1
- **Constraints:**
    -   `1 <= nums.length <= 5000`
    -   `-10^4 <= nums[i] <= 10^4`
    -   All values of `nums` are **unique**.
    -   `nums` is an ascending array that is possibly rotated.
    -   `-10^4 <= target <= 10^4`

### Solution: binary search

```java
public int search(int[] nums, int target) {
    if (nums == null || nums.length == 0) {
        return -1;
    }
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        if (nums[left] == target) {
            return left;
        }
        if (nums[right] == target) {
            return right;
        }
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] > nums[left]) {
            if (target < nums[mid] && target > nums[left]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {
            if (target > nums[mid] && target < nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    return -1;
}
```

Time Complexity: O(logn)

Space Complexity: O(1)

## 9. [LeetCode 15](https://leetcode.com/problems/3sum/) 3 Sum (medium)

- Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.
- Notice that the solution set must not contain duplicate triplets.
- **Example 1:**
    - **Input:** nums = [-1,0,1,2,-1,-4]
    - **Output:** `[[-1,-1,2],[-1,0,1]]`
- **Example 2:**
    - **Input:** nums = []
    - **Output:** []
- **Example 3:**
    - **Input:** nums = [0]
    - **Output:** []
- **Constraints:**
    -   `0 <= nums.length <= 3000`
    -   `-10^5 <= nums[i] <= 10^5`

### Solution: sort

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

Space Complexity: O(1)

## 10. [LeetCode 11](https://leetcode.com/problems/container-with-most-water/) Container With Most Water (medium)

- You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.
- Find two lines that together with the x-axis form a container, such that the container contains the most water.
- Return _the maximum amount of water a container can store_.
- **Notice** that you may not slant the container.
- **Example 1:**
    - <img src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg" style="zoom:50%;" />
    - **Input:** height = [1,8,6,2,5,4,8,3,7]
    - **Output:** 49
    - **Explanation:** The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
- **Example 2:**
    - **Input:** height = [1,1]
    - **Output:** 1
- **Constraints:**
    -   `n == height.length`
    -   `2 <= n <= 10^5`
    -   `0 <= height[i] <= 10^4`

### Solution

```java
public int maxArea(int[] height) {
    if (height == null || height.length == 0) {
        return 0;
    }
    int result = 0;
    int left = 0, right = height.length - 1;
    while (left < right) {
        result = Math.max(result, (right - left) * Math.min(height[left], height[right]));
        if (height[left] > height[right]) {
            right--;
        } else {
            left++;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)
