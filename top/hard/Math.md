# Hard Collection Day 8 (Math)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/123/math/

## 1. [LeetCode 179](https://leetcode.com/problems/largest-number/) Largest Number (medium)

- Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it.
- Since the result may be very large, so you need to return a string instead of an integer.
- **Example 1:**
    - **Input:** nums = [10,2]
    - **Output:** "210"
- **Example 2:**
    - **Input:** nums = [3,30,34,5,9]
    - **Output:** "9534330"
- **Constraints:**
    -   `1 <= nums.length <= 100`
    -   `0 <= nums[i] <= 10^9`

### Solution

```java
class MyComparator implements Comparator<String> {
    @Override
    public int compare(String a, String b) {
        String ab = a + b;
        String ba = b + a;
        return ba.compareTo(ab);
    }
}

public String largestNumber(int[] nums) {
    if (nums == null || nums.length == 0) {
        return "";
    }
    String[] array = new String[nums.length];
    for (int i = 0; i < nums.length; i++) {
        array[i] = String.valueOf(nums[i]);
    }
    Arrays.sort(array, new MyComparator());
    if (array[0].equals("0")) {
        return "0";
    }
    StringBuilder sb = new StringBuilder();
    for (String s : array) {
        sb.append(s);
    }
    return sb.toString();
}
```

Time Complexity: O(nlogn)

Space Complexity: O(n)

## 2. [LeetCode 149](https://leetcode.com/problems/max-points-on-a-line/) Max Points on a Line (hard)

- Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane, return _the maximum number of points that lie on the same straight line_.
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/25/plane1.jpg" style="zoom: 50%;" />
    - **Input:** points = `[[1,1],[2,2],[3,3]]`
    - **Output:** 3
- **Example 2:**
    - <img src="https://assets.leetcode.com/uploads/2021/02/25/plane2.jpg" style="zoom: 50%;" />
    - **Input:** points = `[[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]`
    - **Output:** 4
- **Constraints:**
    -   `1 <= points.length <= 300`
    -   `points[i].length == 2`
    -   `-10^4 <= xi, yi <= 10^4`
    -   All the `points` are **unique**.

### Solution

```java
public int maxPoints(int[][] points) {
    if (points == null) {
        return 0;
    }
    if (points.length <= 2) {
        return points.length;
    }
    Map<Double, Set<Integer>> map = new HashMap<>();
    int result = 0;
    for (int i = 0; i < points.length; i++) {
        map.clear();
        for (int j = i + 1; j < points.length; j++) {
            double k = slope(points[i], points[j]);
            Set<Integer> set = map.getOrDefault(k, new HashSet<>());
            set.add(i);
            set.add(j);
            map.put(k, set);
            result = Math.max(result, set.size());
        }
    }
    return result;
}

private double slope(int[] a, int[] b) {
    int x = a[0] - b[0], y = a[1] - b[1];
    return x == 0 ? Double.MIN_VALUE : (double)y / (double)x + 0.0;
}
```

Time Complexity: O(n^2)

Space Complexity: O(n)
