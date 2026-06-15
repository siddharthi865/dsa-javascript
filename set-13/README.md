# Set 13

| S.No. | Question                                                                                                                                                 |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Sum of all nodes at level k](#question-1-sum-of-all-nodes-at-level-k)                                                                                   |
| 2.    | [Maximum width of a binary tree](#question-2-maximum-width-of-a-binary-tree)                                                                             |
| 3.    | [Inorder traversal of a binary tree using O(1) space (Morris Traversal)](#question-3-inorder-traversal-of-a-binary-tree-using-o1-space-morris-traversal) |
| 4.    | [Convert BST to a Greater Sum Tree](#question-4-convert-bst-to-a-greater-sum-tree)                                                                       |
| 5.    | [Find k largest elements in a BST](#question-5-find-k-largest-elements-in-a-bst)                                                                         |
| 6.    | [Minimum height trees (find roots)](#question-6-minimum-height-trees-find-roots)                                                                         |
| 7.    | [Detect cycle using union-find](#question-7-detect-cycle-using-union-find)                                                                               |
| 8.    | [Shortest path in weighted DAG](#question-8-shortest-path-in-weighted-dag)                                                                               |
| 9.    | [Course schedule II (order of courses)](#question-9-course-schedule-ii-order-of-courses)                                                                 |
| 10.   | [Cheapest flights within k stops (BFS/Dijkstra)](#question-10-cheapest-flights-within-k-stops-bfsdijkstra)                                               |
| 11.   | [Knight's shortest path on chessboard](#question-11-knights-shortest-path-on-chessboard)                                                                 |
| 12.   | [Rat in a maze problem](#question-12-rat-in-a-maze-problem)                                                                                              |
| 13.   | [Flood fill algorithm](#question-13-flood-fill-algorithm)                                                                                                |
| 14.   | [Count number of islands with DFS/BFS](#question-14-count-number-of-islands-with-dfsbfs)                                                                 |
| 15.   | [Bipartite graph check using DFS](#question-15-bipartite-graph-check-using-dfs)                                                                          |
| 16.   | [Find element in almost sorted array](#question-16-find-element-in-almost-sorted-array)                                                                  |
| 17.   | [Count pairs with difference k](#question-17-count-pairs-with-difference-k)                                                                              |
| 18.   | [Find repeating element in array of size n + 1](#question-18-find-repeating-element-in-array-of-size-n--1)                                               |
| 19.   | [Search in 2D row-wise and column-wise sorted matrix](#question-19-search-in-2d-row-wise-and-column-wise-sorted-matrix)                                  |
| 20.   | [Find smallest range covering elements from k lists](#question-20-find-smallest-range-covering-elements-from-k-lists)                                    |

## Question 1. Sum of all nodes at level k

# Sum of All Nodes at Level `k` in a Binary Tree

## Direct Answer

Given a binary tree and a level `k`, find the sum of all node values present at that level.

The most common approach is **Level Order Traversal (BFS)**, where we traverse the tree level by level and calculate the sum when we reach level `k`.

---

# 1. Problem Understanding

### Example

```text
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

For:

```text
k = 2
```

Nodes at level 2:

```text
4, 5, 6, 7
```

Sum:

```text
4 + 5 + 6 + 7 = 22
```

Assuming:

- Root is at level `0`.
- Tree may contain negative values.
- If level `k` doesn't exist, return `0`.

---

# 2. Approach 1: Level Order Traversal (BFS)

### Idea

Traverse the tree level by level using a queue.

- Keep track of the current level.
- When the current level equals `k`, sum all nodes in that level.
- Return the sum immediately.

### Steps

1. Push root into queue.
2. Start level = 0.
3. Process nodes level by level.
4. If level == k:
   - Sum all nodes currently in queue level.

5. Return sum.

---

## JavaScript Code (BFS)

```javascript
const sumAtLevel = (root, k) => {
  if (!root) return 0;

  const queue = [root];
  let level = 0;

  while (queue.length) {
    const size = queue.length;

    if (level === k) {
      let sum = 0;

      for (let i = 0; i < size; i++) {
        sum += queue[i].val;
      }

      return sum;
    }

    for (let i = 0; i < size; i++) {
      const node = queue.shift();

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    level++;
  }

  return 0;
};
```

---

### Complexity Analysis

- **Time:** `O(N)`
- **Space:** `O(W)`

Where:

- `N` = number of nodes
- `W` = maximum width of tree

---

# 3. Approach 2: DFS (Recursive)

### Idea

During recursion:

- Pass the current level.
- When current level becomes `k`, add node value to answer.
- Continue traversal.

---

## JavaScript Code (DFS)

```javascript
const sumAtLevel = (root, k) => {
  let sum = 0;

  const dfs = (node, level) => {
    if (!node) return;

    if (level === k) {
      sum += node.val;
      return;
    }

    dfs(node.left, level + 1);
    dfs(node.right, level + 1);
  };

  dfs(root, 0);

  return sum;
};
```

---

### Complexity Analysis

- **Time:** `O(N)`
- **Space:** `O(H)`

Where:

- `H` = height of tree
- `O(log N)` for balanced tree
- `O(N)` for skewed tree

---

# 4. Optimized BFS (Avoid `shift()`)

In JavaScript, `shift()` is `O(N)`.

Use a pointer instead.

```javascript
const sumAtLevel = (root, k) => {
  if (!root) return 0;

  const queue = [root];
  let front = 0;
  let level = 0;

  while (front < queue.length) {
    const size = queue.length - front;

    if (level === k) {
      let sum = 0;

      for (let i = front; i < front + size; i++) {
        sum += queue[i].val;
      }

      return sum;
    }

    for (let i = 0; i < size; i++) {
      const node = queue[front++];

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    level++;
  }

  return 0;
};
```

---

# 5. Edge Cases

### Empty Tree

```text
root = null
```

Output:

```text
0
```

---

### Single Node Tree

```text
  10
```

```text
k = 0
```

Output:

```text
10
```

---

### Level Not Present

```text
k > height of tree
```

Output:

```text
0
```

---

### Negative Values

```text
      1
     / \
   -2   3
```

```text
k = 1
```

Output:

```text
1
```

Because:

```text
-2 + 3 = 1
```

---

# 6. Interview Tips

- Clarify whether the root is considered **level 0 or level 1**.
- BFS is usually the expected interview solution because the problem is level-based.
- Mention that using `Array.shift()` in JavaScript can degrade performance; using a queue pointer is preferred.
- DFS is a valid alternative and demonstrates recursion skills.

### Interview-Ready Summary

> Use **Level Order Traversal (BFS)** to process the tree level by level. When the traversal reaches level `k`, sum all nodes in that level and return the result. This runs in **O(N)** time and **O(W)** space, where `W` is the maximum width of the tree.

## Question 2. Maximum width of a binary tree

# Maximum Width of a Binary Tree

## Direct Answer

The **width of a binary tree** at a level is the number of positions between the leftmost and rightmost non-null nodes (including the null positions in between).

A common interview solution uses **Level Order Traversal (BFS)** and assigns each node an index as if the tree were stored in an array:

- Root → index `0`
- Left child → `2 * index + 1`
- Right child → `2 * index + 2`

For each level:

```text
width = lastIndex - firstIndex + 1
```

Track the maximum width across all levels.

---

# 1. Problem Understanding

Example:

```text
         1
       /   \
      2     3
     /       \
    4         7
```

Indices:

```text
         0
       /   \
      1     2
     /       \
    3         6
```

Width at last level:

```text
6 - 3 + 1 = 4
```

Output:

```text
4
```

Notice that null positions between nodes are counted.

---

# 2. Approach 1: BFS with Indexing (Optimal)

### Idea

Store:

```javascript
[node, index];
```

for each node in the queue.

For every level:

1. Get first node index.
2. Get last node index.
3. Compute width.
4. Update answer.
5. Push children with calculated indices.

To avoid very large numbers in deep trees, normalize indices at every level.

---

## JavaScript Code

```javascript
const widthOfBinaryTree = (root) => {
  if (!root) return 0;

  let maxWidth = 0;
  const queue = [[root, 0]];

  while (queue.length) {
    const size = queue.length;

    const firstIndex = queue[0][1];
    let lastIndex = firstIndex;

    for (let i = 0; i < size; i++) {
      const [node, index] = queue.shift();

      const normalizedIndex = index - firstIndex;
      lastIndex = normalizedIndex;

      if (node.left) {
        queue.push([node.left, 2 * normalizedIndex + 1]);
      }

      if (node.right) {
        queue.push([node.right, 2 * normalizedIndex + 2]);
      }
    }

    maxWidth = Math.max(maxWidth, lastIndex + 1);
  }

  return maxWidth;
};
```

---

# 3. Optimized BFS (No `shift()`)

Since `shift()` is `O(N)` in JavaScript, use a pointer.

```javascript
const widthOfBinaryTree = (root) => {
  if (!root) return 0;

  let maxWidth = 0;
  const queue = [[root, 0]];
  let front = 0;

  while (front < queue.length) {
    const size = queue.length - front;

    const firstIndex = queue[front][1];
    let lastIndex = 0;

    for (let i = 0; i < size; i++) {
      const [node, index] = queue[front++];

      const normalizedIndex = index - firstIndex;
      lastIndex = normalizedIndex;

      if (node.left) {
        queue.push([node.left, 2 * normalizedIndex + 1]);
      }

      if (node.right) {
        queue.push([node.right, 2 * normalizedIndex + 2]);
      }
    }

    maxWidth = Math.max(maxWidth, lastIndex + 1);
  }

  return maxWidth;
};
```

---

# 4. Approach 2: DFS with Level Tracking

### Idea

For each level:

- Store the first index encountered.
- Compute width using current index and first index.

This mimics the BFS indexing logic recursively.

---

## JavaScript Code

```javascript
const widthOfBinaryTree = (root) => {
  const firstIndexAtLevel = [];
  let maxWidth = 0;

  const dfs = (node, level, index) => {
    if (!node) return;

    if (firstIndexAtLevel[level] === undefined) {
      firstIndexAtLevel[level] = index;
    }

    const width = index - firstIndexAtLevel[level] + 1;

    maxWidth = Math.max(maxWidth, width);

    dfs(node.left, level + 1, 2 * index + 1);
    dfs(node.right, level + 1, 2 * index + 2);
  };

  dfs(root, 0, 0);

  return maxWidth;
};
```

---

# 5. Complexity Analysis

### BFS

- **Time:** `O(N)`
- **Space:** `O(N)`

### DFS

- **Time:** `O(N)`
- **Space:** `O(H)`

Where:

- `N` = number of nodes
- `H` = height of tree

---

# 6. Edge Cases

### Empty Tree

```text
root = null
```

Output:

```text
0
```

---

### Single Node

```text
  1
```

Output:

```text
1
```

---

### Skewed Tree

```text
1
 \
  2
   \
    3
```

Output:

```text
1
```

---

### Sparse Tree

```text
        1
       / \
      2   3
     /     \
    4       7
```

Output:

```text
4
```

Because positions are:

```text
4 _ _ 7
```

Width = 4.

---

# Common Pitfalls

### Incorrect Width Calculation

Many candidates do:

```javascript
width = queue.length;
```

This gives the number of nodes at a level, **not the actual width** when gaps exist.

---

### Integer Overflow

For very deep trees:

```javascript
2 * index + 1;
```

can become huge.

Normalize indices per level:

```javascript
normalizedIndex = index - firstIndex;
```

to avoid overflow.

---

# Interview Tip

If the interviewer says **"maximum width including null gaps"**, immediately think:

> "Use BFS with complete-binary-tree indexing."

That's the standard and most expected solution for this problem, achieving **O(N)** time complexity.

## Question 3. Inorder traversal of a binary tree using O(1) space (Morris Traversal)

## Question 4. Convert BST to a Greater Sum Tree

## Question 5. Find k largest elements in a BST

## Question 6. Minimum height trees (find roots)

## Question 7. Detect cycle using union-find

## Question 8. Shortest path in weighted DAG

## Question 9. Course schedule II (order of courses)

## Question 10. Cheapest flights within k stops (BFS/Dijkstra)

## Question 11. Knight's shortest path on chessboard

## Question 12. Rat in a maze problem

## Question 13. Flood fill algorithm

## Question 14. Count number of islands with DFS/BFS

## Question 15. Bipartite graph check using DFS

## Question 16. Find element in almost sorted array

## Question 17. Count pairs with difference k

## Question 18. Find repeating element in array of size n + 1

## Question 19. Search in 2D row-wise and column-wise sorted matrix

## Question 20. Find smallest range covering elements from k lists
