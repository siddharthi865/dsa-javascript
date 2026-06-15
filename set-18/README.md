# Set 18

| S.No. | Question                                                                                                                                    |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Find bottom-left value in a binary tree](#question-1-find-bottom-left-value-in-a-binary-tree)                                              |
| 2.    | [Construct binary tree from inorder and postorder](#question-2-construct-binary-tree-from-inorder-and-postorder)                            |
| 3.    | [Maximum sum path in a binary tree](#question-3-maximum-sum-path-in-a-binary-tree)                                                          |
| 4.    | [Sum of nodes at odd levels minus even levels](#question-4-sum-of-nodes-at-odd-levels-minus-even-levels)                                    |
| 5.    | [Count all nodes in a complete binary tree efficiently](#question-5-count-all-nodes-in-a-complete-binary-tree-efficiently)                  |
| 6.    | [Check if binary tree is complete](#question-6-check-if-binary-tree-is-complete)                                                            |
| 7.    | [Flatten binary tree to linked list in preorder](#question-7-flatten-binary-tree-to-linked-list-in-preorder)                                |
| 8.    | [Check if tree is subtree of another tree](#question-8-check-if-tree-is-subtree-of-another-tree)                                            |
| 9.    | [All nodes at distance K from a given node](#question-9-all-nodes-at-distance-k-from-a-given-node)                                          |
| 10.   | [Diameter of a binary tree (optimized in single traversal)](#question-10-diameter-of-a-binary-tree-optimized-in-single-traversal)           |
| 11.   | [Minimum edges to reverse to reach target node](#question-11-minimum-edges-to-reverse-to-reach-target-node)                                 |
| 12.   | [Count number of connected components in undirected graph](#question-12-count-number-of-connected-components-in-undirected-graph)           |
| 13.   | [Shortest path in unweighted grid (4-direction)](#question-13-shortest-path-in-unweighted-grid-4-direction)                                 |
| 14.   | [Minimum number of moves to reach end in a grid with obstacles](#question-14-minimum-number-of-moves-to-reach-end-in-a-grid-with-obstacles) |
| 15.   | [Detect cycle in directed graph using DFS](#question-15-detect-cycle-in-directed-graph-using-dfs)                                           |
| 16.   | [Topological sort using Kahn's algorithm](#question-16-topological-sort-using-kahns-algorithm)                                              |
| 17.   | [Dijkstra's algorithm (weighted undirected graph)](#question-17-dijkstras-algorithm-weighted-undirected-graph)                              |
| 18.   | [Find articulation points and bridges in graph](#question-18-find-articulation-points-and-bridges-in-graph)                                 |
| 19.   | [Clone a directed graph](#question-19-clone-a-directed-graph)                                                                               |
| 20.   | [Graph bipartite check using BFS](#question-20-graph-bipartite-check-using-bfs)                                                             |

## Question 1. Find bottom-left value in a binary tree

# Find Bottom-Left Value in a Binary Tree

### Direct Answer

The **bottom-left value** of a binary tree is the value of the **leftmost node at the deepest level**.

A common interview solution is **Level Order Traversal (BFS)**, where we record the first node of every level. The first node of the last level is the answer.

---

# 1. Problem Understanding

Given a binary tree, return the value of the leftmost node in the last row.

### Example

```text
        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7
```

Levels:

```text
Level 0: 1
Level 1: 2, 3
Level 2: 4, 5, 6
Level 3: 7
```

Bottom-left value = **7**

---

# Approach 1: Level Order Traversal (BFS)

### Idea

Perform BFS level by level.

- At each level, the first node in the queue is the leftmost node.
- Store that value.
- Continue until all levels are processed.
- The last stored value is the answer.

### Steps

1. Push root into queue.
2. Process nodes level by level.
3. Save the first node's value of each level.
4. Return the saved value from the final level.

### JavaScript Code

```javascript
const findBottomLeftValue = (root) => {
  const queue = [root];
  let answer = root.val;

  while (queue.length) {
    const size = queue.length;

    answer = queue[0].val;

    for (let i = 0; i < size; i++) {
      const node = queue.shift();

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }

  return answer;
};
```

### Complexity Analysis

- Time: **O(n)**
- Space: **O(w)**

Where:

- `n` = number of nodes
- `w` = maximum width of tree

---

# Approach 2: DFS (Preorder with Depth Tracking)

### Idea

Traverse:

```text
Root → Left → Right
```

Keep track of:

- Maximum depth seen so far
- Value of first node encountered at that depth

Since we visit the left subtree before the right subtree, the first node encountered at a depth is automatically the leftmost node.

### JavaScript Code

```javascript
const findBottomLeftValue = (root) => {
  let maxDepth = -1;
  let answer = root.val;

  const dfs = (node, depth) => {
    if (!node) return;

    if (depth > maxDepth) {
      maxDepth = depth;
      answer = node.val;
    }

    dfs(node.left, depth + 1);
    dfs(node.right, depth + 1);
  };

  dfs(root, 0);

  return answer;
};
```

### Complexity Analysis

- Time: **O(n)**
- Space: **O(h)**

Where:

- `h` = tree height
- Worst case: **O(n)** (skewed tree)
- Balanced tree: **O(log n)**

---

# Approach 3: BFS Right-to-Left Trick

### Idea

A clever BFS variation:

- Process **right child before left child**.
- The last node visited during BFS becomes the bottom-left node.

### Example

```text
Queue order:
Root
Right child
Left child
...
```

The final processed node will be the leftmost node of the deepest level.

### JavaScript Code

```javascript
const findBottomLeftValue = (root) => {
  const queue = [root];
  let node = null;

  while (queue.length) {
    node = queue.shift();

    if (node.right) queue.push(node.right);
    if (node.left) queue.push(node.left);
  }

  return node.val;
};
```

### Complexity Analysis

- Time: **O(n)**
- Space: **O(w)**

---

# Edge Cases

### Single Node

```text
1
```

Output:

```text
1
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

Output:

```text
3
```

---

### Right-Skewed Tree

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

Only node at deepest level is also the leftmost node.

---

# Common Pitfalls

### Forgetting to save the first node of each level

Incorrect:

```javascript
answer = node.val;
```

inside the loop.

Correct:

```javascript
answer = queue[0].val;
```

before processing the level.

---

### DFS Traversing Right Before Left

```javascript
dfs(node.right);
dfs(node.left);
```

This may capture the wrong node at a depth.

Always traverse:

```javascript
dfs(node.left);
dfs(node.right);
```

---

# Interview Tips

If asked in an interview:

1. Start with **BFS level-order traversal** because it's intuitive.
2. Mention that the **first node of the last level** is the answer.
3. Then discuss the **DFS optimization** using depth tracking.
4. As a bonus, mention the **right-to-left BFS trick**, which is a neat one-pass solution often appreciated by interviewers.

**Most interview-friendly solution:** BFS Level Order Traversal — **O(n) time, O(w) space**.

## Question 2. Construct binary tree from inorder and postorder

# Construct Binary Tree from Inorder and Postorder Traversal

### Direct Answer

Given the **inorder** and **postorder** traversals of a binary tree with unique values, we can reconstruct the tree by:

1. The **last element of postorder** is always the root.
2. Find that root in the inorder array.
3. Elements left of the root in inorder belong to the left subtree.
4. Elements right of the root in inorder belong to the right subtree.
5. Recursively build the **right subtree first**, then the left subtree, because we're consuming postorder from the end.

---

# 1. Problem Understanding

### Example

```text
Inorder    = [9, 3, 15, 20, 7]
Postorder  = [9, 15, 7, 20, 3]
```

Tree:

```text
        3
       / \
      9   20
         /  \
        15   7
```

### Why?

- Postorder = Left → Right → Root
- Last element (`3`) is the root.
- In inorder:

```text
[9] 3 [15,20,7]
```

Left subtree:

```text
Inorder = [9]
```

Right subtree:

```text
Inorder = [15,20,7]
```

Recursively repeat.

---

# Approach 1: Recursive Construction with Hash Map (Optimal)

### Idea

Use:

- A pointer (`postIndex`) starting from the last element of postorder.
- A hash map to quickly locate each value in inorder.

### Key Observation

Since we're moving backward through postorder:

```text
Postorder (reverse):
Root → Right → Left
```

We must build:

```text
Right subtree first
Then left subtree
```

Otherwise nodes will be consumed in the wrong order.

---

# Algorithm

1. Create a map of value → inorder index.
2. Set `postIndex = postorder.length - 1`.
3. Create recursive function:
   - Take current root from `postorder[postIndex]`.
   - Decrement `postIndex`.
   - Find root position in inorder.
   - Build right subtree.
   - Build left subtree.

4. Return root.

---

# JavaScript Code (Optimal)

```javascript
class TreeNode {
  constructor(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}

const buildTree = (inorder, postorder) => {
  const indexMap = new Map();

  inorder.forEach((value, index) => {
    indexMap.set(value, index);
  });

  let postIndex = postorder.length - 1;

  const construct = (left, right) => {
    if (left > right) return null;

    const rootValue = postorder[postIndex--];
    const root = new TreeNode(rootValue);

    const inorderIndex = indexMap.get(rootValue);

    root.right = construct(inorderIndex + 1, right);
    root.left = construct(left, inorderIndex - 1);

    return root;
  };

  return construct(0, inorder.length - 1);
};
```

---

# Dry Run

### Input

```text
Inorder   = [9,3,15,20,7]
Postorder = [9,15,7,20,3]
```

### Step 1

```text
Root = 3
```

Inorder split:

```text
[9] | 3 | [15,20,7]
```

---

### Step 2

Build right subtree first:

```text
Root = 20
```

Split:

```text
[15] | 20 | [7]
```

---

### Step 3

Right child:

```text
7
```

Left child:

```text
15
```

---

### Step 4

Build left subtree of 3:

```text
9
```

Final tree:

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

# Approach 2: Without Hash Map

### Idea

Every time we choose a root:

- Search for it linearly in inorder.

### Code

```javascript
const buildTree = (inorder, postorder) => {
  let postIndex = postorder.length - 1;

  const construct = (left, right) => {
    if (left > right) return null;

    const rootValue = postorder[postIndex--];
    const root = new TreeNode(rootValue);

    let inorderIndex = left;

    while (inorder[inorderIndex] !== rootValue) {
      inorderIndex++;
    }

    root.right = construct(inorderIndex + 1, right);
    root.left = construct(left, inorderIndex - 1);

    return root;
  };

  return construct(0, inorder.length - 1);
};
```

---

# Complexity Analysis

## Optimal Hash Map Solution

### Time

Building map:

```text
O(n)
```

Tree construction:

```text
O(n)
```

Total:

```text
O(n)
```

### Space

Hash map:

```text
O(n)
```

Recursion stack:

```text
O(h)
```

Total:

```text
O(n)
```

Worst case (skewed tree):

```text
O(n)
```

Balanced tree:

```text
O(log n)
```

---

## Without Hash Map

### Time

Finding root index repeatedly:

```text
O(n)
```

for each node.

Total:

```text
O(n²)
```

### Space

```text
O(h)
```

---

# Edge Cases

### Empty Tree

```javascript
inorder = [];
postorder = [];
```

Output:

```text
null
```

---

### Single Node

```javascript
inorder = [1];
postorder = [1];
```

Output:

```text
1
```

---

### Left-Skewed Tree

```text
    3
   /
  2
 /
1
```

```javascript
inorder = [1, 2, 3];
postorder = [1, 2, 3];
```

---

### Right-Skewed Tree

```text
1
 \
  2
   \
    3
```

```javascript
inorder = [1, 2, 3];
postorder = [3, 2, 1];
```

---

# Common Pitfalls

### Building Left Before Right

Wrong:

```javascript
root.left = construct(...);
root.right = construct(...);
```

Because postorder is consumed from the end.

Correct:

```javascript
root.right = construct(...);
root.left = construct(...);
```

---

### Forgetting the Hash Map

Using linear search inside every recursive call degrades performance from:

```text
O(n)
```

to

```text
O(n²)
```

---

# Interview Tips

- Mention the traversal properties:
  - Inorder → Left, Root, Right
  - Postorder → Left, Right, Root

- Explain why the last postorder element is the root.
- Point out the critical detail:
  - **When traversing postorder backwards, construct the right subtree before the left subtree.**

- Use a hash map to achieve **O(n)** time complexity.

**Optimal Solution:** Hash Map + Recursive Construction using a postorder pointer → **O(n) time, O(n) space**.

## Question 3. Maximum sum path in a binary tree

## Question 4. Sum of nodes at odd levels minus even levels

## Question 5. Count all nodes in a complete binary tree efficiently

## Question 6. Check if binary tree is complete

## Question 7. Flatten binary tree to linked list in preorder

## Question 8. Check if tree is subtree of another tree

## Question 9. All nodes at distance K from a given node

## Question 10. Diameter of a binary tree (optimized in single traversal)

## Question 11. Minimum edges to reverse to reach target node

## Question 12. Count number of connected components in undirected graph

## Question 13. Shortest path in unweighted grid (4-direction)

## Question 14. Minimum number of moves to reach end in a grid with obstacles

## Question 15. Detect cycle in directed graph using DFS

## Question 16. Topological sort using Kahn's algorithm

## Question 17. Dijkstra's algorithm (weighted undirected graph)

## Question 18. Find articulation points and bridges in graph

## Question 19. Clone a directed graph

## Question 20. Graph bipartite check using BFS
