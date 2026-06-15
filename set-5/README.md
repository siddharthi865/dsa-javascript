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

# Construct a BST from a Sorted Array

## Concise Answer

To construct a **balanced Binary Search Tree (BST)** from a sorted array, repeatedly choose the **middle element as the root**, then recursively build the left and right subtrees.

This ensures the BST is height-balanced.

---

# 1. Problem Understanding

You are given a sorted array (ascending order):

```text
[-10, -3, 0, 5, 9]
```

You need to construct a BST such that:

- Inorder traversal of the BST gives the same sorted array
- The tree is as balanced as possible

Example output structure:

```text
        0
       / \
    -10   5
      \     \
      -3     9
```

---

# 2. Key Insight

A BST property:

```text
Left subtree < Root < Right subtree
```

Since the array is already sorted:

- Middle element naturally becomes the root
- Left half → left subtree
- Right half → right subtree

---

# 3. Approach 1: Recursive Divide & Conquer (Best Approach)

## Idea

1. Pick middle element → root
2. Recursively build:
   - left subtree from left half
   - right subtree from right half

This guarantees:

- Balanced tree
- Optimal height: O(log n)

---

## Steps

Given array:

```text
[-10, -3, 0, 5, 9]
```

1. mid = 0 → root = 0
2. left: [-10, -3]
3. right: [5, 9]

Repeat recursively.

---

# JavaScript Solution

```javascript
class TreeNode {
  constructor(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

const sortedArrayToBST = (nums) => {
  const build = (left, right) => {
    if (left > right) return null;

    const mid = Math.floor((left + right) / 2);

    const root = new TreeNode(nums[mid]);

    root.left = build(left, mid - 1);
    root.right = build(mid + 1, right);

    return root;
  };

  return build(0, nums.length - 1);
};
```

---

# 4. Complexity Analysis

## Time Complexity

Each element is processed once:

```text
O(n)
```

## Space Complexity

Recursive call stack (balanced tree):

```text
O(log n)
```

(ignoring output tree space)

---

# 5. Approach 2: Skewed Variant (Less Optimal)

## Idea

Instead of middle, always pick:

- first element → root

This produces:

```text
-10
   \
   -3
      \
       0
         \
          5
            \
             9
```

### Code

```javascript
const sortedArrayToBSTSkewed = (nums) => {
  const build = (index) => {
    if (index >= nums.length) return null;

    const root = new TreeNode(nums[index]);
    root.right = build(index + 1);

    return root;
  };

  return build(0);
};
```

### Complexity

- Time: O(n)
- Space: O(n)
- ❌ Not balanced

---

# 6. Why Middle Element Works Best

Choosing the middle ensures:

- Left and right subtrees have roughly equal nodes
- Minimizes height
- Produces optimal BST

Height becomes:

```text
O(log n)
```

instead of:

```text
O(n)
```

---

# 7. Edge Cases

### 1. Empty array

```javascript
sortedArrayToBST([]); // null
```

---

### 2. Single element

```text
[10]
```

Output:

```text
10
```

---

### 3. Two elements

```text
[1, 2]
```

Possible BST:

```text
  1
   \
    2
```

or balanced depending on mid choice.

---

### 4. Duplicate values (if allowed)

BST still valid but may need rule:

- duplicates go left or right consistently

---

# 8. Common Mistakes

### ❌ Using linear insertion order

Leads to skewed tree (O(n) height)

---

### ❌ Wrong midpoint calculation

Always use:

```javascript
Math.floor((left + right) / 2);
```

---

### ❌ Forgetting base case

```javascript
if (left > right) return null;
```

---

# 9. Interview Tips

A strong interview explanation:

> “Since the array is sorted, I use divide and conquer. I pick the middle element as the root to ensure balance, then recursively build left and right subtrees from the two halves. This ensures O(n) time and O(log n) height.”

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
