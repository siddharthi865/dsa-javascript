# Set 7

| S.No. | Question                                                                                                                  |
| ----- | ------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Detect and remove loop in a linked list](#question-1-detect-and-remove-loop-in-a-linked-list)                            |
| 2.    | [Check if a linked list is a palindrome](#question-2-check-if-a-linked-list-is-a-palindrome)                              |
| 3.    | [Add two numbers represented as linked lists](#question-3-add-two-numbers-represented-as-linked-lists)                    |
| 4.    | [Flatten a multilevel linked list](#question-4-flatten-a-multilevel-linked-list)                                          |
| 5.    | [Rotate a linked list by k nodes](#question-5-rotate-a-linked-list-by-k-nodes)                                            |
| 6.    | [Swap nodes in a linked list without swapping data](#question-6-swap-nodes-in-a-linked-list-without-swapping-data)        |
| 7.    | [Delete a node given only access to that node](#question-7-delete-a-node-given-only-access-to-that-node)                  |
| 8.    | [Merge k sorted linked lists](#question-8-merge-k-sorted-linked-lists)                                                    |
| 9.    | [Reorder a linked list (like L0 → Ln → L1 → Ln-1 → ...)](#question-9-reorder-a-linked-list-like-l0--ln--l1--ln-1--)       |
| 10.   | [Partition a linked list around a value x](#question-10-partition-a-linked-list-around-a-value-x)                         |
| 11.   | [Implement a stack that supports push, pop, and getMax](#question-11-implement-a-stack-that-supports-push-pop-and-getmax) |
| 12.   | [Next greater element for each element in an array](#question-12-next-greater-element-for-each-element-in-an-array)       |
| 13.   | [Implement a queue using two stacks](#question-13-implement-a-queue-using-two-stacks)                                     |
| 14.   | [Maximum of all subarrays of size k](#question-14-maximum-of-all-subarrays-of-size-k)                                     |
| 15.   | [Check for balanced parentheses in an expression](#question-15-check-for-balanced-parentheses-in-an-expression)           |
| 16.   | [Check if a tree is symmetric](#question-16-check-if-a-tree-is-symmetric)                                                 |
| 17.   | [Find all root-to-leaf paths in a binary tree](#question-17-find-all-root-to-leaf-paths-in-a-binary-tree)                 |
| 18.   | [Sum of all left leaves in a binary tree](#question-18-sum-of-all-left-leaves-in-a-binary-tree)                           |
| 19.   | [Count nodes with exactly one child](#question-19-count-nodes-with-exactly-one-child)                                     |
| 20.   | [Find the path with maximum sum from root to leaf](#question-20-find-the-path-with-maximum-sum-from-root-to-leaf)         |

## Question 1. Detect and remove loop in a linked list

# Detect and Remove Loop in a Linked List

## Concise Answer

The most efficient way to detect and remove a loop in a linked list is to use **Floyd's Cycle Detection Algorithm (Tortoise and Hare)**.

1. Detect if a cycle exists using slow and fast pointers.
2. Find the starting node of the loop.
3. Find the last node in the loop and set its `next` pointer to `null`.

**Time Complexity:** O(n)
**Space Complexity:** O(1)

---

# 1. Problem Understanding

Given a singly linked list, determine whether it contains a cycle (loop).

Example:

```text
1 → 2 → 3 → 4 → 5
          ↑     ↓
          ← ← ←
```

The node `5` points back to `3`, creating a loop.

After removal:

```text
1 → 2 → 3 → 4 → 5 → null
```

---

# 2. Approach 1: Floyd's Cycle Detection (Optimal)

## Step 1: Detect the Loop

Use two pointers:

- `slow` moves one step.
- `fast` moves two steps.

If they ever meet, a cycle exists.

```text
slow: 1 → 2 → 3 → 4
fast: 1 → 3 → 5 → 4

Meet at node 4
```

---

## Step 2: Find the Starting Node of the Loop

Once a meeting point is found:

- Move one pointer to `head`.
- Keep the other at the meeting point.
- Move both one step at a time.

The node where they meet again is the **start of the loop**.

### Why does this work?

Suppose:

```text
Distance from head to loop start = x
Loop length = y
```

When slow and fast meet:

```text
2 × distance(slow) = distance(fast)
```

This mathematical relationship guarantees that resetting one pointer to the head and moving both one step at a time leads them to the loop's starting node.

---

## Step 3: Remove the Loop

Once the loop start is known:

- Traverse the loop until reaching the node whose `next` points back to the loop start.
- Set its `next` to `null`.

---

# JavaScript Solution (Optimal)

```javascript
class ListNode {
  constructor(val) {
    this.val = val;
    this.next = null;
  }
}

const detectAndRemoveLoop = (head) => {
  if (!head || !head.next) return head;

  let slow = head;
  let fast = head;
  let hasLoop = false;

  // Detect loop
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow === fast) {
      hasLoop = true;
      break;
    }
  }

  if (!hasLoop) return head;

  // Find start of loop
  slow = head;

  while (slow !== fast) {
    slow = slow.next;
    fast = fast.next;
  }

  const loopStart = slow;

  // Find last node of loop
  let ptr = loopStart;

  while (ptr.next !== loopStart) {
    ptr = ptr.next;
  }

  // Remove loop
  ptr.next = null;

  return head;
};
```

---

# 3. Approach 2: Using HashSet

## Idea

Store visited nodes in a Set.

For every node:

- If already visited → loop detected.
- Break the loop by setting previous node's `next = null`.

---

## JavaScript

```javascript
const detectAndRemoveLoop = (head) => {
  const visited = new Set();

  let curr = head;
  let prev = null;

  while (curr) {
    if (visited.has(curr)) {
      prev.next = null;
      return head;
    }

    visited.add(curr);
    prev = curr;
    curr = curr.next;
  }

  return head;
};
```

---

## Complexity

| Operation | Time | Space |
| --------- | ---- | ----- |
| Detection | O(n) | O(n)  |
| Removal   | O(1) | O(n)  |

---

# Special Case: Loop Starts at Head

Example:

```text
1 → 2 → 3 → 4
↑         ↓
← ← ← ← ←
```

After Floyd detection:

```javascript
slow = head;
fast = meetingPoint;
```

Both pointers may already be at the loop start.

The previous algorithm still works correctly.

Example:

```javascript
1 -> 2 -> 3 -> 4
^              |
|______________|
```

The loop is removed by finding node `4` and setting:

```javascript
4.next = null;
```

---

# Improved Floyd Solution (Handles All Cases)

A common interview implementation:

```javascript
const detectAndRemoveLoop = (head) => {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow === fast) {
      break;
    }
  }

  if (!fast || !fast.next) {
    return head; // No loop
  }

  slow = head;

  if (slow === fast) {
    while (fast.next !== slow) {
      fast = fast.next;
    }
  } else {
    while (slow.next !== fast.next) {
      slow = slow.next;
      fast = fast.next;
    }
  }

  fast.next = null;

  return head;
};
```

**Why this version is popular:**
It finds the node immediately before the loop start and removes the loop without needing a separate traversal after locating the loop start.

---

# Complexity Analysis

### Floyd's Algorithm

| Operation   | Complexity |
| ----------- | ---------- |
| Detect loop | O(n)       |
| Find start  | O(n)       |
| Remove loop | O(n)       |
| Total       | O(n)       |
| Extra Space | O(1)       |

---

# Edge Cases

### 1. Empty List

```text
null
```

Output:

```text
null
```

---

### 2. Single Node Without Loop

```text
1 → null
```

No changes.

---

### 3. Single Node With Self Loop

```text
1
↑↓
```

After removal:

```text
1 → null
```

---

### 4. Loop Starts at Head

```text
1 → 2 → 3 → 4
↑         ↓
← ← ← ← ←
```

Must correctly break the link from `4`.

---

### 5. Loop in Middle

```text
1 → 2 → 3 → 4 → 5
          ↑     ↓
          ← ← ←
```

Remove:

```text
5.next = null
```

---

# Common Interview Pitfalls

1. Detecting the loop but not removing it.
2. Forgetting the case where the loop starts at the head.
3. Comparing node values instead of node references.
4. Using extra space when an O(1) solution is expected.
5. Losing the loop-start node while trying to remove the cycle.

---

# Interview Tip

When asked **"Detect and remove loop in a linked list"**, start with:

> "I'll use Floyd's Cycle Detection Algorithm to detect the loop in O(n) time and O(1) space. After detecting the cycle, I'll locate the start of the loop and disconnect the last node in the cycle by setting its next pointer to null."

This demonstrates knowledge of both **cycle detection** and **cycle removal**, which interviewers typically expect.

## Question 2. Check if a linked list is a palindrome

## Question 3. Add two numbers represented as linked lists

## Question 4. Flatten a multilevel linked list

## Question 5. Rotate a linked list by k nodes

## Question 6. Swap nodes in a linked list without swapping data

## Question 7. Delete a node given only access to that node

## Question 8. Merge k sorted linked lists

## Question 9. Reorder a linked list (like L0 → Ln → L1 → Ln-1 → ...)

## Question 10. Partition a linked list around a value x

## Question 11. Implement a stack that supports push, pop, and getMax

## Question 12. Next greater element for each element in an array

## Question 13. Implement a queue using two stacks

## Question 14. Maximum of all subarrays of size k

## Question 15. Check for balanced parentheses in an expression

## Question 16. Check if a tree is symmetric

## Question 17. Find all root-to-leaf paths in a binary tree

## Question 18. Sum of all left leaves in a binary tree

## Question 19. Count nodes with exactly one child

## Question 20. Find the path with maximum sum from root to leaf
