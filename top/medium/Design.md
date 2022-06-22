# Medium Collection Day 4 (Design)

https://leetcode.com/explore/interview/card/top-interview-questions-medium/112/design/

## 1. Flatten 2D Vector (medium)

[LeetCode 251](https://leetcode.com/problems/flatten-2d-vector/)

```java
import java.util.NoSuchElementException;

class Vector2D {
    private int[][] vector;
    private int row = 0;
    private int col = 0;
    
    public Vector2D(int[][] vec) {
        vector = vec;
    }
    
    public int next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        return vector[row][col++];
    }
    
    public boolean hasNext() {
        while (row < vector.length && col == vector[row].length) {
            col = 0;
            row++;
        }
        return row < vector.length;
    }
}
```

## 2. Serialize and Deserialize Binary Tree (hard)

[LeetCode 297](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

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

## 3. Insert Delete GetRandom O(1) (medium)

[LeetCode 380](https://leetcode.com/problems/insert-delete-getrandom-o1/)

```java
class RandomizedSet {
    private Map<Integer, Integer> map;
    private List<Integer> list;
    private Random rand;
    
    public RandomizedSet() {
        map = new HashMap<>();
        list = new ArrayList<>();
        rand = new Random();
    }
    
    public boolean insert(int val) {
        if (map.containsKey(val)) {
            return false;
        }
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    public boolean remove(int val) {
        if (!map.containsKey(val)) {
            return false;
        }
        int index = map.get(val);
        int last = list.get(list.size() - 1);
        list.set(index, last);
        map.put(last, index);
        list.remove(list.size() - 1);
        map.remove(val);
        return true;
    }
    
    public int getRandom() {
        return list.get(rand.nextInt(list.size()));
    }
}
```

## 4. Design Tic-Tac-Toe (medium)

[LeetCode 348](https://leetcode.com/problems/design-tic-tac-toe/)

```java
class TicTacToe {
    private int n;
    private int[] rows;
    private int[] cols;
    private int diagonal;
    private int antiDiagonal;
    
    public TicTacToe(int n) {
        this.n = n;
        rows = new int[n];
        cols = new int[n];
    }
    
    public int move(int row, int col, int player) {
        int current = player == 1 ? 1 : -1;
        rows[row] += current;
        cols[col] += current;
        if (row == col) {
            diagonal += current;
        }
        if (col == (n - row - 1)) {
            antiDiagonal += current;
        }
        if (Math.abs(rows[row]) == n || Math.abs(cols[col]) == n || Math.abs(diagonal) == n || Math.abs(antiDiagonal) == n) {
            return player;
        }
        return 0;
    }
}
```

