# Set 2

| S.No. | Question                                                                                                                                             |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.    | [Detect a cycle in a linked list](#question-1-detect-a-cycle-in-a-linked-list)                                                                       |
| 2.    | [Find the middle element of a linked list](#question-2-find-the-middle-element-of-a-linked-list)                                                     |
| 3.    | [Merge two sorted linked lists](#question-3-merge-two-sorted-linked-lists)                                                                           |
| 4.    | [Remove duplicates from a linked list](#question-4-remove-duplicates-from-a-linked-list)                                                             |
| 5.    | [Find the nth node from the end](#question-5-find-the-nth-node-from-the-end)                                                                         |
| 6.    | [Implement a stack using an array](#question-6-implement-a-stack-using-an-array)                                                                     |
| 7.    | [Implement a stack using a linked list](#question-7-implement-a-stack-using-a-linked-list)                                                           |
| 8.    | [Implement a queue using an array](#question-8-implement-a-queue-using-an-array)                                                                     |
| 9.    | [Implement a queue using a linked list](#question-9-implement-a-queue-using-a-linked-list)                                                           |
| 10.   | [Implement a circular queue](#question-10-implement-a-circular-queue)                                                                                |
| 11.   | [How do you evaluate a postfix (Reverse Polish Notation) expression?](#question-11-how-do-you-evaluate-a-postfix-reverse-polish-notation-expression) |
| 12.   | [How do you convert an infix expression to a postfix expression?](#question-12-how-do-you-convert-an-infix-expression-to-a-postfix-expression)       |
| 13.   | [Implement a min-stack that returns the minimum element in O(1)](#question-13-implement-a-min-stack-that-returns-the-minimum-element-in-o1)          |
| 14.   | [Implement a deque (double-ended queue)](#question-14-implement-a-deque-double-ended-queue)                                                          |
| 15.   | [How do you implement a stack using two queues?](#question-15-how-do-you-implement-a-stack-using-two-queues)                                         |
| 16.   | [What is a binary tree?](#question-16-what-is-a-binary-tree)                                                                                         |
| 17.   | [Difference between binary tree and binary search tree (BST)](#question-17-difference-between-binary-tree-and-binary-search-tree-bst)                |
| 18.   | [Implement inorder, preorder, and postorder traversal (recursive)](#question-18-implement-inorder-preorder-and-postorder-traversal-recursive)        |
| 19.   | [Implement inorder, preorder, and postorder traversal (iterative)](#question-19-implement-inorder-preorder-and-postorder-traversal-iterative)        |
| 20.   | [Find the height of a binary tree](#question-20-find-the-height-of-a-binary-tree)                                                                    |

## Question 1. Detect a cycle in a linked list

# Detect a Cycle in a Linked List

### Direct Answer

The most efficient way to detect a cycle in a linked list is **Floyd's Cycle Detection Algorithm (Tortoise and Hare)**.

- Use two pointers:
  - `slow` moves 1 step at a time.
  - `fast` moves 2 steps at a time.

- If there is a cycle, the two pointers will eventually meet.
- If `fast` reaches `null`, there is no cycle.

---

# 1. Problem Understanding

Given the head of a singly linked list, determine whether the list contains a cycle.

### Example

```
1 → 2 → 3 → 4
        ↑   ↓
        ← ← ←
```

Output:

```js
true;
```

---

# 2. Approach 1: Hash Set

### Idea

While traversing the list, store each visited node in a `Set`.

- If a node is encountered again, a cycle exists.
- Otherwise, continue until reaching `null`.

### Visualization

```
Visited:
1 → 2 → 3 → 4

If next node is 2 again:
Cycle detected
```

### JavaScript Code

```js
const hasCycle = (head) => {
  const visited = new Set();

  let current = head;

  while (current) {
    if (visited.has(current)) {
      return true;
    }

    visited.add(current);
    current = current.next;
  }

  return false;
};
```

### Complexity

| Metric | Value |
| ------ | ----- |
| Time   | O(n)  |
| Space  | O(n)  |

### Pros

- Easy to understand.
- Easy to implement.

### Cons

- Requires extra memory.

---

# 3. Approach 2: Floyd's Cycle Detection (Optimal)

### Idea

Use two pointers:

- `slow` moves one step.
- `fast` moves two steps.

If a cycle exists:

- Fast pointer eventually catches slow pointer.

If no cycle exists:

- Fast pointer reaches the end (`null`).

### Why Does It Work?

Suppose:

```
Slow: 1 step
Fast: 2 steps
```

Inside a cycle:

- Fast gains 1 node on slow every iteration.
- Eventually, fast and slow must meet.

### Visualization

```
Cycle:

1 → 2 → 3 → 4
      ↑     ↓
      ← ← ←

Iteration 1:
slow = 2
fast = 3

Iteration 2:
slow = 3
fast = 2

Iteration 3:
slow = 4
fast = 4   ← Meet
```

---

# JavaScript Code (Optimal)

```js
const hasCycle = (head) => {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow === fast) {
      return true;
    }
  }

  return false;
};
```

---

# Complexity Analysis

| Metric | Value |
| ------ | ----- |
| Time   | O(n)  |
| Space  | O(1)  |

This is the preferred interview solution because it uses constant extra space.

---

# Edge Cases

### Empty List

```js
head = null;
```

Output:

```js
false;
```

---

### Single Node Without Cycle

```
1 → null
```

Output:

```js
false;
```

---

### Single Node With Cycle

```
1
↑↓
```

Output:

```js
true;
```

---

### Cycle Starting at Head

```
1 → 2 → 3
↑       ↓
← ← ← ←
```

Output:

```js
true;
```

---

# Common Pitfalls

### Forgetting to Check `fast.next`

Incorrect:

```js
while (fast) {
  fast = fast.next.next; // Error
}
```

Correct:

```js
while (fast && fast.next)
```

---

### Comparing Node Values Instead of Nodes

Incorrect:

```js
if (slow.val === fast.val)
```

Different nodes can have the same value.

Correct:

```js
if (slow === fast)
```

---

# Follow-Up: Find the Starting Node of the Cycle

A very common interview follow-up:

After detecting the meeting point:

1. Move one pointer back to `head`.
2. Move both pointers one step at a time.
3. The node where they meet again is the start of the cycle.

This is the basis of the famous **Linked List Cycle II** problem.

---

# Interview Tip

When asked **"Detect a cycle in a linked list"**, immediately mention:

> "A HashSet solution works in O(n) time and O(n) space, but the optimal solution is Floyd's Tortoise and Hare algorithm, which runs in O(n) time and O(1) space."

This demonstrates awareness of both brute-force and optimal approaches, which interviewers typically look for.

## Question 2. Find the middle element of a linked list

## Question 3. Merge two sorted linked lists

## Question 4. Remove duplicates from a linked list

## Question 5. Find the nth node from the end

## Question 6. Implement a stack using an array

## Question 7. Implement a stack using a linked list

## Question 8. Implement a queue using an array

## Question 9. Implement a queue using a linked list

## Question 10. Implement a circular queue

## Question 11. How do you evaluate a postfix (Reverse Polish Notation) expression?

## Question 12. How do you convert an infix expression to a postfix expression?

## Question 13. Implement a min-stack that returns the minimum element in O(1)

## Question 14. Implement a deque (double-ended queue)

## Question 15. How do you implement a stack using two queues?

## Question 16. What is a binary tree?

## Question 17. Difference between binary tree and binary search tree (BST)

## Question 18. Implement inorder, preorder, and postorder traversal (recursive)

## Question 19. Implement inorder, preorder, and postorder traversal (iterative)

## Question 20. Find the height of a binary tree
