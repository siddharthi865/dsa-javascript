# Set 3

| S.No. | Question                                                                                                                                  |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Count the number of nodes in a binary tree](#question-1-count-the-number-of-nodes-in-a-binary-tree)                                      |
| 2.    | [Find the diameter of a binary tree](#question-2-find-the-diameter-of-a-binary-tree)                                                      |
| 3.    | [Check if a tree is balanced](#question-3-check-if-a-tree-is-balanced)                                                                    |
| 4.    | [Lowest common ancestor in a BST](#question-4-lowest-common-ancestor-in-a-bst)                                                            |
| 5.    | [Perform Level-order traversal of a binary tree (BFS)](#question-5-perform-level-order-traversal-of-a-binary-tree-bfs)                    |
| 6.    | [Represent a graph using adjacency list and adjacency matrix](#question-6-represent-a-graph-using-adjacency-list-and-adjacency-matrix)    |
| 7.    | [Depth-First Search (DFS) in a graph](#question-7-depth-first-search-dfs-in-a-graph)                                                      |
| 8.    | [Breadth-First Search (BFS) in a graph](#question-8-breadth-first-search-bfs-in-a-graph)                                                  |
| 9.    | [Detect a cycle in a directed graph](#question-9-detect-a-cycle-in-a-directed-graph)                                                      |
| 10.   | [Detect a cycle in an undirected graph](#question-10-detect-a-cycle-in-an-undirected-graph)                                               |
| 11.   | [Perform Topological Sorting of a Directed Acyclic Graph (DAG)](#question-11-perform-topological-sorting-of-a-directed-acyclic-graph-dag) |
| 12.   | [Dijkstra's shortest path algorithm](#question-12-dijkstras-shortest-path-algorithm)                                                      |
| 13.   | [Bellman-Ford algorithm](#question-13-bellman-ford-algorithm)                                                                             |
| 14.   | [Prim's algorithm for minimum spanning tree](#question-14-prims-algorithm-for-minimum-spanning-tree)                                      |
| 15.   | [Kruskal's algorithm for minimum spanning tree](#question-15-kruskals-algorithm-for-minimum-spanning-tree)                                |
| 16.   | [Implement linear search](#question-16-implement-linear-search)                                                                           |
| 17.   | [Implement binary search](#question-17-implement-binary-search)                                                                           |
| 18.   | [Binary search on a rotated sorted array](#question-18-binary-search-on-a-rotated-sorted-array)                                           |
| 19.   | [Implement bubble sort](#question-19-implement-bubble-sort)                                                                               |
| 20.   | [Implement insertion sort](#question-20-implement-insertion-sort)                                                                         |

## Question 1. Count the number of nodes in a binary tree

# Count the Number of Nodes in a Binary Tree

## Direct Answer

To count the number of nodes in a binary tree, traverse every node once and keep a count.

The recursive formula is:

[
\text{count(root)} = 1 + \text{count(root.left)} + \text{count(root.right)}
]

If the node is `null`, return `0`.

---

# 1. Problem Understanding

Given the root of a binary tree, return the total number of nodes present in the tree.

### Example

```text
        1
       / \
      2   3
     / \
    4   5
```

Total nodes = **5**

---

# 2. Approach 1: Recursive DFS (Most Common Interview Solution)

### Idea

Every node contributes:

- 1 for itself
- count of nodes in left subtree
- count of nodes in right subtree

### Algorithm

1. If root is `null`, return 0.
2. Recursively count nodes in left subtree.
3. Recursively count nodes in right subtree.
4. Return `1 + leftCount + rightCount`.

### JavaScript Code

```javascript
const countNodes = (root) => {
  if (!root) return 0;

  return 1 + countNodes(root.left) + countNodes(root.right);
};
```

### Complexity Analysis

- **Time:** O(n)
  - Every node is visited once.

- **Space:** O(h)
  - Recursive call stack.
  - `h` = height of tree.

Worst case (skewed tree):

```text
O(n)
```

Balanced tree:

```text
O(log n)
```

---

# 3. Approach 2: Iterative DFS Using Stack

### Idea

Avoid recursion by using an explicit stack.

### Algorithm

1. Push root into stack.
2. While stack is not empty:
   - Pop a node.
   - Increment count.
   - Push left child.
   - Push right child.

3. Return count.

### JavaScript Code

```javascript
const countNodes = (root) => {
  if (!root) return 0;

  let count = 0;
  const stack = [root];

  while (stack.length) {
    const node = stack.pop();
    count++;

    if (node.left) stack.push(node.left);
    if (node.right) stack.push(node.right);
  }

  return count;
};
```

### Complexity

- **Time:** O(n)
- **Space:** O(h) average, O(n) worst case

---

# 4. Approach 3: Iterative BFS (Level Order Traversal)

### Idea

Traverse level by level and count nodes.

### JavaScript Code

```javascript
const countNodes = (root) => {
  if (!root) return 0;

  let count = 0;
  const queue = [root];

  while (queue.length) {
    const node = queue.shift();
    count++;

    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }

  return count;
};
```

### Complexity

- **Time:** O(n)
- **Space:** O(w)

Where `w` is the maximum width of the tree.

---

# 5. Optimized Approach for Complete Binary Trees

Interviewers sometimes specifically ask:

> "Count nodes in a Complete Binary Tree."

A complete binary tree allows a faster solution than O(n).

### Idea

For a subtree:

- Compute left height.
- Compute right height.
- If both heights are equal:
  - Tree is perfect.
  - Number of nodes:

2^h-1

- Otherwise recursively count left and right subtrees.

### JavaScript Code

```javascript
const leftHeight = (node) => {
  let height = 0;

  while (node) {
    height++;
    node = node.left;
  }

  return height;
};

const rightHeight = (node) => {
  let height = 0;

  while (node) {
    height++;
    node = node.right;
  }

  return height;
};

const countNodes = (root) => {
  if (!root) return 0;

  const lh = leftHeight(root);
  const rh = rightHeight(root);

  if (lh === rh) {
    return Math.pow(2, lh) - 1;
  }

  return 1 + countNodes(root.left) + countNodes(root.right);
};
```

### Complexity

- **Time:** O(log² n)
- **Space:** O(log n)

This is the expected optimized solution for the classic complete binary tree problem.

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

### Single Node

```text
  1
```

Output:

```text
1
```

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
3
```

---

# 7. Interview Tips

- For a **general binary tree**, the standard answer is recursive DFS with **O(n)** time.
- Mention iterative DFS/BFS if asked to avoid recursion.
- If the interviewer says **"complete binary tree"**, discuss the **O(log² n)** optimization.
- Always clarify whether the tree is:
  - General Binary Tree
  - Complete Binary Tree
  - Perfect Binary Tree

This distinction often determines whether the interviewer expects the optimized solution.

## Question 2. Find the diameter of a binary tree

## Question 3. Check if a tree is balanced

## Question 4. Lowest common ancestor in a BST

## Question 5. Perform Level-order traversal of a binary tree (BFS)

## Question 6. Represent a graph using adjacency list and adjacency matrix

## Question 7. Depth-First Search (DFS) in a graph

## Question 8. Breadth-First Search (BFS) in a graph

## Question 9. Detect a cycle in a directed graph

## Question 10. Detect a cycle in an undirected graph

## Question 11. Perform Topological Sorting of a Directed Acyclic Graph (DAG)

## Question 12. Dijkstra's shortest path algorithm

## Question 13. Bellman-Ford algorithm

## Question 14. Prim's algorithm for minimum spanning tree

## Question 15. Kruskal's algorithm for minimum spanning tree

## Question 16. Implement linear search

## Question 17. Implement binary search

## Question 18. Binary search on a rotated sorted array

## Question 19. Implement bubble sort

## Question 20. Implement insertion sort
