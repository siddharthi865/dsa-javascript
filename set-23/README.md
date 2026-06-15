# Set 23

| S.No. | Question                                                                                                                          |
| ----- | --------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Serialize and deserialize a binary tree](#question-1-serialize-and-deserialize-a-binary-tree)                                    |
| 2.    | [Check if two trees are mirror images](#question-2-check-if-two-trees-are-mirror-images)                                          |
| 3.    | [Construct tree from inorder and level order](#question-3-construct-tree-from-inorder-and-level-order)                            |
| 4.    | [Print all nodes at distance K from a given node](#question-4-print-all-nodes-at-distance-k-from-a-given-node)                    |
| 5.    | [Find maximum path sum in a binary tree](#question-5-find-maximum-path-sum-in-a-binary-tree)                                      |
| 6.    | [Count the number of paths with sum = K](#question-6-count-the-number-of-paths-with-sum--k)                                       |
| 7.    | [Convert BST to sorted doubly linked list](#question-7-convert-bst-to-sorted-doubly-linked-list)                                  |
| 8.    | [Check if binary tree is height-balanced](#question-8-check-if-binary-tree-is-height-balanced)                                    |
| 9.    | [Flatten binary tree to linked list in preorder](#question-9-flatten-binary-tree-to-linked-list-in-preorder)                      |
| 10.   | [Lowest common ancestor in a binary tree](#question-10-lowest-common-ancestor-in-a-binary-tree)                                   |
| 11.   | [Detect strongly connected components (Kosaraju / Tarjan)](#question-11-detect-strongly-connected-components-kosaraju--tarjan)    |
| 12.   | [Shortest path in weighted DAG](#question-12-shortest-path-in-weighted-dag)                                                       |
| 13.   | [Minimum edges to reverse to make path exist](#question-13-minimum-edges-to-reverse-to-make-path-exist)                           |
| 14.   | [Maximum flow in a network (Ford-Fulkerson / Edmonds-Karp)](#question-14-maximum-flow-in-a-network-ford-fulkerson--edmonds-karp)  |
| 15.   | [Bipartite graph check using DFS/BFS](#question-15-bipartite-graph-check-using-dfsbfs)                                            |
| 16.   | [Find all topological sorts of a DAG](#question-16-find-all-topological-sorts-of-a-dag)                                           |
| 17.   | [Count number of connected components](#question-17-count-number-of-connected-components)                                         |
| 18.   | [Detect cycle in directed graph using DFS recursion stack](#question-18-detect-cycle-in-directed-graph-using-dfs-recursion-stack) |
| 19.   | [Minimum spanning tree (Prim's and Kruskal's)](#question-19-minimum-spanning-tree-prims-and-kruskals)                             |
| 20.   | [Word ladder II (all shortest paths)](#question-20-word-ladder-ii-all-shortest-paths)                                             |

## Question 1. Serialize and deserialize a binary tree

# Serialize and Deserialize a Binary Tree

## Direct Answer

**Serialization** is the process of converting a binary tree into a string (or array) so it can be stored or transmitted.

**Deserialization** is the reverse process: reconstructing the original binary tree from that serialized representation.

A common interview solution uses **Preorder Traversal (Root → Left → Right)** and stores `null` markers for missing nodes.

---

# 1. Problem Understanding

Given a binary tree:

```
       1
      / \
     2   3
        / \
       4   5
```

Serialize it into something like:

```
1,2,N,N,3,4,N,N,5,N,N
```

where `N` represents a null node.

Then deserialize this string back into the exact same tree structure.

### Why store nulls?

Without null markers, different trees can produce the same traversal.

Example:

```
   1          1
  /            \
 2              2
```

Preorder without nulls:

```
1,2
```

Both trees look identical.

Therefore, null markers are required.

---

# 2. Approach 1: Preorder DFS (Most Common Interview Solution)

## Idea

### Serialization

Perform preorder traversal:

1. Visit root.
2. Serialize left subtree.
3. Serialize right subtree.
4. Store `"N"` for null nodes.

### Deserialization

Read values one by one:

1. If value is `"N"`, return null.
2. Create node.
3. Recursively build left subtree.
4. Recursively build right subtree.

---

## Example

Tree:

```
     1
    / \
   2   3
```

Serialized:

```
1,2,N,N,3,N,N
```

Deserialization:

```
1
├── left => 2
│   ├── N
│   └── N
└── right => 3
    ├── N
    └── N
```

Tree reconstructed successfully.

---

# JavaScript Solution (DFS)

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

// Serialize
const serialize = (root) => {
  const result = [];

  const dfs = (node) => {
    if (!node) {
      result.push("N");
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

  const dfs = () => {
    if (values[index] === "N") {
      index++;
      return null;
    }

    const node = new TreeNode(Number(values[index++]));

    node.left = dfs();
    node.right = dfs();

    return node;
  };

  return dfs();
};
```

---

# Complexity Analysis

### Serialization

- Time: **O(N)**
- Space: **O(N)**

### Deserialization

- Time: **O(N)**
- Space: **O(N)**

where `N` is the number of nodes.

---

# 3. Approach 2: Level Order Traversal (BFS)

Another valid approach uses a queue.

---

## Serialization

Perform level-order traversal and include null markers.

Example:

```
       1
      / \
     2   3
```

Serialized:

```
1,2,3,N,N,N,N
```

---

## JavaScript Solution (BFS)

### Serialize

```javascript
const serialize = (root) => {
  if (!root) return "N";

  const result = [];
  const queue = [root];

  while (queue.length) {
    const node = queue.shift();

    if (!node) {
      result.push("N");
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
const deserialize = (data) => {
  if (data === "N") return null;

  const values = data.split(",");
  const root = new TreeNode(Number(values[0]));

  const queue = [root];
  let index = 1;

  while (queue.length) {
    const node = queue.shift();

    if (values[index] !== "N") {
      node.left = new TreeNode(Number(values[index]));
      queue.push(node.left);
    }
    index++;

    if (values[index] !== "N") {
      node.right = new TreeNode(Number(values[index]));
      queue.push(node.right);
    }
    index++;
  }

  return root;
};
```

---

# Comparison of Approaches

| Approach          | Time | Space | Interview Preference |
| ----------------- | ---- | ----- | -------------------- |
| DFS (Preorder)    | O(N) | O(N)  | ⭐ Most common       |
| BFS (Level Order) | O(N) | O(N)  | Also acceptable      |

---

# Edge Cases

### Empty Tree

```
root = null
```

Serialized:

```
N
```

---

### Single Node

```
1
```

Serialized:

```
1,N,N
```

---

### Left Skewed Tree

```
1
/
2
/
3
```

Serialized:

```
1,2,3,N,N,N,N
```

---

### Right Skewed Tree

```
1
 \
  2
   \
    3
```

Serialized:

```
1,N,2,N,3,N,N
```

---

# Common Pitfalls

### 1. Forgetting null markers

This makes reconstruction impossible for many trees.

---

### 2. Using a local index during deserialization

The index must be shared across recursive calls.

Correct:

```javascript
let index = 0;
```

outside the recursive function.

---

### 3. Not converting strings back to numbers

```javascript
new TreeNode(Number(values[index]));
```

instead of:

```javascript
new TreeNode(values[index]);
```

if numeric values are expected.

---

# Interview Tips

- Start by explaining that **tree structure must be preserved**, so null markers are mandatory.
- Mention that **preorder + null markers uniquely identifies a binary tree**.
- State complexity upfront: **O(N) time and O(N) space**.
- If asked for an alternative, discuss the **BFS/Level Order approach**.
- The preorder DFS solution is the one most interviewers expect and is usually the cleanest implementation.

## Question 2. Check if two trees are mirror images

# Check if Two Trees are Mirror Images

## Direct Answer

Two binary trees are **mirror images** if:

1. Their root values are equal.
2. The left subtree of one tree is the mirror of the right subtree of the other.
3. The right subtree of one tree is the mirror of the left subtree of the other.

This can be efficiently checked using **recursion**.

---

# 1. Problem Understanding

### Example 1: Mirror Trees

```text
Tree 1              Tree 2

    1                  1
   / \                / \
  2   3              3   2
 / \                / \
4   5              5   4
```

These are mirror images.

---

### Example 2: Not Mirror Trees

```text
Tree 1              Tree 2

    1                  1
   / \                / \
  2   3              2   3
```

These are not mirrors because the structure is the same rather than reversed.

---

# 2. Approach 1: Recursive Solution (Most Common)

## Idea

Compare nodes pairwise:

- If both nodes are `null` → mirror.
- If one is `null` and the other isn't → not mirror.
- Values must match.
- Compare:
  - left of first tree with right of second tree
  - right of first tree with left of second tree

---

## Recursive Relation

If nodes are `a` and `b`:

```text
isMirror(a, b) =
    a.val === b.val &&
    isMirror(a.left, b.right) &&
    isMirror(a.right, b.left)
```

---

## JavaScript Solution

```javascript
class TreeNode {
  constructor(val) {
    this.val = val;
    this.left = null;
    this.right = null;
  }
}

const areMirror = (root1, root2) => {
  const check = (node1, node2) => {
    if (!node1 && !node2) return true;

    if (!node1 || !node2) return false;

    return (
      node1.val === node2.val &&
      check(node1.left, node2.right) &&
      check(node1.right, node2.left)
    );
  };

  return check(root1, root2);
};
```

---

## Dry Run

```text
Tree 1          Tree 2

    1              1
   / \            / \
  2   3          3   2
```

Compare:

```text
1 == 1 ✓

2 ↔ 2 ✓
3 ↔ 3 ✓

All mirrored comparisons succeed.
```

Result:

```javascript
true;
```

---

# Complexity Analysis

### Time Complexity

Every node is visited once.

```text
O(N)
```

where `N` is the number of nodes.

### Space Complexity

Recursive stack:

```text
O(H)
```

where `H` is the height of the tree.

- Balanced tree → `O(log N)`
- Skewed tree → `O(N)`

---

# 3. Approach 2: Iterative Using Queue

Useful when recursion depth may become large.

---

## Idea

Store node pairs that should be mirrors.

For every pair:

1. Both null → continue.
2. One null → false.
3. Values differ → false.
4. Push:
   - left of first with right of second
   - right of first with left of second

---

## JavaScript Solution

```javascript
const areMirror = (root1, root2) => {
  const queue = [[root1, root2]];

  while (queue.length) {
    const [node1, node2] = queue.shift();

    if (!node1 && !node2) continue;

    if (!node1 || !node2) return false;

    if (node1.val !== node2.val) return false;

    queue.push([node1.left, node2.right]);
    queue.push([node1.right, node2.left]);
  }

  return true;
};
```

---

# Complexity Analysis

### Time

```text
O(N)
```

### Space

```text
O(N)
```

for the queue in the worst case.

---

# Special Case: Check if a Tree is Symmetric

A common interview variation asks:

> Is a binary tree symmetric around its center?

Example:

```text
       1
      / \
     2   2
    / \ / \
   3  4 4  3
```

This is equivalent to checking whether the tree is a mirror of itself.

```javascript
const isSymmetric = (root) => {
  const check = (left, right) => {
    if (!left && !right) return true;
    if (!left || !right) return false;

    return (
      left.val === right.val &&
      check(left.left, right.right) &&
      check(left.right, right.left)
    );
  };

  return check(root.left, root.right);
};
```

---

# Edge Cases

### Both Trees Empty

```text
null
null
```

Output:

```javascript
true;
```

---

### One Empty, One Non-Empty

```text
null

  1
```

Output:

```javascript
false;
```

---

### Same Structure but Different Values

```text
Tree 1       Tree 2

  1            1
 /              \
2                3
```

Output:

```javascript
false;
```

---

### Mirror Structure but Different Node Values

```text
Tree 1       Tree 2

   1            1
  /              \
 2                5
```

Output:

```javascript
false;
```

---

# Interview Tips

- Clearly state the mirror condition:
  - `left ↔ right`
  - `right ↔ left`

- Mention that this is essentially a **simultaneous traversal of both trees**.
- The recursive solution is usually preferred because it directly models the mirror definition.
- If asked about symmetry of a single tree, explain that it's the same problem with `root.left` and `root.right`.
- Complexity: **O(N) time**, **O(H) recursive space**.

## Question 3. Construct tree from inorder and level order

## Question 4. Print all nodes at distance K from a given node

## Question 5. Find maximum path sum in a binary tree

## Question 6. Count the number of paths with sum = K

## Question 7. Convert BST to sorted doubly linked list

## Question 8. Check if binary tree is height-balanced

## Question 9. Flatten binary tree to linked list in preorder

## Question 10. Lowest common ancestor in a binary tree

## Question 11. Detect strongly connected components (Kosaraju / Tarjan)

## Question 12. Shortest path in weighted DAG

## Question 13. Minimum edges to reverse to make path exist

## Question 14. Maximum flow in a network (Ford-Fulkerson / Edmonds-Karp)

## Question 15. Bipartite graph check using DFS/BFS

## Question 16. Find all topological sorts of a DAG

## Question 17. Count number of connected components

## Question 18. Detect cycle in directed graph using DFS recursion stack

## Question 19. Minimum spanning tree (Prim's and Kruskal's)

## Question 20. Word ladder II (all shortest paths)
