# Medium Collection Day 1 (Array and Strings)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/103/array-and-strings/

## 1. 3 Sum (medium)

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

## 2. Set Matrix Zeros (medium)

[LeetCode 73](https://leetcode.com/problems/set-matrix-zeroes/)

```java
public void setZeroes(int[][] matrix) {
    if (matrix == null || matrix.length == 0) {
        return;
    }
    int k = 0;
    while (k < matrix[0].length && matrix[0][k] != 0) {
        k++;
    }
    for (int i = 1; i < matrix.length; i++) {
        for (int j = 0; j < matrix[0].length; j++) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    for (int i = 1; i < matrix.length; i++) {
        for (int j = matrix[0].length - 1; j >= 0; j--) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                matrix[i][j] = 0;
            }
        }
    }
    if (k < matrix[0].length) {
        Arrays.fill(matrix[0], 0);
    }
    return;
}
```

Time Complexity: O(mn)

Space Complexity: O(1)

## 3. Group Anagrams (medium)

[LeetCode 49](https://leetcode.com/problems/group-anagrams/)

```java
public List<List<String>> groupAnagrams(String[] strs) {
    if (strs == null || strs.length == 0) {
        return new ArrayList<>();
    }
    Map<String, List<String>> result = new HashMap<>();
    for (String s : strs) {
        char[] array = s.toCharArray();
        Arrays.sort(array);
        String key = new String(array);
        if (!result.containsKey(key)) {
            result.put(key, new ArrayList<>());
        }
        result.get(key).add(s);
    }
    return new ArrayList<>(result.values());
}
```

Time Complexity: O(nLlogL)

Space Complexity: O(nL)

## 4. Longest Substring Without Repeating Characters (medium)

[LeetCode 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

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

## 5. Longest Palindromic Substring (medium)

[LeetCode 5](https://leetcode.com/problems/longest-palindromic-substring/)

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 2) {
        return s;
    }
    char[] array = s.toCharArray();
    int start = 0, end = 0;
    for (int i = 0; i < array.length; i++) {
        int maxLength = Math.max(expand(array, i, i), expand(array, i, i + 1));
        if (end - start < maxLength) {
            start = i - (maxLength - 1) / 2;
            end = i + maxLength / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expand(char[] array, int i, int j) {
    while (i >= 0 && j < array.length && array[i] == array[j]) {
        i--;
        j++;
    }
    return j - i - 1;
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

## 6. Increasing Triplet Subsequence (medium)

[LeetCode 334](https://leetcode.com/problems/increasing-triplet-subsequence/)

```java
public boolean increasingTriplet(int[] nums) {
    if (nums == null || nums.length < 3) {
        return false;
    }
    int numI = Integer.MAX_VALUE, numJ = Integer.MAX_VALUE;
    for (int num : nums) {
        if (num <= numI) {
            numI = num;
        } else if (num <= numJ) {
            numJ = num;
        } else {
            return true;
        }
    }
    return false;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 7. Missing Ranges (easy)

[LeetCode 163](https://leetcode.com/problems/missing-ranges/)

```java
public List<String> findMissingRanges(int[] nums, int lower, int upper) {
    List<String> result = new ArrayList<>();
    int previous = lower - 1;
    for (int i = 0; i <= nums.length; i++) {
        int current = i < nums.length ? nums[i] : upper + 1;
        if (previous + 1 <= current - 1) {
            result.add(helper(previous + 1, current - 1));
        }
        previous = current;
    }
    return result;
}

private String helper(int lower, int upper) {
    if (lower == upper) {
        return String.valueOf(lower);
    }
    return lower + "->" + upper;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 8. Count and Say (medium)

[LeetCode 38](https://leetcode.com/problems/count-and-say/)

```java
public String countAndSay(int n) {
    String s = "1";
    for (int i = 1; i < n; i++) {
        s = helper(s);
    }
    return s;
}

private String helper(String s) {
    StringBuilder sb = new StringBuilder();
    char c = s.charAt(0);
    int count = 1;
    for (int i = 1; i < s.length(); i++) {
        if (s.charAt(i) == c) {
            count++;
        } else {
            sb.append(count).append(c);
            c = s.charAt(i);
            count = 1;
        }
    }
    sb.append(count).append(c);
    return sb.toString();
}
```

Time Complexity: O(n<sup>2</sup>)

Space Complexity: O(1)

