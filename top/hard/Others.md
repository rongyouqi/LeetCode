# Hard Collection Day 8 (Others)

https://leetcode.com/explore/interview/card/top-interview-questions-hard/124/others/

## 1. [LeetCode 406](https://leetcode.com/problems/queue-reconstruction-by-height/) Queue Reconstruction by Height (medium)

- You are given an array of people, `people`, which are the attributes of some people in a queue (not necessarily in order). Each `people[i] = [hi, ki]` represents the `ith` person of height `hi` with **exactly** `ki` other people in front who have a height greater than or equal to `hi`.
- Reconstruct and return _the queue that is represented by the input array_ `people`. The returned queue should be formatted as an array `queue`, where `queue[j] = [hj, kj]` is the attributes of the `jth` person in the queue (`queue[0]` is the person at the front of the queue).
- **Example 1:**
    - **Input:** people = `[[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]`
    - **Output:** `[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]`
    - **Explanation:**
        - Person 0 has height 5 with no other people taller or the same height in front.
        - Person 1 has height 7 with no other people taller or the same height in front.
        - Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
        - Person 3 has height 6 with one person taller or the same height in front, which is person 1.
        - Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
        - Person 5 has height 7 with one person taller or the same height in front, which is person 1.
        - Hence `[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]` is the reconstructed queue.
- **Example 2:**
    - **Input:** people = `[[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]`
    - **Output:** `[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]`
- **Constraints:**
    -   `1 <= people.length <= 2000`
    -   `0 <= hi <= 10^6`
    -   `0 <= ki < people.length`
    -   It is guaranteed that the queue can be reconstructed.

### Solution

```java
public int[][] reconstructQueue(int[][] people) {
    if (people == null || people.length <= 1) {
        return people;
    }
    Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
    List<int[]> result = new ArrayList<>();
    for (int[] p : people) {
        result.add(p[1], p);
    }
    return result.toArray(new int[people.length][2]);
}
```

Time Complexity: O(n^2)

Space Complexity: O(n)

## 2. [LeetCode 42](https://leetcode.com/problems/trapping-rain-water/) Trapping Rain Water (hard)

- Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)
    - **Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
    - **Output:** 6
    - **Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
- **Example 2:**
    - **Input:** height = [4,2,0,3,2,5]
    - **Output:** 9
- **Constraints:**
    -   `n == height.length`
    -   `1 <= n <= 2 * 10^4`
    -   `0 <= height[i] <= 10^5`

### Solution

```java
public int trap(int[] height) {
    if (height == null || height.length <= 2) {
        return 0;
    }
    int left = 0, right = height.length - 1;
    int leftMax = height[0], rightMax = height[right];
    int result = 0;
    while (left < right) {
        if (height[left] <= height[right]) {
            result += Math.max(0, leftMax - height[left]);
            leftMax = Math.max(leftMax, height[left]);
            left++;
        } else {
            result += Math.max(0, rightMax - height[right]);
            rightMax = Math.max(rightMax, height[right]);
            right--;
        }
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(1)

## 3. [LeetCode 218](https://leetcode.com/problems/the-skyline-problem/) The Skyline Problem (hard)

- A city's **skyline** is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Given the locations and heights of all the buildings, return _the **skyline** formed by these buildings collectively_.
- The geometric information of each building is given in the array `buildings` where `buildings[i] = [left_i, right_i, height_i]`:
    -   `left_i` is the x coordinate of the left edge of the `ith` building.
    -   `right_i` is the x coordinate of the right edge of the `ith` building.
    -   `height_i` is the height of the `ith` building.
- You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height `0`.
- The **skyline** should be represented as a list of "key points" **sorted by their x-coordinate** in the form `[[x1,y1],[x2,y2],...]`. Each key point is the left endpoint of some horizontal segment in the skyline except the last point in the list, which always has a y-coordinate `0` and is used to mark the skyline's termination where the rightmost building ends. Any ground between the leftmost and rightmost buildings should be part of the skyline's contour.
- **Note:** There must be no consecutive horizontal lines of equal height in the output skyline. For instance, `[...,[2 3],[4 5],[7 5],[11 5],[12 7],...]` is not acceptable; the three lines of height 5 should be merged into one in the final output as such: `[...,[2 3],[4 5],[12 7],...]`
- **Example 1:**
    - <img src="https://assets.leetcode.com/uploads/2020/12/01/merged.jpg" style="zoom: 33%;" />
    - **Input:** buildings = `[[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]`
    - **Output:** `[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]`
    - **Explanation:**
        - Figure A shows the buildings of the input.
        - Figure B shows the skyline formed by those buildings. The red points in figure B represent the key points in the output list.
- **Example 2:**
    - **Input:** buildings = `[[0,2,3],[2,5,3]]`
    - **Output:** `[[0,3],[5,0]]`
- **Constraints:**
    -   `1 <= buildings.length <= 10^4`
    -   `0 <= left_i < right_i <= 2^31 - 1`
    -   `1 <= height_i <= 2^31 - 1`
    -   `buildings` is sorted by `left_i` in non-decreasing order.

### Solution

```java
public List<List<Integer>> getSkyline(int[][] buildings) {
    List<List<Integer>> result = new ArrayList<>();
    if (buildings == null || buildings.length == 0) {
        return result;
    }
    List<int[]> points = new ArrayList<>();
    for (int[] building : buildings) {
        points.add(new int[]{building[0], -building[2]});
        points.add(new int[]{building[1], building[2]});
    }
    Collections.sort(points, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
    maxHeap.offer(0);
    int previous = 0;
    for (int[] p : points) {
        if (p[1] < 0) {
            maxHeap.offer(-p[1]);
        } else {
            maxHeap.remove(p[1]);
        }
        int current = maxHeap.peek();
        if (current != previous) {
            List<Integer> list = new ArrayList<>();
            list.add(p[0]);
            list.add(current);
            result.add(list);
            previous = current;
        }
    }
    return result;
}
```

Time Complexity: O(nlogn)

Space Complexity: O(n)

## 4. [LeetCode 84](https://leetcode.com/problems/largest-rectangle-in-histogram/) Largest Rectangle in Histogram (hard)

- Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.
- **Example 1:**
    - ![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)
    - **Input:** heights = [2,1,5,6,2,3]
    - **Output:** 10
    - **Explanation:** The above is a histogram where width of each bar is 1.
        - The largest rectangle is shown in the red area, which has an area = 10 units.
- **Example 2:**
    - ![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)
    - **Input:** heights = [2,4]
    - **Output:** 4
- **Constraints:**
    -   `1 <= heights.length <= 10^5`
    -   `0 <= heights[i] <= 10^4`

### Solution

```java
public int largestRectangleArea(int[] heights) {
    if (heights == null || heights.length == 0) {
        return 0;
    }
    int result = 0;
    Deque<Integer> stack = new ArrayDeque<>();
    for (int i = 0; i <= heights.length; i++) {
        int current = i == heights.length ? 0 : heights[i];
        while (!stack.isEmpty() && heights[stack.peekFirst()] >= current) {
            int height = heights[stack.pollFirst()];
            int left = stack.isEmpty() ? 0 : stack.peekFirst() + 1;
            result = Math.max(result, height * (i - left));
        }
        stack.offerFirst(i);
    }
    return result;
}
```

Time Complexity: O(n)

Space Complexity: O(n)



