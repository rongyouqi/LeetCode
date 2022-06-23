# Blind 75 Day 1 (Array)

## 1. 2 Sum (easy)

[LeetCode 1](https://leetcode.com/problems/two-sum/)

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

Time Complexity: O(n<sup>2</sup>)

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

## 2. Best Time to Buy and Sell Stock (easy)

[LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

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

## 3. Contains Duplicate (easy)

[LeetCode 217](https://leetcode.com/problems/contains-duplicate/)

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

## 4. Product of Array Except Self (medium)

[LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/)

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

## 5. Maximum Subarray (easy)

[LeetCode 53](https://leetcode.com/problems/maximum-subarray/)

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

## 6. Maximum Product Subarray (medium)

[LeetCode 152](https://leetcode.com/problems/maximum-product-subarray/)

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

## 7. Find Minimum in Rotated Sorted Array (medium)

[LeetCode 153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

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

## 8. Search in Rotated Sorted Array (medium)

[LeetCode 33](https://leetcode.com/problems/search-in-rotated-sorted-array/)

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

## 9. 3 Sum (medium)

[LeetCode 15](https://leetcode.com/problems/3sum/)

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

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

## 10. Container With Most Water (medium)

[LeetCode 11](https://leetcode.com/problems/container-with-most-water/)

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
