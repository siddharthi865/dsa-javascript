# Set 10

| S.No. | Question                                                                                                                         |
| ----- | -------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Count number of BSTs with n nodes (Catalan number)](#question-1-count-number-of-bsts-with-n-nodes-catalan-number)               |
| 2.    | [Find distance between two nodes in a BST](#question-2-find-distance-between-two-nodes-in-a-bst)                                 |
| 3.    | [Convert a BST to a sorted doubly linked list](#question-3-convert-a-bst-to-a-sorted-doubly-linked-list)                         |
| 4.    | [Find the Kth smallest or Kth largest element in a BST](#question-4-find-the-kth-smallest-or-kth-largest-element-in-a-bst)       |
| 5.    | [Lowest common ancestor in a binary tree (not BST)](#question-5-lowest-common-ancestor-in-a-binary-tree-not-bst)                 |
| 6.    | [Minimum spanning tree with cycles allowed (clarification)](#question-6-minimum-spanning-tree-with-cycles-allowed-clarification) |
| 7.    | [Detect negative weight cycle in a graph](#question-7-detect-negative-weight-cycle-in-a-graph)                                   |
| 8.    | [Cheapest flights within k stops (DP/BFS)](#question-8-cheapest-flights-within-k-stops-dpbfs)                                    |
| 9.    | [Network delay time problem](#question-9-network-delay-time-problem)                                                             |
| 10.   | [Alien dictionary (derive order of characters)](#question-10-alien-dictionary-derive-order-of-characters)                        |
| 11.   | [Sliding window median](#question-11-sliding-window-median)                                                                      |
| 12.   | [Largest rectangle in histogram](#question-12-largest-rectangle-in-histogram)                                                    |
| 13.   | [Maximal rectangle in a 2D binary matrix](#question-13-maximal-rectangle-in-a-2d-binary-matrix)                                  |
| 14.   | [Count all palindromic substrings](#question-14-count-all-palindromic-substrings)                                                |
| 15.   | [Longest consecutive path in a matrix](#question-15-longest-consecutive-path-in-a-matrix)                                        |
| 16.   | [Median of sliding window](#question-16-median-of-sliding-window)                                                                |
| 17.   | [Implement trie (insert, search, startsWith)](#question-17-implement-trie-insert-search-startswith)                              |
| 18.   | [Word search problem in 2D grid](#question-18-word-search-problem-in-2d-grid)                                                    |
| 19.   | [Gas station problem (circular array)](#question-19-gas-station-problem-circular-array)                                          |
| 20.   | [Find celebrity in a party (matrix representation)](#question-20-find-celebrity-in-a-party-matrix-representation)                |

## Question 1. Count number of BSTs with n nodes (Catalan number)

# Count Number of BSTs with `n` Nodes (Catalan Number)

## Direct Answer

The number of **unique Binary Search Trees (BSTs)** that can be formed using `n` distinct keys is given by the **nth Catalan Number**:

C_n = \frac{1}{n+1}\binom{2n}{n}

Examples:

| n   | Number of BSTs |
| --- | -------------- |
| 0   | 1              |
| 1   | 1              |
| 2   | 2              |
| 3   | 5              |
| 4   | 14             |
| 5   | 42             |

---

# 1. Problem Understanding

Given `n` distinct values, count how many structurally unique BSTs can be formed.

For example, with `{1, 2, 3}`:

- Root = 1 → right subtree has 2 nodes
- Root = 2 → left subtree has 1 node, right subtree has 1 node
- Root = 3 → left subtree has 2 nodes

Total unique BSTs = **5**.

---

# 2. Recursive Insight

Suppose `i` is chosen as the root.

Then:

- Left subtree contains `i - 1` nodes.
- Right subtree contains `n - i` nodes.

If:

- Left side can form `dp[i-1]` BSTs
- Right side can form `dp[n-i]` BSTs

Then total BSTs with root `i`:

```text
dp[i-1] * dp[n-i]
```

Summing over all possible roots:

C*n = \sum*{i=1}^{n} C*{i-1} \cdot C*{n-i}

This is the Catalan recurrence.

---

# 3. Approach 1: Dynamic Programming

### Idea

Build answers from smaller values.

Base cases:

```text
dp[0] = 1
dp[1] = 1
```

For every `n`:

```text
dp[n] += dp[left] * dp[right]
```

where:

```text
left = root - 1
right = n - root
```

---

## JavaScript Code (DP)

```javascript
const countBSTs = (n) => {
  const dp = new Array(n + 1).fill(0);

  dp[0] = 1;
  dp[1] = 1;

  for (let nodes = 2; nodes <= n; nodes++) {
    for (let root = 1; root <= nodes; root++) {
      const left = root - 1;
      const right = nodes - root;

      dp[nodes] += dp[left] * dp[right];
    }
  }

  return dp[n];
};
```

### Complexity

- Time: **O(n²)**
- Space: **O(n)**

---

# 4. Approach 2: Catalan Formula

Instead of DP, directly compute:

C_n = \frac{(2n)!}{(n+1)!,n!}

To avoid factorial overflow, compute the binomial coefficient iteratively.

---

## JavaScript Code (Optimized)

```javascript
const countBSTs = (n) => {
  let catalan = 1;

  for (let i = 0; i < n; i++) {
    catalan = (catalan * (2 * n - i)) / (i + 1);
  }

  return catalan / (n + 1);
};
```

### Complexity

- Time: **O(n)**
- Space: **O(1)**

---

# Example Walkthrough (`n = 3`)

```text
Root = 1
Left = 0 nodes
Right = 2 nodes
Ways = 1 × 2 = 2

Root = 2
Left = 1 node
Right = 1 node
Ways = 1 × 1 = 1

Root = 3
Left = 2 nodes
Right = 0 nodes
Ways = 2 × 1 = 2

Total = 2 + 1 + 2 = 5
```

Answer:

```text
Count of BSTs with 3 nodes = 5
```

---

# Edge Cases

### n = 0

Empty tree is considered one valid BST.

```text
Output = 1
```

### n = 1

```text
Output = 1
```

### Large n

For large values (e.g., `n > 19`), JavaScript's `Number` may lose precision. Use `BigInt` if exact results are required.

---

# Interview Tips

- This is one of the most common applications of **Catalan Numbers**.

- Mention the recurrence:

  ```text
  C(n) = Σ C(i−1) × C(n−i)
  ```

- Explain why left and right subtree counts are multiplied (independent choices).

- State that the answer equals the **nth Catalan Number**.

- If asked for optimization, move from **O(n²) DP** to the **Catalan formula O(n)**.

### Related Interview Problems

1. Count unique BSTs
2. Generate all unique BSTs
3. Number of ways to parenthesize expressions
4. Valid parentheses combinations
5. Mountain ranges problem

All of these are classic applications of **Catalan Numbers**.

## Question 2. Find distance between two nodes in a BST

# Find Distance Between Two Nodes in a BST

## Direct Answer

The distance between two nodes in a BST is the number of edges on the shortest path between them.

A common and efficient approach is:

1. Find the **Lowest Common Ancestor (LCA)** of the two nodes.
2. Find the distance from the LCA to each node.
3. Add the two distances.

```text
Distance(u, v) =
Distance(LCA, u) + Distance(LCA, v)
```

---

# 1. Problem Understanding

Given a BST and two node values `a` and `b`, find the number of edges between them.

Example:

```text
        20
       /  \
      10   30
     / \   / \
    5  15 25 35
```

Find distance between `5` and `15`.

Path:

```text
5 → 10 → 15
```

Distance = **2 edges**

---

# 2. Key Observation (BST Property)

In a BST:

```text
Left subtree  < Root
Right subtree > Root
```

Using this property, we can find the LCA efficiently.

### LCA Rules

For nodes `a` and `b`:

- If both are smaller than current node → go left.
- If both are greater than current node → go right.
- Otherwise, current node is the LCA.

---

# 3. Approach 1 (Optimal)

## Step 1: Find LCA

Example:

```text
a = 5, b = 15

At 20:
Both < 20
Go left

At 10:
5 < 10 < 15

LCA = 10
```

---

## Step 2: Find Distance from LCA to Each Node

```text
Distance(10,5) = 1
Distance(10,15) = 1
```

Total:

```text
1 + 1 = 2
```

---

# JavaScript Solution

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

const findLCA = (root, a, b) => {
  while (root) {
    if (a < root.val && b < root.val) {
      root = root.left;
    } else if (a > root.val && b > root.val) {
      root = root.right;
    } else {
      return root;
    }
  }

  return null;
};

const findDistanceFromNode = (root, target) => {
  let distance = 0;

  while (root && root.val !== target) {
    if (target < root.val) {
      root = root.left;
    } else {
      root = root.right;
    }
    distance++;
  }

  return root ? distance : -1;
};

const distanceBetweenNodes = (root, a, b) => {
  const lca = findLCA(root, a, b);

  const d1 = findDistanceFromNode(lca, a);
  const d2 = findDistanceFromNode(lca, b);

  if (d1 === -1 || d2 === -1) return -1;

  return d1 + d2;
};
```

---

# Example Walkthrough

```text
        20
       /  \
      10   30
     / \   / \
    5  15 25 35
```

```javascript
distanceBetweenNodes(root, 5, 15);
```

### Finding LCA

```text
20 -> both smaller
10 -> split occurs

LCA = 10
```

### Distances

```text
10 → 5  = 1
10 → 15 = 1
```

Answer:

```text
2
```

---

# 4. Alternative Approach

Find:

```text
Distance(root, a)
Distance(root, b)
Distance(root, LCA)
```

Then:

```text
Distance(a,b)
=
Distance(root,a)
+
Distance(root,b)
-
2 × Distance(root,LCA)
```

This is commonly used in general trees as well.

---

## JavaScript

```javascript
const distanceFromRoot = (root, target) => {
  let dist = 0;

  while (root && root.val !== target) {
    root = target < root.val ? root.left : root.right;
    dist++;
  }

  return root ? dist : -1;
};

const distanceBetweenNodes = (root, a, b) => {
  const lca = findLCA(root, a, b);

  const da = distanceFromRoot(root, a);
  const db = distanceFromRoot(root, b);
  const dlca = distanceFromRoot(root, lca.val);

  return da + db - 2 * dlca;
};
```

---

# Complexity Analysis

### Optimal BST Approach

| Operation               | Complexity |
| ----------------------- | ---------- |
| Find LCA                | O(h)       |
| Distance to first node  | O(h)       |
| Distance to second node | O(h)       |

Total:

```text
Time: O(h)
Space: O(1)
```

Where `h` is the height of the BST.

- Balanced BST → **O(log n)**
- Skewed BST → **O(n)**

---

# Edge Cases

### Same Node

```text
a = b = 10
```

Distance:

```text
0
```

---

### One Node is Ancestor of Other

```text
10 and 15
```

Path:

```text
10 → 15
```

Distance:

```text
1
```

---

### Node Not Present

```text
a = 5
b = 100
```

Return:

```text
-1
```

(or handle according to problem requirements)

---

# Interview Tips

- Mention that BST properties allow finding the LCA in **O(h)** without storing paths.
- State the formula:

```text
Distance(u,v) =
Distance(LCA,u) + Distance(LCA,v)
```

- Clarify whether distance means:
  - **Number of edges** (most common), or
  - **Number of nodes in the path** (occasionally asked).

This is the expected optimal interview solution for finding the distance between two nodes in a BST.

## Question 3. Convert a BST to a sorted doubly linked list

## Question 4. Find the Kth smallest or Kth largest element in a BST

## Question 5. Lowest common ancestor in a binary tree (not BST)

## Question 6. Minimum spanning tree with cycles allowed (clarification)

## Question 7. Detect negative weight cycle in a graph

## Question 8. Cheapest flights within k stops (DP/BFS)

## Question 9. Network delay time problem

## Question 10. Alien dictionary (derive order of characters)

## Question 11. Sliding window median

## Question 12. Largest rectangle in histogram

## Question 13. Maximal rectangle in a 2D binary matrix

## Question 14. Count all palindromic substrings

## Question 15. Longest consecutive path in a matrix

## Question 16. Median of sliding window

## Question 17. Implement trie (insert, search, startsWith)

## Question 18. Word search problem in 2D grid

## Question 19. Gas station problem (circular array)

## Question 20. Find celebrity in a party (matrix representation)
