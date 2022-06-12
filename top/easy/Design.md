# Easy Collection Day 3 (Design)

https://leetcode.com/explore/interview/card/top-interview-questions-easy/98/design/

## 1. Shuffle an Array (medium)

[LeetCode 384](https://leetcode.com/problems/shuffle-an-array/)

```java
class Solution {
    private int[] array;
    private int[] original;
    
    Random rand = new Random();
    private int randRange(int min, int max) {
        return rand.nextInt(max - min) + min;
    }
    
    public Solution(int[] nums) {
        array = nums;
        original = nums.clone();
    }
    
    public int[] reset() {
        array = original;
        original = original.clone();
        return original;
    }
    
    public int[] shuffle() {
        for (int i = 0; i < array.length; i++) {
            swap(array, i, randRange(i, array.length));
        }
        return array;
    }
    
    private void swap(int[] array, int x, int y) {
        int temp = array[x];
        array[x] = array[y];
        array[y] = temp;
    }
}
```

## 2. Min Stack (easy)

[LeetCode 155](https://leetcode.com/problems/min-stack/)

```java
class MinStack {
    private Deque<Integer> stack;
    private Deque<Integer> minStack;
    
    public MinStack() {
        stack = new ArrayDeque<>();
        minStack = new ArrayDeque<>();
    }
    
    public void push(int val) {
        stack.offerFirst(val);
        if (minStack.isEmpty() || val <= minStack.peekFirst()) {
            minStack.offerFirst(val);
        }
    }
    
    public void pop() {
        if (stack.peekFirst().equals(minStack.peekFirst())) { // ??? Integer
            minStack.pollFirst();
        }
        stack.pollFirst();
    }
    
    public int top() {
        return stack.peekFirst();
    }
    
    public int getMin() {
        return minStack.peekFirst();
    }
}
```



```java
class MinStack {
    private Deque<Integer> stack;
    private Deque<int[]> minStack;
    
    public MinStack() {
        stack = new ArrayDeque<>();
        minStack = new ArrayDeque<>();
    }
    
    public void push(int val) {
        stack.offerFirst(val);
        if (minStack.isEmpty() || val < minStack.peekFirst()[0]) {
            minStack.offerFirst(new int[]{val, 1});
        } else if (val == minStack.peekFirst()[0]) {
            minStack.peekFirst()[1]++;
        }
    }
    
    public void pop() {
        if (stack.peekFirst() == minStack.peekFirst()[0]) { // ??? Integer == int
            minStack.peekFirst()[1]--;
        }
        if (minStack.peekFirst()[1] == 0) {
            minStack.pollFirst();
        }
        stack.pollFirst();
    }
    
    public int top() {
        return stack.peekFirst();
    }
    
    public int getMin() {
        return minStack.peekFirst()[0];
    }
}
```

