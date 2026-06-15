# Set 15

| S.No. | Question                                                                                                                    |
| ----- | --------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Find all duplicate subtrees in a binary tree](#question-1-find-all-duplicate-subtrees-in-a-binary-tree)                    |
| 2.    | [Distance between nodes in a binary tree](#question-2-distance-between-nodes-in-a-binary-tree)                              |
| 3.    | [Construct binary tree from level order + inorder](#question-3-construct-binary-tree-from-level-order--inorder)             |
| 4.    | [Sum of all numbers formed from root to leaf paths](#question-4-sum-of-all-numbers-formed-from-root-to-leaf-paths)          |
| 5.    | [Recover BST (two nodes swapped)](#question-5-recover-bst-two-nodes-swapped)                                                |
| 6.    | [Detect negative cycle using Bellman-Ford](#question-6-detect-negative-cycle-using-bellman-ford)                            |
| 7.    | [Find all bridges in a graph](#question-7-find-all-bridges-in-a-graph)                                                      |
| 8.    | [Find articulation points in a graph](#question-8-find-articulation-points-in-a-graph)                                      |
| 9.    | [Count number of spanning trees in a graph](#question-9-count-number-of-spanning-trees-in-a-graph)                          |
| 10.   | [Minimum cost to connect ropes / cables (Greedy + Heap)](#question-10-minimum-cost-to-connect-ropes--cables-greedy--heap)   |
| 11.   | [Trapping rain water II (2D matrix)](#question-11-trapping-rain-water-ii-2d-matrix)                                         |
| 12.   | [Sliding window maximum sum subarray](#question-12-sliding-window-maximum-sum-subarray)                                     |
| 13.   | [Maximum XOR of two numbers in array](#question-13-maximum-xor-of-two-numbers-in-array)                                     |
| 14.   | [Alien dictionary (order of characters from words)](#question-14-alien-dictionary-order-of-characters-from-words)           |
| 15.   | [Word break problem](#question-15-word-break-problem)                                                                       |
| 16.   | [Maximum subarray sum after at most k negations](#question-16-maximum-subarray-sum-after-at-most-k-negations)               |
| 17.   | [Design hit counter (time-based sliding window)](#question-17-design-hit-counter-time-based-sliding-window)                 |
| 18.   | [Shortest bridge (minimum flips to connect two islands)](#question-18-shortest-bridge-minimum-flips-to-connect-two-islands) |
| 19.   | [Count submatrices with all ones](#question-19-count-submatrices-with-all-ones)                                             |
| 20.   | [Longest increasing path in a matrix](#question-20-longest-increasing-path-in-a-matrix)                                     |

## Question 1. Find all duplicate subtrees in a binary tree

# Find All Duplicate Subtrees in a Binary Tree

## Direct Answer

A duplicate subtree is a subtree that appears more than once in a binary tree with the **same structure and node values**.

The most common interview solution is to **serialize each subtree** and use a hash map to count occurrences. Whenever a serialization appears for the second time, add that subtree's root to the answer.

---

# 1. Problem Understanding

Given the root of a binary tree, return the roots of all duplicate subtrees.

### Example

```
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
```

Duplicate subtrees:

```
  4
```

and

```
  2
 /
4
```

Output: roots of these duplicate subtrees.

---

# 2. Approach 1: Serialize Each Subtree (HashMap)

### Idea

For every node:

1. Serialize left subtree.
2. Serialize right subtree.
3. Create current subtree representation:

```
value,left,right
```

Example:

```
4,#,#
```

Store serialization frequency in a hash map.

When frequency becomes exactly `2`, add the node to the result.

### Why frequency == 2?

If a subtree occurs:

- First time → count = 1
- Second time → count = 2 → add once
- Third time → count = 3 → don't add again

This avoids duplicates in the answer.

---

# 3. Algorithm

Perform postorder traversal:

```
serialize(node):
    if node is null:
        return "#"

    left = serialize(node.left)
    right = serialize(node.right)

    current = value + "," + left + "," + right

    count[current]++

    if count[current] == 2:
         result.push(node)

    return current
```

---

# 4. JavaScript Solution

```javascript
const findDuplicateSubtrees = (root) => {
  const map = new Map();
  const result = [];

  const serialize = (node) => {
    if (!node) return "#";

    const left = serialize(node.left);
    const right = serialize(node.right);

    const subtree = `${node.val},${left},${right}`;

    map.set(subtree, (map.get(subtree) || 0) + 1);

    if (map.get(subtree) === 2) {
      result.push(node);
    }

    return subtree;
  };

  serialize(root);

  return result;
};
```

---

# 5. Example Walkthrough

Tree:

```
        1
       / \
      2   3
     /   / \
    4   2   4
       /
      4
```

Generated serializations:

```
4,#,#                -> count 3
2,4,#,#,#            -> count 2
1,...
```

When:

```
4,#,#                -> count becomes 2
```

add node `4`.

When:

```
2,4,#,#,#            -> count becomes 2
```

add node `2`.

Result:

```
[4-subtree-root, 2-subtree-root]
```

---

# 6. Complexity Analysis

### Time Complexity

For each node we build a serialization string.

Worst case:

```
O(N²)
```

because concatenating long subtree strings repeatedly can be expensive.

### Space Complexity

```
O(N²)
```

for stored serializations.

---

# 7. Approach 2: Subtree ID Mapping (Optimized)

Instead of storing long strings repeatedly:

Assign a unique ID to every distinct subtree.

Represent subtree as:

```
(node.val, leftId, rightId)
```

Store this tuple in a map.

### Idea

```
(4,0,0) -> id 1
(2,1,0) -> id 2
...
```

Duplicate IDs indicate duplicate subtrees.

This avoids huge serialization strings.

---

## Optimized JavaScript Solution

```javascript
const findDuplicateSubtrees = (root) => {
  const subtreeToId = new Map();
  const count = new Map();
  const result = [];

  let id = 1;

  const dfs = (node) => {
    if (!node) return 0;

    const leftId = dfs(node.left);
    const rightId = dfs(node.right);

    const key = `${node.val},${leftId},${rightId}`;

    if (!subtreeToId.has(key)) {
      subtreeToId.set(key, id++);
    }

    const currId = subtreeToId.get(key);

    count.set(currId, (count.get(currId) || 0) + 1);

    if (count.get(currId) === 2) {
      result.push(node);
    }

    return currId;
  };

  dfs(root);

  return result;
};
```

---

# Complexity of Optimized Approach

### Time Complexity

```
O(N)
```

Each node is processed once.

### Space Complexity

```
O(N)
```

For maps and recursion stack.

---

# Edge Cases

### Empty Tree

```
root = null
```

Output:

```
[]
```

---

### Single Node

```
1
```

No duplicates.

---

### All Nodes Same

```
      1
     / \
    1   1
```

Duplicate leaf subtree:

```
1
```

---

### Skewed Tree

```
1
 \
  2
   \
    3
```

No duplicates.

---

# Interview Tips

- Clarify that **both structure and values must match**.
- Mention that the straightforward serialization solution is easy to implement but can degrade to **O(N²)**.
- Impress interviewers by discussing the **subtree ID (hash-consing)** optimization that reduces complexity to **O(N)**.
- Use **postorder traversal** because a node's representation depends on its children being processed first.

## Question 2. Distance between nodes in a binary tree

# Distance Between Two Nodes in a Binary Tree

## Direct Answer

The distance between two nodes in a binary tree is the **number of edges in the shortest path connecting them**.

The most common interview approach is:

1. Find the **Lowest Common Ancestor (LCA)** of the two nodes.
2. Find the distance from the LCA to each node.
3. Distance =

[
\text{dist}(u,v) = \text{dist}(LCA,u) + \text{dist}(LCA,v)
]

---

# 1. Problem Understanding

Given a binary tree and two node values `p` and `q`, find the number of edges between them.

### Example

```
          1
         / \
        2   3
       / \   \
      4   5   6
```

Find distance between `4` and `6`.

Path:

```
4 → 2 → 1 → 3 → 6
```

Number of edges:

```
4
```

Output:

```
4
```

---

# 2. Approach 1: LCA + Distance Search

### Idea

The shortest path between two nodes always passes through their Lowest Common Ancestor.

For example:

```
4 and 5

      2
     / \
    4   5
```

LCA = `2`

Distance:

```
2->4 = 1
2->5 = 1

Total = 2
```

---

## Step 1: Find LCA

```javascript
const findLCA = (root, p, q) => {
  if (!root) return null;

  if (root.val === p || root.val === q) {
    return root;
  }

  const left = findLCA(root.left, p, q);
  const right = findLCA(root.right, p, q);

  if (left && right) return root;

  return left || right;
};
```

---

## Step 2: Find Distance from a Node

```javascript
const findDistance = (root, target, dist) => {
  if (!root) return -1;

  if (root.val === target) return dist;

  const left = findDistance(root.left, target, dist + 1);

  if (left !== -1) return left;

  return findDistance(root.right, target, dist + 1);
};
```

---

## Step 3: Compute Final Distance

```javascript
const distanceBetweenNodes = (root, p, q) => {
  const lca = findLCA(root, p, q);

  const d1 = findDistance(lca, p, 0);
  const d2 = findDistance(lca, q, 0);

  return d1 + d2;
};
```

---

# 3. Complete JavaScript Solution

```javascript
const distanceBetweenNodes = (root, p, q) => {
  const findLCA = (node, p, q) => {
    if (!node) return null;

    if (node.val === p || node.val === q) {
      return node;
    }

    const left = findLCA(node.left, p, q);
    const right = findLCA(node.right, p, q);

    if (left && right) return node;

    return left || right;
  };

  const findDistance = (node, target, dist) => {
    if (!node) return -1;

    if (node.val === target) return dist;

    const left = findDistance(node.left, target, dist + 1);

    if (left !== -1) return left;

    return findDistance(node.right, target, dist + 1);
  };

  const lca = findLCA(root, p, q);

  const d1 = findDistance(lca, p, 0);
  const d2 = findDistance(lca, q, 0);

  return d1 + d2;
};
```

---

# 4. Example Walkthrough

Tree:

```
          1
         / \
        2   3
       / \   \
      4   5   6
```

Find distance between:

```
4 and 6
```

### Find LCA

```
LCA(4,6) = 1
```

### Distances

```
1 -> 4 = 2 edges
1 -> 6 = 2 edges
```

### Answer

```
2 + 2 = 4
```

---

# 5. Complexity Analysis

### Time Complexity

LCA search:

```
O(N)
```

Distance to first node:

```
O(N)
```

Distance to second node:

```
O(N)
```

Total:

```
O(N)
```

---

### Space Complexity

Recursion stack:

```
O(H)
```

where `H` is tree height.

- Balanced tree → `O(log N)`
- Skewed tree → `O(N)`

---

# 6. Approach 2: Store Root-to-Node Paths

Another interview-friendly solution.

### Idea

Find:

```
root → p path
root → q path
```

Then:

1. Find where paths diverge.
2. Remaining path lengths give the distance.

---

### Example

```
Path to 4:
[1,2,4]

Path to 6:
[1,3,6]
```

Common prefix:

```
[1]
```

Distance:

```
(3 - 1) + (3 - 1)
= 4
```

---

## JavaScript Solution

```javascript
const distanceBetweenNodes = (root, p, q) => {
  const getPath = (node, target, path) => {
    if (!node) return false;

    path.push(node);

    if (node.val === target) return true;

    if (getPath(node.left, target, path) || getPath(node.right, target, path)) {
      return true;
    }

    path.pop();
    return false;
  };

  const path1 = [];
  const path2 = [];

  getPath(root, p, path1);
  getPath(root, q, path2);

  let i = 0;

  while (i < path1.length && i < path2.length && path1[i] === path2[i]) {
    i++;
  }

  return path1.length - i + (path2.length - i);
};
```

---

# Edge Cases

### Same Node

```
p = q
```

Distance:

```
0
```

---

### One Node is Ancestor of Another

```
      1
     /
    2
   /
  4
```

Distance between `2` and `4`:

```
1
```

LCA = `2`

---

### Root and Leaf

```
1
 \
  2
   \
    3
```

Distance between `1` and `3`:

```
2
```

---

### Node Not Present

Depending on the problem statement:

- Return `-1`
- Throw an error
- Assume nodes always exist

Clarify this with the interviewer.

---

# Interview Tips

- State immediately: **"Distance = distance(LCA, p) + distance(LCA, q)."**
- Mention that finding the LCA first is the standard optimal approach.
- Explain why the shortest path between two nodes passes through the LCA.
- If asked for alternatives, discuss the **root-to-node path approach**.
- Clarify whether distance is measured in **edges** (most common) or **nodes**.

## Question 3. Construct binary tree from level order + inorder

## Question 4. Sum of all numbers formed from root to leaf paths

## Question 5. Recover BST (two nodes swapped)

## Question 6. Detect negative cycle using Bellman-Ford

## Question 7. Find all bridges in a graph

## Question 8. Find articulation points in a graph

## Question 9. Count number of spanning trees in a graph

## Question 10. Minimum cost to connect ropes / cables (Greedy + Heap)

## Question 11. Trapping rain water II (2D matrix)

## Question 12. Sliding window maximum sum subarray

## Question 13. Maximum XOR of two numbers in array

## Question 14. Alien dictionary (order of characters from words)

## Question 15. Word break problem

## Question 16. Maximum subarray sum after at most k negations

## Question 17. Design hit counter (time-based sliding window)

## Question 18. Shortest bridge (minimum flips to connect two islands)

## Question 19. Count submatrices with all ones

## Question 20. Longest increasing path in a matrix
