# Set 8

| S.No. | Question                                                                                                                                      |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Print the boundary of a binary tree](#question-1-print-the-boundary-of-a-binary-tree)                                                        |
| 2.    | [Vertical order traversal of a binary tree](#question-2-vertical-order-traversal-of-a-binary-tree)                                            |
| 3.    | [Top view of a binary tree](#question-3-top-view-of-a-binary-tree)                                                                            |
| 4.    | [Bottom view of a binary tree](#question-4-bottom-view-of-a-binary-tree)                                                                      |
| 5.    | [Convert a binary tree to a doubly linked list](#question-5-convert-a-binary-tree-to-a-doubly-linked-list)                                    |
| 6.    | [Check if a graph is bipartite](#question-6-check-if-a-graph-is-bipartite)                                                                    |
| 7.    | [Find the number of connected components](#question-7-find-the-number-of-connected-components)                                                |
| 8.    | [Shortest path in unweighted graph (BFS)](#question-8-shortest-path-in-unweighted-graph-bfs)                                                  |
| 9.    | [Minimum number of edges to add to make graph connected](#question-9-minimum-number-of-edges-to-add-to-make-graph-connected)                  |
| 10.   | [Count all paths between two vertices](#question-10-count-all-paths-between-two-vertices)                                                     |
| 11.   | [Hamiltonian path problem](#question-11-hamiltonian-path-problem)                                                                             |
| 12.   | [Graph coloring problem](#question-12-graph-coloring-problem)                                                                                 |
| 13.   | [Word ladder II (all shortest paths)](#question-13-word-ladder-ii-all-shortest-paths)                                                         |
| 14.   | [Clone a graph](#question-14-clone-a-graph)                                                                                                   |
| 15.   | [Course scheduling problem (detect cycle in prerequisites)](#question-15-course-scheduling-problem-detect-cycle-in-prerequisites)             |
| 16.   | [Search in a 2D matrix](#question-16-search-in-a-2d-matrix)                                                                                   |
| 17.   | [Find the first and last position of an element in sorted array](#question-17-find-the-first-and-last-position-of-an-element-in-sorted-array) |
| 18.   | [Find the smallest missing positive integer](#question-18-find-the-smallest-missing-positive-integer)                                         |
| 19.   | [Count occurrences of a number in sorted array](#question-19-count-occurrences-of-a-number-in-sorted-array)                                   |
| 20.   | [Find the maximum sum of i\*arr[i] after rotations](#question-20-find-the-maximum-sum-of-iarri-after-rotations)                               |

## Question 1. Print the boundary of a binary tree

# Print the Boundary of a Binary Tree

## Concise Answer

The **boundary traversal** of a binary tree prints nodes in this order:

1. Root node
2. Left boundary (excluding leaf nodes)
3. All leaf nodes (left to right)
4. Right boundary (excluding leaf nodes, printed in reverse order)

A common interview solution uses **three separate traversals** for left boundary, leaves, and right boundary.

---

# 1. Problem Understanding

Given a binary tree, print all nodes lying on its boundary in an anti-clockwise direction.

### Example

```
        20
       /  \
      8    22
     / \     \
    4  12     25
      /  \
     10  14
```

Boundary Traversal:

```
20 8 4 10 14 25 22
```

Explanation:

- Root → 20
- Left boundary → 8
- Leaves → 4, 10, 14, 25
- Right boundary (bottom-up) → 22

---

# 2. Approach

The boundary consists of three parts:

### A. Left Boundary

Traverse from root's left child:

- Prefer left child
- Otherwise go right
- Exclude leaf nodes

### B. Leaf Nodes

Perform DFS and collect all leaf nodes from left to right.

### C. Right Boundary

Traverse from root's right child:

- Prefer right child
- Otherwise go left
- Exclude leaf nodes
- Store nodes and add them in reverse order

---

# 3. Algorithm

1. Add root.
2. Traverse left boundary.
3. Traverse all leaves.
4. Traverse right boundary and reverse it.
5. Return the result.

---

# 4. JavaScript Solution

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

const boundaryTraversal = (root) => {
  if (!root) return [];

  const result = [];

  const isLeaf = (node) => node && !node.left && !node.right;

  // Add root
  if (!isLeaf(root)) {
    result.push(root.val);
  }

  // Left Boundary
  let curr = root.left;

  while (curr) {
    if (!isLeaf(curr)) {
      result.push(curr.val);
    }

    curr = curr.left ? curr.left : curr.right;
  }

  // Leaf Nodes
  const addLeaves = (node) => {
    if (!node) return;

    if (isLeaf(node)) {
      result.push(node.val);
      return;
    }

    addLeaves(node.left);
    addLeaves(node.right);
  };

  addLeaves(root);

  // Right Boundary
  curr = root.right;
  const rightBoundary = [];

  while (curr) {
    if (!isLeaf(curr)) {
      rightBoundary.push(curr.val);
    }

    curr = curr.right ? curr.right : curr.left;
  }

  for (let i = rightBoundary.length - 1; i >= 0; i--) {
    result.push(rightBoundary[i]);
  }

  return result;
};
```

---

# Example Walkthrough

```javascript
const root = new TreeNode(20);

root.left = new TreeNode(8);
root.right = new TreeNode(22);

root.left.left = new TreeNode(4);
root.left.right = new TreeNode(12);

root.left.right.left = new TreeNode(10);
root.left.right.right = new TreeNode(14);

root.right.right = new TreeNode(25);

console.log(boundaryTraversal(root));
```

Output:

```javascript
[20, 8, 4, 10, 14, 25, 22];
```

---

# 5. Complexity Analysis

### Time Complexity

- Left boundary: `O(H)`
- Right boundary: `O(H)`
- Leaf traversal: `O(N)`

Overall:

[
O(N)
]

### Space Complexity

- Recursion stack during leaf traversal: `O(H)`
- Right boundary storage: `O(H)`

Overall:

[
O(H)
]

where `H` is the height of the tree.

---

# 6. Edge Cases

### Empty Tree

```text
null
```

Output:

```text
[]
```

---

### Single Node

```text
1
```

Output:

```text
[1]
```

The root is also a leaf.

---

### Left Skewed Tree

```text
    1
   /
  2
 /
3
```

Output:

```text
1 2 3
```

---

### Right Skewed Tree

```text
1
 \
  2
   \
    3
```

Output:

```text
1 3 2
```

---

# Common Pitfalls

### 1. Printing leaf nodes twice

If leaf nodes are included in left/right boundary traversal and again during leaf traversal, duplicates occur.

Always exclude leaves while processing boundaries.

---

### 2. Printing right boundary in top-down order

Wrong:

```text
1 2 3
```

Correct:

```text
3 2 1
```

Right boundary must be added in reverse.

---

### 3. Forgetting the single-node case

If the root is a leaf, return only the root once.

---

# Interview Tips

When asked this question, clearly state:

> "Boundary traversal can be divided into three independent parts: left boundary, leaf nodes, and right boundary. We exclude leaves from the boundary traversals to avoid duplicates and print the right boundary in reverse order."

This demonstrates a clean and interview-preferred `O(N)` solution.

## Question 2. Vertical order traversal of a binary tree

# Vertical Order Traversal of a Binary Tree

## Concise Answer

In **Vertical Order Traversal**, nodes are grouped according to their **horizontal distance (HD)** from the root:

- Root has HD = 0
- Left child → HD - 1
- Right child → HD + 1

We typically use **BFS (Level Order Traversal)** along with a hash map to store nodes for each horizontal distance and then print columns from leftmost HD to rightmost HD.

---

# 1. Problem Understanding

Given a binary tree, print nodes column by column from left to right.

### Example

```text
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

Horizontal Distances:

```text
HD = -2 : 4
HD = -1 : 2
HD =  0 : 1, 5, 6
HD = +1 : 3
HD = +2 : 7
```

Output:

```javascript
[[4], [2], [1, 5, 6], [3], [7]];
```

---

# 2. Key Observation

Every node belongs to a vertical line determined by its HD.

```text
root      => HD = 0
left      => HD - 1
right     => HD + 1
```

We need:

1. Compute HD for every node.
2. Group nodes by HD.
3. Print groups from smallest HD to largest HD.

---

# 3. Approach 1 (BFS + HashMap) — Preferred

### Idea

Perform level-order traversal while tracking each node's HD.

Use:

- Queue → `(node, hd)`
- Map → `hd => nodes`
- Track minimum and maximum HD encountered

---

### Algorithm

1. Start with `(root, 0)`.
2. Pop a node.
3. Store node value in `map[hd]`.
4. Push left child with `hd - 1`.
5. Push right child with `hd + 1`.
6. After traversal, iterate from `minHD` to `maxHD`.

---

# 4. JavaScript Solution (BFS)

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

const verticalOrderTraversal = (root) => {
  if (!root) return [];

  const map = new Map();
  const queue = [[root, 0]];

  let minHD = 0;
  let maxHD = 0;

  while (queue.length) {
    const [node, hd] = queue.shift();

    if (!map.has(hd)) {
      map.set(hd, []);
    }

    map.get(hd).push(node.val);

    minHD = Math.min(minHD, hd);
    maxHD = Math.max(maxHD, hd);

    if (node.left) {
      queue.push([node.left, hd - 1]);
    }

    if (node.right) {
      queue.push([node.right, hd + 1]);
    }
  }

  const result = [];

  for (let hd = minHD; hd <= maxHD; hd++) {
    result.push(map.get(hd));
  }

  return result;
};
```

---

# Example Walkthrough

```text
        1
      /   \
     2     3
    / \   / \
   4   5 6   7
```

Queue processing:

```text
(1,0)
(2,-1) (3,+1)
(4,-2) (5,0) (6,0) (7,+2)
```

Map:

```javascript
{
  -2: [4],
  -1: [2],
   0: [1,5,6],
   1: [3],
   2: [7]
}
```

Result:

```javascript
[[4], [2], [1, 5, 6], [3], [7]];
```

---

# 5. Approach 2 (DFS + Sorting)

### Idea

Store:

```javascript
(hd, level, value);
```

for each node during DFS.

After traversal:

1. Sort by HD.
2. Group nodes with the same HD.

---

### JavaScript

```javascript
const verticalOrderDFS = (root) => {
  const nodes = [];

  const dfs = (node, hd, level) => {
    if (!node) return;

    nodes.push([hd, level, node.val]);

    dfs(node.left, hd - 1, level + 1);
    dfs(node.right, hd + 1, level + 1);
  };

  dfs(root, 0, 0);

  nodes.sort((a, b) => {
    if (a[0] !== b[0]) return a[0] - b[0];
    return a[1] - b[1];
  });

  const map = new Map();

  for (const [hd, level, value] of nodes) {
    if (!map.has(hd)) {
      map.set(hd, []);
    }

    map.get(hd).push(value);
  }

  return [...map.values()];
};
```

---

# 6. Complexity Analysis

## BFS Approach

### Time Complexity

Each node visited once:

[
O(N)
]

### Space Complexity

Queue + Map:

[
O(N)
]

---

## DFS + Sorting Approach

### Time Complexity

DFS:

[
O(N)
]

Sorting:

[
O(N \log N)
]

Total:

[
O(N \log N)
]

### Space Complexity

[
O(N)
]

---

# 7. Edge Cases

### Empty Tree

```text
null
```

Output:

```javascript
[];
```

---

### Single Node

```text
1
```

Output:

```javascript
[[1]];
```

---

### Left Skewed Tree

```text
    1
   /
  2
 /
3
```

Output:

```javascript
[[3], [2], [1]];
```

---

### Right Skewed Tree

```text
1
 \
  2
   \
    3
```

Output:

```javascript
[[1], [2], [3]];
```

---

# Common Pitfalls

### 1. Using DFS without level information

Nodes in the same vertical line may appear in the wrong order.

BFS naturally preserves top-to-bottom ordering.

---

### 2. Forgetting to track min/max HD

You'll need sorting of keys later, increasing complexity.

---

### 3. Using `queue.shift()`

In JavaScript, `shift()` is `O(N)`.

For production-quality code, use a pointer:

```javascript
let front = 0;

while (front < queue.length) {
  const [node, hd] = queue[front++];
}
```

This keeps BFS truly `O(N)`.

---

# Interview Tips

If asked in an interview, start with:

> "I’ll assign each node a horizontal distance from the root. Using BFS, I can visit nodes level by level while grouping them by horizontal distance in a map. Finally, I output the columns from the minimum HD to the maximum HD. This gives an O(N) solution."

This is the most commonly expected and interview-friendly approach.

## Question 3. Top view of a binary tree

## Question 4. Bottom view of a binary tree

## Question 5. Convert a binary tree to a doubly linked list

## Question 6. Check if a graph is bipartite

## Question 7. Find the number of connected components

## Question 8. Shortest path in unweighted graph (BFS)

## Question 9. Minimum number of edges to add to make graph connected

## Question 10. Count all paths between two vertices

## Question 11. Hamiltonian path problem

## Question 12. Graph coloring problem

## Question 13. Word ladder II (all shortest paths)

## Question 14. Clone a graph

## Question 15. Course scheduling problem (detect cycle in prerequisites)

## Question 16. Search in a 2D matrix

## Question 17. Find the first and last position of an element in sorted array

## Question 18. Find the smallest missing positive integer

## Question 19. Count occurrences of a number in sorted array

## Question 20. Find the maximum sum of i\*arr[i] after rotations
