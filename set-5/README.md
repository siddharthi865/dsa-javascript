# Set 5

| S.No. | Question                                                                                                                            |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Serialize and deserialize a binary tree](#question-1-serialize-and-deserialize-a-binary-tree)                                      |
| 2.    | [Construct a BST from a sorted array](#question-2-construct-a-bst-from-a-sorted-array)                                              |
| 3.    | [Construct a tree from inorder and preorder traversals](#question-3-construct-a-tree-from-inorder-and-preorder-traversals)          |
| 4.    | [Check if two trees are identical](#question-4-check-if-two-trees-are-identical)                                                    |
| 5.    | [Find the maximum sum path between two leaves](#question-5-find-the-maximum-sum-path-between-two-leaves)                            |
| 6.    | [Detect strongly connected components (Kosaraju's algorithm)](#question-6-detect-strongly-connected-components-kosarajus-algorithm) |
| 7.    | [Detect articulation points in a graph](#question-7-detect-articulation-points-in-a-graph)                                          |
| 8.    | [Detect bridges in a graph](#question-8-detect-bridges-in-a-graph)                                                                  |
| 9.    | [Find all shortest paths in a graph (Floyd-Warshall)](#question-9-find-all-shortest-paths-in-a-graph-floyd-warshall)                |
| 10.   | [Word ladder problem using BFS](#question-10-word-ladder-problem-using-bfs)                                                         |
| 11.   | [Sliding window maximum/minimum problem](#question-11-sliding-window-maximumminimum-problem)                                        |
| 12.   | [Implement LRU cache](#question-12-implement-lru-cache)                                                                             |
| 13.   | [Trapping Rainwater problem](#question-13-trapping-rainwater-problem)                                                               |
| 14.   | [Median of two sorted arrays](#question-14-median-of-two-sorted-arrays)                                                             |
| 15.   | [Count number of islands in a 2D grid](#question-15-count-number-of-islands-in-a-2d-grid)                                           |
| 16.   | [Wildcard matching problem](#question-16-wildcard-matching-problem)                                                                 |
| 17.   | [Find the skyline of buildings](#question-17-find-the-skyline-of-buildings)                                                         |
| 18.   | [Minimum number of platforms required for trains](#question-18-minimum-number-of-platforms-required-for-trains)                     |
| 19.   | [Maximum flow in a network (Ford-Fulkerson)](#question-19-maximum-flow-in-a-network-ford-fulkerson)                                 |
| 20.   | [N-Queens problem](#question-20-n-queens-problem)                                                                                   |

## Question 1. Serialize and deserialize a binary tree

# Serialize and Deserialize a Binary Tree

## Concise Answer

Serialization is the process of converting a binary tree into a string (or array) so it can be stored or transmitted. Deserialization reconstructs the original binary tree from that serialized data.

A common interview solution uses **preorder traversal + null markers**.

---

# 1. Problem Understanding

Given a binary tree:

```text
       1
      / \
     2   3
        / \
       4   5
```

Serialize it into something like:

```text
"1,2,null,null,3,4,null,null,5,null,null"
```

And then deserialize the string back into the same tree structure.

### Requirements

- Preserve structure exactly.
- Handle null children.
- Serialization and deserialization should be efficient.

---

# 2. Approach 1: Preorder DFS with Null Markers (Most Common)

## Idea

Use preorder traversal:

```text
Root → Left → Right
```

Whenever a node is missing, store a special marker:

```text
null
```

### Example

Tree:

```text
      1
     / \
    2   3
```

Serialized:

```text
1,2,null,null,3,null,null
```

The null markers allow us to reconstruct the exact structure.

---

## Serialization

### Steps

1. Visit current node.
2. Store value.
3. Serialize left subtree.
4. Serialize right subtree.
5. For null nodes, store `"null"`.

---

## Deserialization

### Steps

Read values in preorder order:

```text
1,2,null,null,3,null,null
```

Process sequentially:

```text
1 → root

2 → left child

null → left of 2

null → right of 2

3 → right child of 1

...
```

Because preorder naturally describes how the tree was built, reconstruction becomes straightforward.

---

# JavaScript Code

```javascript
class TreeNode {
  constructor(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

// Serialize
const serialize = (root) => {
  const result = [];

  const dfs = (node) => {
    if (!node) {
      result.push("null");
      return;
    }

    result.push(node.val);
    dfs(node.left);
    dfs(node.right);
  };

  dfs(root);

  return result.join(",");
};

// Deserialize
const deserialize = (data) => {
  const values = data.split(",");
  let index = 0;

  const buildTree = () => {
    if (values[index] === "null") {
      index++;
      return null;
    }

    const node = new TreeNode(Number(values[index++]));

    node.left = buildTree();
    node.right = buildTree();

    return node;
  };

  return buildTree();
};
```

---

# Dry Run

Tree:

```text
       1
      / \
     2   3
```

Serialization:

```text
1,2,null,null,3,null,null
```

Deserialization:

```text
1
├── 2
│   ├── null
│   └── null
└── 3
    ├── null
    └── null
```

Original tree restored.

---

# Complexity Analysis

### Serialization

- Visit every node once.

```text
Time: O(n)
Space: O(n)
```

---

### Deserialization

- Rebuild every node once.

```text
Time: O(n)
Space: O(n)
```

where `n` = number of nodes.

---

# 3. Approach 2: Level Order (BFS)

Another valid approach uses level-order traversal.

## Serialization

Store nodes level by level.

Example:

```text
       1
      / \
     2   3
        /
       4
```

Serialized:

```text
1,2,3,null,null,4,null,null,null
```

---

## JavaScript Code

### Serialize

```javascript
const serializeBFS = (root) => {
  if (!root) return "";

  const result = [];
  const queue = [root];

  while (queue.length) {
    const node = queue.shift();

    if (!node) {
      result.push("null");
      continue;
    }

    result.push(node.val);

    queue.push(node.left);
    queue.push(node.right);
  }

  return result.join(",");
};
```

### Deserialize

```javascript
const deserializeBFS = (data) => {
  if (!data) return null;

  const values = data.split(",");
  const root = new TreeNode(Number(values[0]));

  const queue = [root];
  let index = 1;

  while (queue.length) {
    const node = queue.shift();

    if (values[index] !== "null") {
      node.left = new TreeNode(Number(values[index]));
      queue.push(node.left);
    }
    index++;

    if (values[index] !== "null") {
      node.right = new TreeNode(Number(values[index]));
      queue.push(node.right);
    }
    index++;
  }

  return root;
};
```

---

# Which Approach is Better?

### DFS (Preorder)

✅ Most commonly expected in interviews
✅ Elegant recursive solution
✅ Easy reconstruction

```text
Time: O(n)
Space: O(n)
```

---

### BFS (Level Order)

✅ Natural for level-order storage
✅ Useful when data is already level-based

```text
Time: O(n)
Space: O(n)
```

---

# Edge Cases

### Empty Tree

```javascript
serialize(null);
// "null"
```

---

### Single Node

```text
  1
```

Serialized:

```text
1,null,null
```

---

### Left-Skewed Tree

```text
    1
   /
  2
 /
3
```

Must preserve missing right children.

---

### Right-Skewed Tree

```text
1
 \
  2
   \
    3
```

Null markers are essential to preserve structure.

---

# Common Interview Pitfalls

### ❌ Not storing null nodes

Example:

```text
1,2,3
```

Cannot uniquely reconstruct the tree.

Different trees can produce the same sequence.

---

### ❌ Forgetting traversal order during reconstruction

Serialization and deserialization must use the same traversal strategy.

---

### ❌ Using `shift()` heavily in recursion

For deserialization, prefer an index pointer:

```javascript
let index = 0;
```

instead of:

```javascript
values.shift();
```

because `shift()` is O(n).

---

# Interview Tip

If asked this question in an interview, start with:

> "I'll serialize the tree using preorder traversal and include a null marker for missing children. During deserialization, I'll read the values in the same order and recursively rebuild the tree. The null markers allow me to reconstruct the exact structure. This gives O(n) time and O(n) space."

This is the standard and most interview-friendly solution.

## Question 2. Construct a BST from a sorted array

## Question 3. Construct a tree from inorder and preorder traversals

## Question 4. Check if two trees are identical

## Question 5. Find the maximum sum path between two leaves

## Question 6. Detect strongly connected components (Kosaraju's algorithm)

## Question 7. Detect articulation points in a graph

## Question 8. Detect bridges in a graph

## Question 9. Find all shortest paths in a graph (Floyd-Warshall)

## Question 10. Word ladder problem using BFS

## Question 11. Sliding window maximum/minimum problem

## Question 12. Implement LRU cache

## Question 13. Trapping Rainwater problem

## Question 14. Median of two sorted arrays

## Question 15. Count number of islands in a 2D grid

## Question 16. Wildcard matching problem

## Question 17. Find the skyline of buildings

## Question 18. Minimum number of platforms required for trains

## Question 19. Maximum flow in a network (Ford-Fulkerson)

## Question 20. N-Queens problem
